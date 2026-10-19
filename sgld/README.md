---
description: >-
  Understand SGLD, Syncrate's gold-backed token on Base, including its backing,
  transferability, and intended use.
---

# What is SGLD

**SGLD (Syncrate Gold)** is a tokenized gold asset issued by Syncrate Technologies Inc. (BVI Number: 2217779). Each SGLD token is an ERC-20 token representing a direct ownership to 1 troy ounce of LBMA-standard physical gold held in secure vaults in the UAE. It brings gold-backed exposure onto Base, allowing users to hold and transfer a digital token rather than manage physical bullion themselves.

SGLD is the foundation of Syncrate's tokenized commodities products. It provides gold-backed exposure.

## How SGLD works

SGLD connects physical gold reserves with an onchain token:

* **Physical backing:** Every SGLD token is designed to be backed 1:1 by physical LBMA-certified gold bars held in secure vaults with reputable custodians in the UAE.
* **Reserve verification:** The physical reserves are subject to independent audits conducted by Bureau Veritas. Audit reports and reserve asset statements can be found in the [Transparency Dashboard](https://www.syncrate.org/sgld) on the SGLD webpage.
* **Pricing infrastructure:** The SyncrateEngine contracts relies on Chainlink's XAU/USD price feed for SGLD pricing.
* **Controlled issuance:** SGLD is issued through the protocol's minting contracts. The intended relationship is that tokens are minted against corresponding USDC/USDT collateral and reduced when redeemed, rather than created through discretionary, unbacked issuance. More info in the Technical Architecture page.

Reserve audits and blockchain data verify different things. A bullion inventory audit provides evidence about physical gold holdings; onchain records show token supply and contract activity. Neither should be treated as a substitute for the other.

## Transferability

SGLD is a transferable ERC-20 token on Base. A holder can send it to another compatible ERC-20 address or interact with DeFi applications that support the token, subject to the token contract, the receiving application, and any applicable restrictions.

To transfer SGLD, a user generally needs:

1. A wallet that supports Base or an exchange that has listed SGLD.
2. The correct SGLD token contract address.
3. A small amount of ETH on Base to pay the network transaction fee.

Transferability does not mean that every exchange, wallet, protocol, or jurisdiction supports SGLD. Third-party services set their own listing, custody, and access requirements. Network fees are paid separately and can vary with Base network conditions.

## Important considerations

SGLD is a digital token representing gold-backed exposure. Users should review the applicable terms and disclosures before acquiring or transferring it. The value of gold can fluctuate, and token holders also face risks related to smart contracts, blockchain infrastructure, custodians, counterparties, liquidity, and the operation of the protocol.

The token's stated backing should not be interpreted as an unconditional promise that every holder can immediately exchange any amount of SGLD for physical gold. The available redemption routes, eligibility, fees, minimum amounts, processing times, and legal rights are governed by the applicable redemption terms.

For the specific steps and conditions, see [Minting & Redemption](minting-and-redemption.md).

## Next steps

* [Minting & Redemption](minting-and-redemption.md): how SGLD is issued and how onchain and physical redemption are handled.
