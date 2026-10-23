# Executive Summary

Syncrate Gold (SGLD) is a fully collateralized, 1:1 physical-gold-backed digital asset designed to bridge institutional-grade bullion markets with onchain DeFi infrastructure. Each SGLD token represents a claim on one troy ounce of LBMA-standard physical gold bars held in secure, independent vaults in the UAE, with continuous pricing and verification supported by Chainlink decentralized oracle infrastructure.

The protocol’s architecture consists of two primary upgradeable smart contracts:

* SGLDToken – An ERC-20 standard, UUPS-upgradeable token implementing mint, burn, pause, and role-based access control.
* SyncrateEngine – The core mint-and-redeem engine responsible for collateral management, oracle price validation, reserve capacity enforcement, asynchronous FIFO redemption queuing and fee collection.

The system prioritizes capital efficiency, transparent reserve accounting, and operational resilience under varying liquidity conditions. Minting is immediate and fee-free against approved stablecoin collateral. Redemptions may settle immediately when onchain liquidity is sufficient or enter a transparent FIFO queue when liquidity is constrained, ensuring orderly processing without forced liquidations or insolvency risk.

This section provides a comprehensive technical description of the architecture.

<br>
