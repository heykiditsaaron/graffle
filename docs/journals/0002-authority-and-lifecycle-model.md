Authority Model

Guild Bank
- Source authority for deposits

Witness
- Observes guild-bank information
- Cannot determine eligibility

Hosted Core
- Arbitrates observations
- Produces canonical ledger entries
- Determines eligibility

Raffle
- Consumes ledger entries
- Produces draw results

Publisher
- Read-only consumer

Lifecycle Model

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
Winner (optional)


Deposit Alone != Entry
Intent Alone != Eligibility
Intent + Matching Deposit = Eligible
