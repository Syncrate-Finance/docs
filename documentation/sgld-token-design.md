# SGLD Token Design

### Contract Overview

SGLDToken is an upgradeable ERC-20 token implementing the following OpenZeppelin upgradeable modules:

* Initializable
* ERC20Upgradeable
* ERC20BurnableUpgradeable
* ERC20PausableUpgradeable
* AccessControlUpgradeable
* UUPSUpgradeable

### Access Control & Roles

| Role                 | Permissions                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| DEFAULT\_ADMIN\_ROLE | Grant/revoke roles, pause/unpause token, authorize UUPS upgrades, call reinitializer functions. |
| MINTER\_ROLE         | Call mint(address to, uint256 amount). Granted exclusively to the SyncrateEngine contract.      |

### Key Functions

* mint(address to, uint256 amount) – Restricted to MINTER\_ROLE. Creates new SGLD supply.
* burn / burnFrom – Standard ERC-20 burnable functionality used by the Engine during redemptions.
* pause() / unpause() – Admin-only emergency controls that halt all transfers when activated.
* \_authorizeUpgrade – Restricted to DEFAULT\_ADMIN\_ROLE; enforces UUPS upgrade governance.

### Security Properties

* Constructor disables initializers to prevent implementation-contract initialization attacks.
* Pausable transfers provide circuit-breaker capability.
* Role separation ensures the Engine can mint without holding full administrative power over the token.
* UUPS pattern minimizes proxy complexity while retaining upgrade flexibility.
