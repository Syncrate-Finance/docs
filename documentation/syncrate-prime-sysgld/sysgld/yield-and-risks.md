# Yield & Risks

sySGLD yield comes from direct leasing of the physical gold backing staked SGLD to vetted bullion banks and established counterparties. They pay lease interest; treasury converts collected cash interest into backed SGLD and funds the staking system.

> **Before deployment:** This page explains the current staking implementation, not a live yield distribution service. Staking contracts are not yet deployed or accepting deposits. Independent security review and application/operational integration remain before activation.

Holding SGLD alone does not earn staking yield. SYNC incentives, points distributions and token-market purchases are separate from the gold-leasing yield source.

## From cash interest to SGLD rewards

1. **Counterparty payment:** Syncrate receives cash interest and identifies the period in which it was earned.
2. **Conversion:** Treasury obtains backed SGLD through the established mint/purchase process and its reserve controls. The staking contract does not mint SGLD to manufacture interest.
3. **Earning period:** An authorized funder creates the next elapsed accounting period, also called an epoch. Periods are contiguous from deployment, cannot end in the future and can cover at most 90 days each.
4. **Allocation:** Historical position weights are calculated onchain in bounded batches. Funding is accepted only after that period's calculation is complete and it has eligible stake.
5. **Funding:** Treasury approves and transfers the actual SGLD amount using the contract's funding function. The contract checks the amount received and reserves it for that period's participants.
6. **Claim:** After a position matures, its owner may claim its funded allocation, with principal redemption or separately.

This is an explicit funding process. A bank payment, the passing of time or an ordinary SGLD transfer to the contract does not automatically create rewards. Direct token transfers are not recognized as funded distributions and have no recovery mechanism in this version.

Treasury must attribute late payments to the period in which they were earned, rather than reward only the wallets present when cash arrives. The contract proves that SGLD was funded and calculates allocations from its position history; it cannot verify bank statements, physical gold custody or offchain payment attribution.

## Time-weighted allocation

Each position's allocation depends on its **net principal after the deposit fee** and the time it was eligible during the earning period:

```
Position weight = net SGLD principal × eligible seconds
Eligible time starts at deposit and stops at position maturity.

Cumulative entitlement = floor(
  total SGLD funded for the period × position weight / total period weight
)

Outstanding entitlement = cumulative entitlement − previously claimed yield
```

The 30-day and 90-day terms use the same per-unit, per-time weighting. A longer term can participate in more eligible time; it does not receive a separate higher rate multiplier. Deposits made after a period ends cannot collect its earlier yield. A position left idle after maturity earns no additional weight unless explicitly restaked into a new term.

### Allocation example

Assume a 30-day earning period, two 100 SGLD gross deposits and 3 SGLD of funded yield. After the 0.25% deposit fee, each position has 99.75 SGLD net principal.

| Position                       | Eligible time in this period | Weight in SGLD-days | Allocated yield |
| ------------------------------ | ---------------------------: | ------------------: | --------------: |
| Alice: deposited at the start  |                      30 days |             2,992.5 |          2 SGLD |
| Bob: deposited halfway through |                      15 days |            1,496.25 |          1 SGLD |

Alice has twice Bob's weight for this period. Each allocation becomes claimable only after that position's own maturity. For example, Bob's 30-day term beginning on day 15 matures on day 45, not at the day-30 accounting cutoff. This example illustrates allocation, not a promised rate.

## Late interest and additional funding

Principal withdrawal does not erase a position's history. If you redeem matured principal before treasury funds the relevant interest, that closed position keeps its reward entitlement. You can claim the late-funded SGLD afterward, even with no sySGLD left in your wallet. Restaking also preserves the old position's claims.

A period can be funded more than once. The contract recalculates cumulative entitlement and subtracts what has already been claimed. If your share was 2 SGLD and additional funding raises it to 3 SGLD, you can claim the additional 1 SGLD. The same entitlement cannot be paid twice.

Yield claims and treasury reward funding have no staking fee. The 0.50% redemption fee applies only to principal, whether yield is claimed together or later. See [Staking SGLD](staking.md) for the full deposit/redemption fee example.

## What your dashboard should show

| Value                | Meaning                                                          |
| -------------------- | ---------------------------------------------------------------- |
| sySGLD balance       | The principal receipt; it does not grow automatically with yield |
| Funded yield, locked | Allocated funded yield for a position not yet mature             |
| Claimable yield      | Funded entitlement available after position maturity             |
| Yield claimed        | SGLD already transferred to your wallet                          |

These are the intended distinctions for the frontend integration, not a claim that live balances or yield history are already connected. Interest not yet funded onchain is not a guaranteed claimable balance. Yield is paid in SGLD rather than by rebasing sySGLD or increasing a share-conversion ratio.

## Timing, rates and processing

There is no guaranteed yield rate or automatic staking-yield payout day. The Sunday SYNC points schedule is a separate program. Funding depends on actual collection, conversion, period attribution and onchain transactions.

Allocation is permissionless to process but still needs transactions: each batch handles up to 100 historical positions, and funding waits for the period to be finalized. Total processing work grows with historical positions. Claims collect up to 32 periods per transaction, with additional calls for longer histories. An operator/keeper process and monitoring are needed before launch; these actions do not happen automatically merely because a period ends.

Historical trailing-30-day performance should derive from authoritative earning-period and funded-distribution records, not from when a bank payment happened to arrive. Any APY requires clearly disclosed annualization/compounding assumptions. The contract does not automatically reinvest yield, and historical returns do not guarantee future performance. No rate should be shown as actual history before verified data exists.

## Custody and risks

The contract separately accounts for outstanding principal and unclaimed funded rewards, and requires its SGLD balance to cover both. There is no administrative function to sweep those balances. Integer-rounding dust remains reserved. These accounting controls do not eliminate the following risks:

* **Counterparty risk:** a lessee may pay late or fail to return leased gold or interest. Only funded SGLD can be paid as contract rewards.
* **Fixed-term and liquidity risk:** there is no early-exit right. sySGLD is non-transferable and is not a normal secondary-market or lending asset. Maturity does not eliminate external settlement risks.
* **Custody and operational risk:** gold handling, custody arrangements, treasury cash conversion, legal agreements, correct payment attribution and keeper/funding operations depend on service providers and operators.
* **Market risk:** SGLD yield remains exposed to changes in gold's market value. Receiving more SGLD does not guarantee a positive fiat-currency return.
* **Smart-contract and network risk:** defects, compromised administrative roles, blockchain disruption or incompatible changes to the underlying upgradeable SGLD token may interrupt staking, exits or claims. Pausing new deposits is designed to preserve matured exits and claims, but an underlying SGLD pause can still block token transfers.
* **Implementation status:** tests are not an independent audit, and the staking contracts have not been deployed or independently audited. Verify official deployment information and final transaction details before any future participation.

Loss is possible. Vetting counterparties and verifying reserves do not guarantee financing performance or reward funding.

## Verification and access

[SGLD asset verification](../../sgld/asset-verification.md) concerns evidence about gold reserves. It is not evidence of staking yield, lessee repayment, cash interest collection or available reward funding.

Access is subject to eligibility, applicable jurisdiction restrictions and [product terms](https://www.syncrate.org/terms). Businesses seeking a gold-leasing facility can use the separate [business inquiry page](https://www.syncrate.org/sysgld).

For technical review, see the [staking implementation and tests](https://github.com/Syncrate-Finance/syncrate-sgld/pull/1). This is a draft implementation reference, not a verified deployed contract address.
