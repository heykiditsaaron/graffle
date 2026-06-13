# 0003 - Domain Model

## Purpose

This journal defines the first stable domain vocabulary for Graffle before implementation begins.

The goal is to prevent the early system from collapsing distinct concepts into one vague "raffle entry" shape.

## Core Principle

Graffle separates intent, observation, ledger truth, eligibility, and publication.

A member is not eligible because they clicked a button.
A member is not eligible because a witness observed a deposit.
A member is eligible only when the Hosted Core accepts a valid intent and a matching deposit under the raffle rules.

## Domain Objects

### Guild

Represents the World of Warcraft guild context Graffle operates within.

Fields to define later:

- guild id or realm-qualified guild identity
- guild name
- realm
- region
- configured admin ranks
- settings

The guild is the social boundary for raffles, witnesses, and ledger entries.

### Raffle

Represents a single raffle event.

Fields to define later:

- raffle id
- guild id
- title
- status
- ticket price
- prize configuration
- created by
- opened at
- closed at
- drawn at

The raffle owns its rules, lifecycle, and draw result.

The raffle does not own payment truth.
Payment truth comes from the ledger.

### Prize

Represents what the winner receives.

Initial prize types:

- pot percentage
- fixed gold
- item

A 50/50 raffle is a pot-percentage prize where the winner receives 50% of the accepted pot and the guild bank retains the remainder.

Prize delivery is not yet modeled as an automatically verified event.

### EntryIntent

Represents a member intentionally joining a raffle.

Fields to define later:

- entry intent id
- raffle id
- character identity
- account/member identity if available later
- confirmed at
- source client
- status

Entry intent is required for eligibility.

A deposit without intent must not create eligibility by itself.

### DepositObservation

Represents a witness report that a guild-bank money-log entry was observed.

Fields to define later:

- observation id
- guild id
- observer identity
- observed player name
- amount
- transaction type
- relative age fields from WoW
- captured client time
- source addon version
- raw index if available

A deposit observation is a claim from a witness.
It is not canonical ledger truth by itself.

### LedgerEntry

Represents a canonical deposit-like fact accepted by the Hosted Core after arbitration.

Fields to define later:

- ledger entry id
- guild id
- transaction type
- player identity
- amount
- accepted time range
- supporting observations
- confidence or decision state

A ledger entry is produced by the Hosted Core.
AddOns and bridges may not directly create canonical ledger entries.

### EligibilityDecision

Represents the Hosted Core's decision about whether a member is eligible for a raffle.

Fields to define later:

- eligibility decision id
- raffle id
- entry intent id
- ledger entry id if matched
- character identity
- state
- reason
- decided at

Eligibility is derived from rules.
It is not manually declared by officers in normal operation.

### DrawResult

Represents the winner selection for a closed raffle.

Fields to define later:

- draw result id
- raffle id
- eligible entry set
- winner
- backup winners if supported
- draw method
- drawn at
- drawn by or requested by
- audit data

The draw must operate only on eligible entries accepted by the Hosted Core.

### Publication

Represents a readable output derived from the Hosted Core.

Examples:

- public web view
- Discord post
- audit export

Publication surfaces are read-only consumers.
They must not become authority for deposits, eligibility, or draw results.

## Foundational Rules

- Deposit alone is not an entry.
- Intent alone is not eligibility.
- Observation alone is not ledger truth.
- Ledger truth is produced by the Hosted Core.
- Eligibility is produced by the Hosted Core.
- Raffle draw operates only on eligible entries.
- Publisher output is derived from existing truth and creates no new truth.

## Initial Lifecycle

Member-facing lifecycle:

```text
Not Entered
↓
Intent Confirmed
↓
Awaiting Deposit
↓
Deposit Matched
↓
Eligible
↓
Drawn / Not Drawn
```

Problem states:

```text
Deposit Without Intent
Wrong Amount
Ambiguous Deposit
Deposit After Close
Observation Pending Arbitration
Needs Review
```

## Deferred

- exact identity mapping across alts/accounts
- exact WoW guild-bank API capture fields
- deduplication algorithm
- hosted API schema
- Discord/web publication format
- UI layout
