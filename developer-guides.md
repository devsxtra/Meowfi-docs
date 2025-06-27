# Developer Guides

## Deploy Wallet

```solidity
Modular4337Factory factory = Modular4337Factory(0x0e16a3...);
address wallet = factory.deployAccount(ownerEOA, keccak256("alloc8-user"));
```
