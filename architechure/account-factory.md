# Account Factory

The `Modular4337Factory` is a fully on-chain, deterministic factory contract used to deploy modular smart accounts (`Modular4337Account`) via the CREATE2 opcode. It supports flexible account instantiation, vault registries, and optional integration with ERC-4337's EntryPoint staking model.

**Key Features**

* **Deterministic Wallet Addresses**\
  Every smart account is deployed using `CREATE2` with a per-user salt and nonce combination, allowing you to predict the address **before** deployment.
* **Per-User Account Caps**\
  Enforces a maximum number of wallets each user can deploy (default: 5), adjustable by the contract owner.
* **Proxy Pattern**\
  All accounts are proxies pointing to a shared `Modular4337Account` implementation, minimizing deployment gas costs.
* **Vault Whitelist Registry**\
  Includes built-in support for managing whitelisted vault contracts that can interact with accounts — useful for protocol-specific integrations.
* **On-Chain Metadata**\
  Maintains a complete registry of:
  * Wallets deployed by each user (`getUserWallets`)
  * Wallet ownership (`userWalletOwner`)
  * Wallet deployment nonces (`userNonce`)
* **EntryPoint Stake Utilities**\
  Supports `addStake`, `unlockStake`, and `withdrawStake` for ERC-4337 compliance and bundler staking.

**Usage Flow**

1. **Compute Wallet Address:**\
   Use `computeAccountAddress(owner, salt)` to pre-calculate the address of the wallet.
2. **Deploy Wallet:**\
   Call `deployAccount(owner, salt)` to deploy the proxy and initialize the wallet with the provided `owner` and `EntryPoint`.
3. **Manage Vaults:**\
   Admins can register protocol-approved vault contracts with `addVault()` or `bulkAddVaults()`.
