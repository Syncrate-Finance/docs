# Oracle Integration & Pricing

### Price Feed

The SyncrateEngine consumes a Chainlink AggregatorV3Interface gold price feed. The latestRoundData() call returns (roundId, answer, startedAt, updatedAt, answeredInRound). The system uses answer (price) and updatedAt for validation.

### Validation Rules

* Price must be strictly greater than zero (invalidates negative or zero answers).
* Price must not be stale: block.timestamp – updatedAt ≤ oracleStalenessThreshold.
* Both mint and redeem paths enforce these checks; failed validation reverts with custom errors.

### Pricing Formulas

**Mint (stablecoin → SGLD):** `sgldAmountToMint = (stablecoinAmount × 1e20) / uint256(price)`

**Redeem (SGLD → USD value):** `usdValue = (sgldAmount × uint256(price)) / 1e20`

The 1e20 scaling factor assumes the gold price feed returns a value with 8 decimals (standard Chainlink convention for USD pairs) while SGLD and typical stablecoins use 18 decimals, producing a consistent 18-decimal result.

### Staleness Protection

The oracleStalenessThreshold is configurable by DEFAULT\_ADMIN\_ROLE. A conservative threshold protects users from acting on outdated prices during oracle outages or network congestion. Operators should set this parameter in accordance with the chosen Chainlink feed’s heartbeat and deviation thresholds.<br>
