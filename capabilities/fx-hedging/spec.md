---
type: spec
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-15
status: committed
---

# Technical Model Specification: FX Hedging Model

## 1. Problem Statement
Our firm has a contracted foreign currency receivable of **EUR 4,500,000** due in **360 days** (1 year). Unhedged, an adverse shift in the exchange rate directly reduces our USD operating proceeds. 

At a baseline spot rate of 1.1000 USD/EUR, the expected inflow is $4,950,000. A 9% decline in the euro down to 1.0000 USD/EUR creates an unhedged budget hole of $450,000. This model evaluates three hedging strategies—Forward Contract, Money-Market Hedge, and Currency Options—to determine the optimal risk-mitigation trade.

## 2. Inputs: The Named-Range Contract

All financial logic relies on ten standardized named ranges. The values below are indicative placeholders used for initial model architecture and will be replaced with live market feeds in Stage 4.

| Named Range | Description | Placeholder Value | Unit | Stage 4 Data Source |
| :--- | :--- | :--- | :--- | :--- |
| `FC_AMT` | Foreign-currency receivable | 4,500,000 | EUR | Commercial Invoice / Sales Contract |
| `S0_in` | Spot exchange rate at inception | 1.1000 | USD per EUR | Live Spot FX Market Feed |
| `F0_in` | 1-Year Forward exchange rate | 1.1050 | USD per EUR | Bank Forward Rate Curve |
| `R_USD` | USD borrowing/investment rate | 5.25% | Annual % | Fed H.15 / 1-Year Treasury Yield |
| `R_FC` | EUR borrowing rate | 3.50% | Annual % | ECB Euro Short-Term Rate (€STR) |
| `K_PUT` | Put option strike price | 1.1000 | USD per EUR | FX Option Market Desk |
| `K_CALL` | Call option strike price | 1.1000 | USD per EUR | FX Option Market Desk |
| `PREM_PUT` | Put option premium per unit | 0.0250 | USD per EUR | Option Pricing Desk / Garman-Kohlhagen |
| `PREM_CALL` | Call option premium per unit | 0.0250 | USD per EUR | Option Pricing Desk / Garman-Kohlhagen |
| `T_DAYS` | Days to settlement | 360 | Days | Transaction Contract Terms |

*Note: All interest rates use the ACT/360 day-count convention.*

## 3. Tab Architecture

The workbook will contain six dedicated tabs to keep inputs, calculations, and reporting clean and auditable:

1. `Cover`: Project metadata, model versioning, author info, and execution status.
2. `Legend_Key`: Color-coding rules (Blue text = Hardcoded inputs, Black text = Calculations/Formulas, Gray fill = Summary outputs).
3. `Inputs`: Dedicated input table hosting all ten named ranges.
4. `Hedge_Calculations`: Explicit step-by-step math for Unhedged, Forward, Money-Market, and Option strategies.
5. `Sensitivity_Analysis`: Payoff grid evaluating settlement spot rates from -5% to +5% of inception spot.
6. `Notes_Assumptions`: Documentation of rate conventions, tax/fee treatments, and execution constraints.

## 4. Assumptions & Constraints
* **Day-Count Convention:** Money-market interest rates and forward calculations use standard Money Market convention: `ACT/360`.
* **Transaction Costs:** Zero execution spreads or brokerage fees assumed for baseline calculations.
* **Option Premium Accounting:** Option premiums are paid upfront at $T=0$. For settlement comparison, option premiums are evaluated at net cash outlay without compounding interest adjustments unless specified.
* **Credit Liquidity:** Assumes full credit availability to borrow foreign currency without facility fees.

## 5. Calculation Flow & Formulas

Every formula uses named ranges instead of direct cell references.

### Strategy 0: Unhedged Baseline
At settlement spot rate $S_T$:
$$\text{USD Proceeds} = \text{FC\_AMT} \times S_T$$

