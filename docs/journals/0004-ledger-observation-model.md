# 0004 - Ledger Observation Model

## Purpose

This journal defines how Graffle thinks about observations.

The system preference is eventual correctness through multiple witnesses rather than immediate correctness through a single privileged actor.

## Authority Structure

Guild Bank
- source authority

Witnesses
- observers

Hosted Core
- arbitrator

Consumers
- raffle
- web
- Discord
- reporting

## Observation Philosophy

An observation is not truth.

An observation is evidence that may contribute to truth.

Graffle should prefer:

- multiple supporting observations
- durable storage
- reconstructable decisions

instead of:

- hidden assumptions
- officer memory
- manual spreadsheets

## Observation Record

Conceptual shape:

```text
Observation
- observer
- observed player
- transaction type
- amount
- observed age information
- captured at
- client metadata
```

Observations should remain immutable once accepted.

Corrections should create new observations or arbitration outcomes rather than rewriting history.

## Swarm Model

Many clients may observe the same guild-bank entry.

Example:

Witness A sees:
- Aaron deposited 1000g

Witness B sees:
- Aaron deposited 1000g

Witness C later sees:
- Aaron deposited 1000g

The Hosted Core may decide these observations refer to the same underlying event.

## Arbitration Goal

The Hosted Core should answer:

```text
Do these observations describe the same deposit?
```

not:

```text
Which witness do I trust most?
```

The preferred model is evidence convergence.

## Canonical Ledger Entry

A canonical ledger entry represents an accepted deposit-like event.

A ledger entry should preserve:

- supporting observations
- arbitration reasoning
- acceptance time

This allows future audit and review.

## Raffle Relationship

Raffles do not consume observations directly.

Raffles consume canonical ledger entries.

This separation prevents raffle rules from becoming entangled with observation handling.

## Long-Term Goal

The guild-bank log is ephemeral.

Graffle's ledger should become more complete over time than any individual WoW client can observe alone.

The system succeeds when historical raffle eligibility remains explainable even after the original guild-bank log entries have disappeared from WoW.
