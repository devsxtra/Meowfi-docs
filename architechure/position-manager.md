---
description: Alloc8PositionManager
---

# Position Manager

The `PositionManager` is a non-fungible liquidity management contract built on top of uniswap V3, and similar models. It allows users to mint, increase, decrease, collect, and burn liquidity positions represented as NFTs, with added support for **treasury fees**, **TWAP volatility guards**, **admin recovery functions**, and **parameterised configuration**.

It inherits core functionality from `BasePositionManager` and adds direct integration with Camelot's `ICamelotNonfungiblePositionManager` and `IAlgebraPoolV3 or other such pools`

***

#### Key Features

**NFT-Based Liquidity Minting**

* Users can mint new positions using `mintLiquidity()`, which returns a unique position `tokenId` representing an LP NFT.
* Includes built-in refund for unused tokens to prevent overpayment.

**Fee Collection & Treasury Routing**

* Automatically deducts protocol treasury fees on `collectFees()` and `burnLiquidity()` using `_collectLogic()`.
* Treasury address and fee 3% (in basis points) can be configured and updated by admins.

**Volatility Guard (TWAP-Based)**

* All mint and increase operations are guarded by `onlyCalmPeriods` which checks if current tick is within a safe deviation from TWAP.
* Admins can update deviation thresholds and TWAP interval.

***

#### Core Functions

* &#x20;**`mintLiquidity(...)`**

Mints a new LP position and returns the `tokenId`. Refunds unused token0/token1.

* &#x20;**`increaseLiquidity(...)`**

Adds tokens to an existing position. Refunds unused amounts. Requires position ownership.

* &#x20;**`decreaseLiquidity(...)`**

Removes part of the liquidity from a position. Transfers fees and liquidity to a recipient.

* &#x20;**`collectFees(...)`**

Transfers LP NFT to the contract, collects all fees, deducts treasury cut, then returns NFT to user.

* &#x20;**`burnLiquidity(...)`**

Performs full withdrawal:

1. Collects fees
2. Removes all liquidity
3. Transfers to user
4. Burns NFT
5. Removes position from `userPositionIds`

* &#x20;**`batchUpdateConfig(...)`**

Admin-only method to update:

1. Treasury address and fee
2. Token0 / Token1
3. Min/Max limits
4. Pool address / fee / TWAP

***

#### &#x20;Admin Functions

| Function                       | Purpose                                       |
| ------------------------------ | --------------------------------------------- |
| `pause()` / `unpause()`        | Emergency stop/start                          |
| `updateTreasury()`             | Set new treasury address and fee percent      |
| `updateTokenPair()`            | Change underlying token0/token1 addresses     |
| `updateAmountLimits()`         | Change min/max limits for token0/token1       |
| `setDeviation()`               | Adjust TWAP deviation for volatility check    |
| `emergencyWithdraw()`          | Recover stuck funds from contract (any ERC20) |
| `addAdmin()` / `removeAdmin()` | Manage ADMIN\_ROLE                            |



***

#### Internal Design Notes

* **Caching**: `currentState()` caches tick/price info for 60 seconds to optimize gas across frequent liquidity actions.
* **Volatility Guard**: `_isCalm()` calculates deviation from TWAP and ensures tick stability.
* **Composability**: Designed to be extended for specific pool integrations via `BasePositionManager`.



***

#### Extensibility

Because `BasePositionManager` is an abstract class, we can reuse this structure for:

* Uniswap V3 (by overriding `currentTick()` and `_twap()`)
* Other Algebra forks (e.g., THENA, Velodrome v2)
* Custom fee managers
* Time-locked vault integrations

***

#### &#x20;Example Use Case

```solidity
solidityCopyEdit// Minting liquidity
IERC20(token0).approve(address(positionManager), amt0);
IERC20(token1).approve(address(positionManager), amt1);

(uint256 tokenId, , , ) = positionManager.mintLiquidity(
    amt0, amt1,
    -887220, 887220, // full range
    0, 0             // slippage
);

// Collecting fees
positionManager.collectFees(tokenId, msg.sender);

// Full exit
positionManager.burnLiquidity(tokenId, msg.sender, 0, 0);
```
