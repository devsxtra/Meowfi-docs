# Fees

Alloc8 charges a **10% performance fee** on **realized yield** (profits). This fee applies only to yield earned (such as LP trading fees and other supported harvested rewards, depending on the strategy). It does **not** apply to your deposited principal.

If no yield is realized at the time of collection, **no performance fee is charged**.

#### When the fee is collected

The performance fee is collected only when Alloc8 collects/compounds yield during:

* **Rebalance execution**, or
* **Autocompound execution** (runs **every Sunday at 7:00 PM UTC**)

#### Example

If your position realizes **$100** in yield at a rebalance/autocompound event:

* Performance fee (10%): **$10**
* Net yield added to your position (90%): **$90**

#### Verifying on-chain

To verify fee collection:

1. Open the rebalance/autocompound transaction in a block explorer.
2. Locate the step where yield is collected/compounded.
3. Confirm that a portion of realized yield is routed to the **protocol treasury/fee recipient**, and the remainder follows your **Smart Account/position** flow per the strategy.

#### Gasless rebalances and execution conditions

* Rebalances and autocompounds are **gasless for users** on Alloc8. The protocol pays gas, and these costs are subsidized from the performance fee.
* Rebalances are triggered dynamically, typically when:\
  **fees/yield earned by your position > gas cost to run the rebalance.**
* If gas becomes very high, it may be uneconomical to rebalance smaller positions. In such cases, your position may appear **out of range** on the frontend until rebalancing becomes economical again. We recommend a position size of at least **$500** for smoother rebalances.

#### Fee changes

The performance fee is currently **10%**. Any future updates will be **communicated to users**.

#### Quick FAQs

1. **Do I pay gas for rebalances or autocompound?**\
   No. Rebalances and autocompounds are **gasless for users**. The protocol pays gas, and this cost is covered using part of the performance fee.<br>
2. **Why didn’t my position rebalance / why is it out of range?**\
   Rebalances usually happen when earned fees/yield are higher than the gas cost to run the rebalance. If gas costs rise a lot, it can become uneconomical to rebalance smaller positions, so your position may remain out of range until conditions improve.<br>
3. **Is there a recommended position size?**\
   Yes. We recommend $500 or more for smoother and more consistent rebalances.<br>
4. **Can the fee change?**\
   The performance fee is currently **10%**. If it changes, Alloc8 will **inform users**.
