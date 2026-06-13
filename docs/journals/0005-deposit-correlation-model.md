# 0005 - Deposit Correlation Model

## Purpose

This journal defines the first deposit-correlation model for Graffle.

The core problem is that multiple witnesses may observe the same guild-bank money-log entry, but WoW does not provide a durable external transaction id that Graffle can blindly rely on.

The Hosted Core must decide whether observations refer to:

- the same underlying deposit
- separate deposits
- an ambiguous case requiring review

## Design Preference

Graffle prefers honest ambiguity over false certainty.

The system should not merge observations merely because doing so is convenient.

The system should also not split observations merely because they arrived from different witnesses.

Correlation must be explicit, reconstructable, and conservative.

## Inputs Available From an Observation

A deposit observation may include:

- guild identity
- observer identity
- observed player name
- transaction type
- amount in copper
- relative age fields reported by WoW
- captured client timestamp
- source client/addon version
- guild-bank log index if available
- local observation sequence if available

Some of these inputs are stronger than others.

The observed player, transaction type, amount, and approximate time relationship are likely the most important early fields.

## Non-Authority Fields

The following fields must not become authority by themselves:

- witness identity
- local client time
- local sequence number
- guild-bank log index

These fields can help correlation, but they do not prove identity of the underlying deposit alone.

## Candidate Correlation Key

An initial candidate correlation key may be based on:

```text
guild
transaction type
observed player
amount
approximate observed time window
```

This key is not a final transaction id.

It is a candidate grouping hint.

## Time Window Problem

WoW money-log entries expose age-like information rather than a perfect timestamp.

Different witnesses may observe the same deposit at different times.

Example:

```text
Witness A sees Aaron deposited 1000g at age 0 hours.
Witness B sees Aaron deposited 1000g at age 0 hours.
Witness C sees Aaron deposited 1000g at age 1 hour.
```

These may describe one deposit.

But if Aaron deposited 1000g twice during the same period, similar observations may describe multiple deposits.

## Conservative Matching Rule

The Hosted Core may correlate observations when they are compatible across:

- same guild
- same transaction type
- same observed player
- same amount
- compatible relative age / captured time relationship

The Hosted Core must avoid automatically merging when there is evidence of multiple same-player same-amount deposits within the same plausible window.

## Ambiguity Rule

If observations could reasonably represent either one deposit or multiple deposits, the system must preserve ambiguity.

The result should be:

```text
Needs Review
```

or:

```text
Unresolved Correlation
```

not a fabricated clean ledger entry.

## Supporting Observation Set

A canonical ledger entry should retain all observations that support it.

Conceptual shape:

```text
LedgerEntry
- canonical ledger entry id
- guild
- transaction type
- observed player
- amount
- accepted time window
- supporting observations
- correlation decision
- correlation reason
```

The supporting observations allow future audit.

## Duplicate Deposit Risk

The highest-risk case is repeated same-player same-amount deposits close together.

Example:

```text
Aaron deposits 1000g at 8:00
Aaron deposits 1000g at 8:03
```

Witnesses later report:

```text
Aaron deposited 1000g
Aaron deposited 1000g
```

The Hosted Core must not blindly collapse these into one event.

For raffles, this matters because duplicate ticket deposits may imply multiple intended entries later, or may be accidental depending on raffle rules.

## Initial Raffle Interaction

For early Graffle raffles, the safest rule is:

```text
One eligible entry requires one confirmed intent and one matched ledger entry.
```

Multiple deposits do not automatically create multiple eligible entries unless the raffle explicitly supports multiple tickets per member.

## Decision States

Correlation decisions should have explicit states:

```text
Candidate
Accepted
Ambiguous
Rejected
Superseded
```

### Candidate

Observations appear related but are not yet final.

### Accepted

The Hosted Core has accepted the observations as describing one canonical ledger entry.

### Ambiguous

The observations are compatible with more than one interpretation.

### Rejected

The observations are incompatible or invalid.

### Superseded

A previous correlation decision was replaced by a more complete decision while preserving audit history.

## Important Non-Goal

This model does not require perfect real-time certainty.

Graffle's goal is not to know everything instantly.

Graffle's goal is to preserve evidence early enough that later arbitration can produce explainable decisions.

## Deferred

- exact time-window math
- exact duplicate detection algorithm
- witness trust scoring
- local AddOn capture format
- hosted API payload format
- admin review workflow
- multi-ticket raffle behavior
