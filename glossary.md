# Glossary

* **EOA**: Externally Owned Account (eg, your Metamask wallet address).
* **Smart Account**: Your modular ERC-4337 account contract.
* **ERC-4337**: Account abstraction standard using UserOperations executed via an EntryPoint.
* **Bundler**: Off-chain service that submits UserOperations to EntryPoint.
* **EntryPoint**: ERC-4337 contract that validates and executes UserOperations.
* **Session Agent**: Authorized signer for an account session.
* **Session**: A time/permission-bounded authorization for an agent to act on your behalf.
* **GSPM**: Global Session Policy Module; policy firewall for agent execution.
* **Position Manager**: Liquidity manager for NFT-based concentrated liquidity positions.
* **MEOW Agent**: Strategy module computing tick ranges and rebalance signals.
* **Slippage budget**: Allowed value loss over a rolling window, enforced on-chain.
* **Recipient pinning**: Enforcement that proceeds go only to the account or owner.
* **Performance fee**: A protocol fee charged on realized fees/yield, collected at rebalance or autocompound events.\
  **Autocompound**: A scheduled process that realizes and reinvests fees/yield back into the position (runs weekly).
