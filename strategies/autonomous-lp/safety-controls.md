# Safety Controls

Even when the agent decides an action is “good,” it must still pass **GSPM** checks before executing. These include:

* **Per-transaction slippage check** (min-out / value-loss bound)
* **Rolling-window slippage budgets** (caps cumulative loss over a time window)
* **Pool ↔ oracle deviation budget** (blocks abnormal price divergence)
* **Tick-ratio sanity checks** (guards against drifting execution)
* **Recipient pinning** (proceeds go only to your Smart Account/owner path)
* **Token + agent allowlists** :contentReference\[oaicite:9]{index=9}

If checks fail, the transaction **reverts** (it does not partially execute).
