# Reserve Management & Capacity Control

### Reserve Cap

The reserveCap variable represents the maximum permitted SGLD supply, inclusive of both circulating supply and any outstanding cross-chain capacity reservations. It is updated exclusively by RESERVE\_MANAGER\_ROLE and should be adjusted only after corresponding physical gold has been secured and verified.

Enforcement formula (checked on every mint and reservation): sgldToken.totalSupply() + totalReserved + newAmount ≤ reserveCap

### Cross-Chain Capacity Reservation

To support multi-chain issuance without double-minting risk, the Engine provides a reservation primitive:

1. reserveCapacity(bytes32 requestId, uint256 sgldAmount) – Locks capacity against the global reserveCap for a unique requestId. Reverts if already reserved or if capacity would be exceeded.
2. confirmReservedMint(bytes32 requestId) – Releases the reservation after the remote-chain mint has been confirmed. Does not itself mint tokens; minting occurs on the destination chain under corresponding controls.
3. cancelReservation(bytes32 requestId) – Releases capacity if the cross-chain operation fails or is aborted.

This design cleanly separates capacity accounting on the home chain from actual token issuance on remote chains, enabling secure LayerZero (or equivalent) integrations.<br>

### Scope relative to sySGLD staking

The reserve-cap and cross-chain reservation functions above describe SGLD issuance capacity. They do not define staking positions, lease accounting or sySGLD withdrawal liquidity.

[sySGLD](../syncrate-prime-sysgld/sysgld/) involves direct leasing of the physical gold backing staked SGLD. Lease accounting and staking settlement are separate from the SGLD minting reserve-cap checks. A reserve-cap check or reserve audit alone does not demonstrate that leased gold can be returned on demand.

See [Yield & Risks](../syncrate-prime-sysgld/sysgld/yield-and-risks.md) for the financing and liquidity considerations.
