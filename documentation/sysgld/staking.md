# Staking SGLD

The current sySGLD design uses fixed-term SGLD staking.

**Staking is coming soon. Deposits are not yet open.**

## Staking parameters

| Parameter                        | Current design                                               |
| -------------------------------- | ------------------------------------------------------------ |
| Asset staked                     | SGLD                                                         |
| Asset received                   | sySGLD                                                       |
| Network shown in the staking app | Base                                                         |
| Terms                            | 30 days or 90 days                                           |
| Minimum stake                    | 1 SGLD                                                       |
| Yield source                     | Interest from direct leasing of the gold backing staked SGLD |
| Published yield rate             | Not yet available                                            |
| SGLD-to-sySGLD conversion        | Not yet specified                                            |

Receiving sySGLD does not establish a 1:1 conversion rate. The final conversion and accounting rules will determine the amount received.

## Explore the staking interface

1. Open [Stake SGLD](https://app.syncrate.org/stake).
2. Connect a compatible wallet on Base to view your available SGLD balance.
3. Select a 30-day or 90-day term.
4. Enter at least 1 SGLD, within your available balance.

These steps let you explore the interface. The staking control remains disabled, so entering an amount does not transfer funds, mint sySGLD or start a term.

## Maturity and withdrawals

The current design specifies fixed terms. The detailed maturity settlement, claim process, early-withdrawal policy and fees have not yet been specified. Do not assume that sySGLD can be redeemed at any time or that a term automatically renews.

The [SGLD minting and redemption process](../sgld/minting-and-redemption.md) describes exchanging SGLD for supported stablecoins. Its fees and FIFO queue do not establish the withdrawal rules for sySGLD.

## Before deposits open

The product terms must explain conversion, yield accounting, maturity settlement, early exits, fees and any transfer restrictions. Review those terms and the [yield and risk guide](yield-and-risks.md) before participating.
