# ![Alloc8 Logo](https://github.com/user-attachments/assets/7f79c33a-0cf2-4eec-bfbb-d0e266621837)

**Alloc8** is an agentic asset allocation protocol enabling programmable delegation of funds through smart accounts. Whether you're a DAO automating treasury strategies, a yield optimizer seeking abstraction, or an LP aiming for passive yield, Alloc8 provides execution autonomy without sacrificing custody.

---

## 🧠 Core Features

- 🔐 **ERC-4337 Smart Accounts**  
  Fully modular, upgradeable accounts with session-key policies and permissioned delegation.

- 🧬 **MEOW Agents**  
  Autonomous strategies that manage LP positions (e.g., Uniswap V3, Camelot) using encoded execution policies and real-time signals.

- 🧑‍💼 **Human Portfolio Manager Support**  
  Allow trusted managers to execute trades or rebalance vaults on behalf of wallet owners.

- 🛡 **Policy-Based Execution**  
  Fine-grained access control for targets, selectors, gas limits, value bounds, and receivers.

- 📈 **AI-Generated Ticks**  
  Optional integration of LLM-based models to determine Uniswap V3 range positions dynamically.

- 🌉 **Arbitrum-native**  
  Deploys and runs on Arbitrum with full bundler compatibility (Alchemy, Stackup, etc).

---

## 🔗 Architecture Overview
```mermaid
sequenceDiagram
    participant EOA
    participant Alloc8Account
    participant SessionPolicyModule
    participant MeowAgent
    participant AlchemyBundler
    participant EntryPoint
    participant Alloc8PositionManager
    participant UniswapV3

    EOA->>Alloc8Account: deploy & validate
    EOA->>SessionPolicyModule: install & create session
    Alloc8Account->>MeowAgent: request optimal ticks
    MeowAgent->>Alloc8Account: return suggested tick range
    Alloc8Account->>Alloc8PositionManager: deposit funds & mint position
    Alloc8PositionManager->>UniswapV3: burn old position, mint new one
    MeowAgent->>AlchemyBundler: submit rebalance operation (validateUserOp)
    AlchemyBundler->>EntryPoint: handleOps()
    EntryPoint->>Alloc8Account: Rebalance Position
