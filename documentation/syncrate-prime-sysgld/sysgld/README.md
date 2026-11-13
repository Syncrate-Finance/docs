# sySGLD

sySGLD is Syncrate's yield-bearing gold staking receipt. Users stake SGLD for a fixed term of **30 or 90 days**, while the physical gold backing their staked SGLD participates in direct leasing to vetted bullion banks and established counterparties. Lease interest is the source of staking yield.

> **Before deployment:** This page describes the current staking contract design. The staking contracts are not yet deployed or open for deposits. The staking interface is a preview; this documentation is not an announcement that staking is live.

## How sySGLD works

1. Choose a 30-day or 90-day term and deposit at least **1 SGLD before fees**.
2. A **0.25% deposit fee** is sent to treasury. The remaining SGLD is recorded as your principal, and you receive sySGLD **1:1 against that net principal**.
3. Your eligible principal and time staked determine your share of funded SGLD yield. Eligibility stops at maturity unless you explicitly start another term.
4. At maturity, redeem the complete position: its sySGLD is burned, and principal is returned less a **0.50% redemption fee**. Funded yield is paid on top without a yield fee.

For example, depositing **100 SGLD** produces **99.75 sySGLD**. Redeeming that position after maturity returns **99.25125 SGLD principal**, plus any funded yield selected for payment. A minimum deposit of 1 SGLD produces 0.9975 sySGLD.

## Principal and yield are separate

Your sySGLD balance represents principal. It does not grow automatically as interest arrives. Yield is recorded separately and paid in **SGLD**, not by increasing the sySGLD balance or its redemption ratio.

Interest must actually be funded onchain before it can be claimed. If interest is paid after you withdraw principal, your historical position retains its entitlement and you can claim the late-funded SGLD separately.

Direct restaking of matured principal into another term is fee-free. Standalone yield claims, including late claims, are also fee-free. There is no automatic renewal or automatic compounding.

## Transfer restrictions

sySGLD is **non-transferable**. It is not intended for ordinary trading, transfers between wallets, Uniswap liquidity or lending collateral. ERC-20 approvals do not allow a delegated transfer to bypass this restriction. Each staking position remains associated with its original owning wallet.

## SGLD, sySGLD and SYNC

| Asset  | Purpose                                                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| SGLD   | Syncrate's gold-backed token. Holding SGLD alone does not earn staking yield.                                                   |
| sySGLD | A non-transferable receipt for net SGLD principal committed to fixed-term staking; funded yield is paid separately in SGLD.     |
| SYNC   | The protocol token described in [SYNC Tokenomics](../../sync/sync-tokenomics.md), with a separate allocation and utility model. |

The SYNC points program and staking yield are separate. The Sunday points schedule does not establish a staking-yield payout schedule, and this staking design does not itself add new points eligibility.

## Learn more

* [Staking SGLD](staking.md): fees, minimum deposit, fixed terms, withdrawal and restaking mechanics.
* [Yield & Risks](yield-and-risks.md): how cash interest becomes SGLD rewards, time-weighted allocation, late payments and risks.
* [Staking interface](https://app.syncrate.org/stake): preview the experience; deposits remain disabled before activation.

Returns depend on actual financing performance and funding, are variable and are not guaranteed. Participation remains subject to eligibility requirements, jurisdiction restrictions and [applicable terms](https://www.syncrate.org/terms).
