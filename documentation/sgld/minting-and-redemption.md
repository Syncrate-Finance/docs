---
description: How SGLD is minted and redeemed.
---

# Minting & Redemption

## Onchain minting

Minting is immediate and carries **no fee**. Users mint SGLD by depositing an approved stablecoin collateral through the SyncrateEngine contract via the [Minting Webapp](https://syncrate.org/app).

The flow is simple:

1. Approve SyncrateEngine to spend the chosen stablecoin.
2. Call the mint function with the amount and token address.
3. The Engine checks that the amount is greater than zero, the stablecoin is approved, and the Chainlink gold price is positive and fresh.
4. The Engine calculates the SGLD amount using the oracle price.
5. The Engine checks the reserve cap, including outstanding cross-chain reservations.
6. The Engine receives the stablecoin and mints SGLD to the user's wallet.

If a check fails, the transaction reverts. Only USDC & USDT collateral is accepted.

## Onchain redemption

Users redeem SGLD through the redemption webapp. The Engine calculates the USD value using the Chainlink gold price and applies the configured redemption fee.

* **Default redemption fee:** 0.25%.
* **Maximum redemption fee:** 5%.
* The fee is paid in the selected stablecoin to the configured fee recipient.
* The remaining amount is owed to the user.

The Engine checks its available balance to determine whether the redemption can settle immediately or must enter the FIFO queue.

## Immediate settlement

If the SyncrateEngine contracts has enough of the requested stablecoin collateral to cover the net payout and fee, it transfers the fee to the fee recipient and sends the remaining stablecoin amount to the user's. The user's SGLD is burned as part of the redemption.

## FIFO redemption queue

If the Engine does not have enough liquidity to settle a redemption, the user's SGLD is burned and the stablecoin payout obligation is recorded in the redemption queue.

* **First in, first out (FIFO):** requests are processed in queue order.
* **Liquidity sources:** new mint deposits or stablecoin top-ups can replenish the Engine and allow queued requests to be paid.
* **Queue visibility:** queue-related events are emitted onchain, including the queue index, user, requested stablecoin, amount burned, and amount owed.

Queue processing stops when it reaches the iteration limit or when the next request cannot be fully satisfied. A request for one stablecoin is paid from that stablecoin balance; a balance in another token does not automatically satisfy it.

**Important:** SGLD is burned when a redemption enters the queue. The user then has a pending stablecoin receivable rather than the burned SGLD tokens. Queue entry does not mean the stablecoin has already been paid.

## Physical gold redemption

SGLD tokens can also be redeemed for the underlying gold bars with a minimum redemption amount of 160.75 SGLD (\~5kg of gold). A 5% fee is charged on all physical SGLD redemptions. Additional KYC & AML checks are been conducted for every physical redemption request.

## Oracle and reserve checks

Both minting and redemption depend on the Chainlink XAU/USD price feed. The Engine rejects prices that are zero or negative, or older than the configured staleness threshold.&#x20;
