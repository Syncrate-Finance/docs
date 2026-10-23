# Mint & Redeem Engine (SyncrateEngine)

### Contract Overview

SyncrateEngine is the operational heart of the SGLD system. It is also UUPS-upgradeable and integrates AccessControl and Pausable modules. The Engine never holds administrative control over the token beyond the MINTER\_ROLE; it interacts with SGLDToken exclusively through a minimal interface:

```
interface ISGLDToken {
    function mint(address to, uint256 amount) external;
    function burnFrom(address account, uint256 amount) external;
    function totalSupply() external view returns (uint256);
}
```

### Roles

| Role                   | Key Permissions                                                                                                                                               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DEFAULT\_ADMIN\_ROLE   | Set collateral whitelist, update gold price feed, set redeem fee, set oracle staleness threshold, pause/unpause, sweep funds to treasury, grant/revoke roles. |
| RESERVE\_MANAGER\_ROLE | Update reserveCap, reserveCapacity, confirmReservedMint, cancelReservation. Critical for operational and cross-chain capacity management.                     |
| UPGRADER\_ROLE         | Authorize UUPS upgrades of the Engine implementation.                                                                                                         |

### Key State Variables

| Variable                 | Type                        | Purpose                                                      |
| ------------------------ | --------------------------- | ------------------------------------------------------------ |
| sgldToken                | ISGLDToken                  | Reference to the SGLD token contract.                        |
| goldPriceFeed            | AggregatorV3Interface       | Chainlink XAU/USD price feed.                                |
| reserveCap               | uint256                     | Maximum total SGLD supply (including reserved capacity).     |
| redeemFeeBps             | uint256                     | Redeem fee in basis points (default 25, max 500).            |
| oracleStalenessThreshold | uint256                     | Maximum age (seconds) of an acceptable oracle price.         |
| redemptionQueue          | RedemptionRequest\[]        | FIFO queue of pending redemption obligations.                |
| nextQueueIndex           | uint256                     | Pointer to the next unprocessed queue entry.                 |
| isAllowedCollateral      | mapping(address => bool)    | Whitelist of accepted stablecoin collateral tokens.          |
| reservedCapacity         | mapping(bytes32 => uint256) | Cross-chain capacity reservations by requestId.              |
| totalReserved            | uint256                     | Aggregate reserved capacity across all pending reservations. |

### 4.4 Constants

* MAX\_FEE\_BPS = 500 (5% absolute ceiling on redeem fee)
* BPS\_DENOMINATOR = 10\_000
