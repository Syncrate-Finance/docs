# Mint Flow

Minting is designed to be capital-efficient and frictionless for users holding approved stablecoins.

### Sequence

1. User approves the Engine to spend the desired stablecoin amount.
2. User calls mint(uint256 stablecoinAmount, address tokenAddress).
3. Engine validates: amount > 0, token is whitelisted, oracle price is valid and fresh.
4. Engine computes sgldAmountToMint using the pricing formula.
5. Engine checks that totalSupply + totalReserved + sgldAmountToMint ≤ reserveCap.
6. Engine pulls stablecoin via safeTransferFrom into itself.
7. Engine calls sgldToken.mint(msg.sender, sgldAmountToMint).
8. SGLDMinted event is emitted.

### Properties

* Zero protocol fee on mint.
* Atomic – either fully succeeds or fully reverts.
* Respects global reserve capacity including pending cross-chain reservations.
* Only whitelisted collateral tokens are accepted.

