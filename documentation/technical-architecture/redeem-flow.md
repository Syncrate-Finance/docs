# Redeem Flow

### Immediate vs Queued Settlement

Redemption is deliberately designed to be robust under varying liquidity conditions. When the Engine holds sufficient stablecoin balance, redemption settles immediately. When liquidity is insufficient, the obligation is recorded in a transparent FIFO queue and the user’s SGLD is burned immediately, converting the claim into a queued stablecoin receivable.

### Sequence

1. User calls redeem(uint256 sgldAmount, address stablecoinAddress).
2. Engine validates amount and collateral whitelist.
3. Engine computes usdValue via oracle and applies redeemFeeBps to produce fee and netOwed.
4. Engine burns sgldAmount from the user via burnFrom (requires prior approval or permit).
5. If Engine balance ≥ netOwed + fee → immediate settlement: fee sent to FEE\_RECIPIENT, netOwed sent to user.
6. Else → queue both the fee (to FEE\_RECIPIENT) and the netOwed (to user) as separate RedemptionRequest entries.

### Queue Processing

Anyone may call processQueue(uint256 maxIterations) while the contract is not paused. The function iterates from nextQueueIndex, paying out requests in order for which sufficient balance of the requested stablecoin exists. Iteration stops when either maxIterations is reached or the next request cannot be fully satisfied. This design allows keepers or interested parties to advance the queue as liquidity arrives (e.g., from new mints or operational top-ups).

### RedemptionRequest Structure

```
struct RedemptionRequest {
    address account;      // recipient of the stablecoin
    address stablecoin;   // which token is owed
    uint256 amountOwed;   // amount of that token
 }
```

<br>
