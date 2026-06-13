Project: Graffle

Purpose:
Create a guild raffle system for World of Warcraft that automatically
determines raffle eligibility through observed guild-bank deposits.

Core Principles:
- Guild Bank is the source truth.
- AddOn clients are witnesses.
- Hosted Core is the arbitration authority.
- Raffle eligibility is derived, not manually declared.
- Discord and web views are publication surfaces, not authorities.
- Eventual correctness is preferred over assumed correctness.

Initial Motivation:
The guild-bank money log is authoritative but ephemeral. Deposits roll
off the visible log quickly, making historical raffle verification
difficult. Graffle exists to preserve and arbitrate that truth.
