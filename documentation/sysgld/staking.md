# Staking SGLD

Stake SGLD for a fixed term and receive non-transferable sySGLD representing your net staked principal. Funded yield is tracked separately and paid in SGLD.

> **Before deployment:** These mechanics describe the current contract implementation. Staking contracts are not yet deployed or accepting deposits. The [staking interface](https://app.syncrate.org/stake) is a preview. Independent security review, deployment verification and application integration remain before activation.

## Staking parameters

| Parameter                     | Current design                                                       |
| ----------------------------- | -------------------------------------------------------------------- |
| Network                       | Base                                                                 |
| Deposit and yield asset       | SGLD                                                                 |
| Receipt                       | Non-transferable sySGLD                                              |
| Terms                         | 30 days or 90 days, separately recorded for each deposit             |
| Minimum deposit               | 1 SGLD before the deposit fee                                        |
| Deposit fee                   | 0.25% of the gross SGLD deposit                                      |
| Receipt issuance              | 1 sySGLD per 1 SGLD of net principal after the deposit fee           |
| Redemption fee                | 0.50% of the position's principal; yield is not charged              |
| Withdrawal                    | Complete positions only, at or after maturity; no early exit         |
| Yield claims                  | Available after position maturity, once the relevant yield is funded |
| Direct restaking              | Fee-free, explicitly selected after maturity                         |
| Automatic renewal/compounding | Neither is implemented                                               |

## Fees and what you receive

The deposit fee is deducted from the amount entering the staking system. The net amount becomes your principal and determines both your sySGLD receipt balance and your time-weighted reward allocation.

| Example: 100 SGLD deposit      |                    Amount |
| ------------------------------ | ------------------------: |
| Gross SGLD deposited           |                  100 SGLD |
| Deposit fee: 0.25%             |                 0.25 SGLD |
| Net principal and receipts     | 99.75 SGLD / 99.75 sySGLD |
| Redemption fee: 0.50% of 99.75 |              0.49875 SGLD |
| Principal returned at maturity |             99.25125 SGLD |

Funded yield is added on top. If this position has 1 SGLD of funded yield, redeeming principal and claiming that yield together pays **100.25125 SGLD**. Claiming yield separately produces the same fees; there is no extra fee on late-interest claims.

The minimum is applied to the gross deposit. A **1 SGLD** deposit therefore creates **0.9975 sySGLD**, representing 0.9975 SGLD net principal. That matured position can still be directly restaked without a fee or a new gross-deposit minimum check.

Both fees are paid in SGLD to the existing Syncrate fee-recipient address:

`0x42f0E74872c77939422f3c6cEb9D62984BaE5b69`

This is a **fee recipient, not an address to send staking deposits to manually**. Fee rates and the recipient are fixed constants in this implementation. Fees round down at raw token precision; transaction quotes should show the exact amount, along with network gas costs.

## Intended staking flow after activation

1. Open [app.syncrate.org/stake](https://app.syncrate.org/stake) and connect your wallet on Base.
2. Select a 30-day or 90-day term and enter at least 1 SGLD within your available balance.
3. Review the gross deposit, deposit fee, net principal, sySGLD amount and maturity time.
4. Approve the gross SGLD amount for the verified staking contract, then confirm the staking transaction in your wallet. Approval alone does not create a position.
5. After confirmation, the contract transfers the fee to treasury, records your net principal and maturity, and mints the corresponding sySGLD.

Each deposit creates a separate position. Adding a deposit does not reset an older position's lock. Do not send SGLD directly to the contract address: a normal token transfer creates neither a staking position nor a receipt, and this version has no recovery function for unsupported/direct transfers.

## Maturity and redemption

A position cannot be redeemed before its own 30-day or 90-day maturity. At or after maturity, its owner may withdraw the complete principal position. Partial principal withdrawals are not implemented in this version.

Redemption burns the position's full sySGLD receipt amount and pays the principal less the 0.50% redemption fee. Any selected funded yield is paid in full. The redemption fee applies once to the principal being withdrawn, not to the yield or to the number of claims.

A withdrawal can collect funded rewards from up to 32 earning periods in one transaction. Longer histories can be claimed in additional transactions. The interface must assemble the relevant periods; users should not need to calculate their allocation manually.

You do not need to wait for interest funding to recover matured principal: redeem principal alone and claim late-funded interest separately. Normal contract checks and the underlying SGLD token/network still apply; maturity is not a guarantee that external dependencies cannot interrupt transactions.

The [SGLD minting and redemption process](../sgld/minting-and-redemption.md) concerns exchanging SGLD for supported stablecoins. Its fee schedule and FIFO queue are separate from sySGLD staking redemption, which returns SGLD.

## Restaking

After maturity, explicitly roll a complete position into a new 30-day or 90-day term. Direct restaking keeps principal in the contract and charges **neither the deposit fee nor the redemption fee**. It does not add yield to principal automatically.

The old position closes and the new position receives its own start and maturity times. The principal receipt amount stays the same. The old position's late-interest rights remain available separately.

Earning eligibility ends at the old maturity. If you wait before restaking, the gap earns no additional yield. There is no automatic renewal.

## Receipt restrictions and yield display

sySGLD cannot be transferred between wallets, including through delegated ERC-20 transfers after approval. It is not intended for ordinary token trading, LP deposits or lending collateral. Contract wallets may own positions; non-transferability is not an absolute guarantee against third-party wrappers of contract-owned positions.

Your receipt balance represents principal only. Yield should be displayed separately as funded-but-locked, claimable or already claimed. Actual yield cannot be claimed before maturity, and offchain interest not yet funded into the contract is not an onchain claimable balance.

See [Yield & Risks](yield-and-risks.md) for the funding, allocation and late-payment mechanics. Review the [terms and disclosures](https://www.syncrate.org/terms) before participating.