### Strategy 1: Forward Hedge
Locks in cash proceeds today:
$$\text{USD Proceeds} = \text{FC\_AMT} \times \text{F0\_in}$$

### Strategy 2: Money-Market Hedge (3-Step Pipeline)
* **Step 1 (Borrow Foreign Currency Today):** Size loan so receivable repays principal + interest exactly at maturity.
  $$\text{Borrow\_FC} = \frac{\text{FC\_AMT}}{1 + \left(\text{R\_FC} \times \frac{\text{T\_DAYS}}{360}\right)}$$
* **Step 2 (Convert to USD at Spot):**
  $$\text{USD\_Now} = \text{Borrow\_FC} \times \text{S0\_in}$$
* **Step 3 (Invest USD at Local Rate):**
  $$\text{USD Proceeds} = \text{USD\_Now} \times \left(1 + \left(\text{R\_USD} \times \frac{\text{T\_DAYS}}{360}\right)\right)$$

### Covered Interest Rate Parity Check
Calculates theoretical forward rate to verify market alignment:
$$\text{F\_Implied} = \text{S0\_in} \times \frac{1 + \left(\text{R\_USD} \times \frac{\text{T\_DAYS}}{360}\right)}{1 + \left(\text{R\_FC} \times \frac{\text{T\_DAYS}}{360}\right)}$$

### Strategy 3: Option Hedge (Put Floor)
Calculates net payout at settlement spot $S_T$:
$$\text{Gross USD Proceeds} = \text{FC\_AMT} \times \max(S_T, \text{K\_PUT})$$
$$\text{Total Premium Paid} = \text{FC\_AMT} \times \text{PREM\_PUT}$$
$$\text{Net USD Proceeds} = \text{Gross USD Proceeds} - \text{Total Premium Paid}$$

## 6. Sensitivity Analysis Plan
The model evaluates scenario spot rates ($S_T$) ranging from **-5% to +5%** of inception spot (`S0_in`) in **1% increments** (11 data points: 1.0450 to 1.1550 USD/EUR).

* **Output Table Columns:** Settlement Spot ($S_T$), Unhedged Proceeds, Forward Proceeds, Money-Market Proceeds, Option Net Proceeds.
* **Chart Purpose:** A line chart comparing settlement spot vs. USD proceeds across all strategies. This shows the CFO where the option hedge outpaces the forward lock and illustrates the floor protection.

## 7. Validation Rules & Self-Checks

To prevent formula bugs, the built workbook must pass three explicit audit checks:

1. **Parity Check:** If market forward rate `F0_in` equals `F_Implied`, Money-Market proceeds must equal Forward proceeds within a $0.01 tolerance:
   $$\text{ABS}(\text{MM\_Proceeds} - \text{Forward\_Proceeds}) < 0.01$$
2. **Error-Free Execution:** No cells may output `#VALUE!`, `#REF!`, `#DIV/0!`, or `#N/A`.
3. **Dynamic Formula Audit:** Summary table outputs must be driven by formulas connected to the input tab; no hardcoded summary numbers.

## 8. Summary Outputs
The final summary section formats key results into gray output cells:
* `OUT_UNHEDGED_BASE`: Unhedged USD proceeds at spot $S_0$.
* `OUT_FORWARD_PROC`: Total guaranteed USD proceeds via Forward.
* `OUT_MM_PROC`: Total guaranteed USD proceeds via Money-Market.
* `OUT_OPTION_FLOOR`: Minimum guaranteed net USD proceeds via Put Option.
* `OUT_PARITY_DIFF`: Variance between Forward and Money-Market execution.

---
## Spec Review & Iteration Log
* **Drafting Notes:** Initial specification drafted following class template.
* **AI Audit Iteration:** An initial review of the draft identified that the interest rate basis was missing explicit day-count formulas. Updated Section 2 and Section 4 to explicitly specify `ACT/360` day-count logic across all interest rate calculations and input tables.
