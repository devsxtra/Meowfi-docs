---
description: Alloc8 Smart Accounts
---

# Modular Smart Accounts

Alloc8’s smart accounts are built on top of the **ERC-4337 account abstraction standard**, extended with **ERC-6900 modularity**. At the core is the `Modular4337Account` — a **highly composable**, **upgrade-friendly**, and **developer-extensible** smart wallet contract.

By leveraging a **hook-based architecture**, developers can easily plug in custom logic to handle validation, execution, policy enforcement, fee routing, and more — all without modifying the core account code.

***

#### Key Capabilities

* **Modular Hook Design**\
  Attach or remove logic dynamically via `installModule()` and `removeModule()`. Add only what you need.
* &#x20;**Validation Hooks**\
  Enforce arbitrary preconditions during `validateUserOp()` — such as session keys, time locks, multi-sig auth, or custom signature schemes.
* **Execution Hooks**\
  Intercept and augment outbound transactions with on-chain logic — from allowlists to fee rebating, or auto-forwarding.
* **ERC-1271 Signature Compatibility**\
  Ensures backwards compatibility with dApps expecting traditional ECDSA signatures.
* **Native ERC-4337 Integration**\
  Fully compatible with the EntryPoint contract and bundler flows — enabling gasless UX and programmable userOps.
* **Extensible by Design**\
  Built for plug-and-play integration with any module conforming to the `IModule` interface — enabling safe upgrades and external extensions.

***

#### Why Use Modular4337Account?

Unlike fixed-function smart wallets, `Modular4337Account` it enables developers and protocols to **compose their wallet features on-chain** — similar to how plug-ins work in software frameworks. This means:

* You don’t need to deploy new contracts to change wallet behaviour.
* You can progressively add features like **social recovery**, **role-based access**, **ERC20 spending caps**, or **session delegation**.
* Modules are auditable, reusable, and independently testable.

This contract serves as the **programmable core** of Alloc8 wallets — enabling use cases far beyond simple ownership.
