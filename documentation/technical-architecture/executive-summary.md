# Executive Summary

Syncrate Gold (SGLD) is a fully collateralized, 1:1 physical-gold-backed digital asset designed to bridge institutional-grade bullion markets with onchain DeFi infrastructure. Each SGLD token represents a claim on one troy ounce of LBMA-standard physical gold bars held in secure, independent vaults in the UAE, with continuous pricing and verification supported by Chainlink decentralized oracle infrastructure.

The SGLD mint-and-redeem layer described in this section consists of two primary upgradeable smart contracts:

* SGLDToken – An ERC-20 standard, UUPS-upgradeable token implementing mint, burn, pause, and role-based access control.
* SyncrateEngine – The core mint-and-redeem engine responsible for collateral management, oracle price validation, reserve capacity enforcement, asynchronous FIFO redemption queuing and fee collection.

The system prioritizes capital efficiency, transparent reserve accounting, and operational resilience under varying liquidity conditions. Minting is immediate and fee-free against approved stablecoin collateral. Redemptions may settle immediately when onchain liquidity is sufficient or enter a transparent FIFO queue when liquidity is constrained. Queueing records an unpaid redemption obligation; it does not eliminate liquidity or solvency risk.

## sySGLD staking

The separate [sySGLD product](../sysgld/) uses fixed-term staking: users stake SGLD for 30 or 90 days, with a minimum of 1 SGLD, and receive sySGLD. The physical gold backing staked SGLD is leased directly to vetted bullion banks and established counterparties, with lease interest providing yield.

The SGLDToken and SyncrateEngine descriptions below cover SGLD issuance and redemption; sySGLD staking uses a separate product flow. Conversion, yield accounting, maturity settlement, early exits and fees are governed by the applicable staking terms.

See [Staking SGLD](../sysgld/staking.md), [Yield & Risks](../sysgld/yield-and-risks.md) and [SYNC Tokenomics](../sync-tokenomics.md) for the corresponding product and token information.

<br>
