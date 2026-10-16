---
type: brief
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-15
status: committed
hypothesis: "I expect a forward contract or money-market hedge to offer the most cost-effective risk protection for our 1-year EUR receivable, as high option premiums will outweigh the value of potential currency upside."
---

# Executive Brief: Foreign Exchange Hedging Strategy

## 1. Executive Summary & Market Risk
We have an incoming payment of EUR 4,500,000 due in one year. Because our financial reporting is in US Dollars, this pending cash flow creates an unhedged foreign currency exposure.

If the EUR/USD rate stays at today's benchmark rate of 1.10, the payment yields **$4,950,000**. If the euro drops to 1.00 by settlement date, our payout falls to **$4,500,000**. That single 9% shift creates a **$450,000 shortfall** against operating projections.

Leaving this exposure untouched turns operational revenue into a speculative currency bet. Our goal is to lock in or protect the dollar value of these proceeds before settlement.

## 2. Evaluation of Hedging Instruments

We are evaluating three main tools to handle this exposure:

| Tool | Mechanics | Main Advantage | Main Drawback |
| :--- | :--- | :--- | :--- |
| **Forward / Futures Contract** | Fixes an exchange rate today for delivery in 1 year. | Complete cash flow certainty; eliminates downside risk. | Gives up all upside if the euro strengthens. |
| **Money-Market Hedge** | Borrow EUR today against the receivable, convert at spot, and invest USD locally. | Delivers the same rate lock as a forward contract. | Uses up corporate borrowing capacity and adds execution steps. |
| **Options (Put Floor)** | Pay an upfront fee for the right to sell EUR at a guaranteed minimum rate. | Secures a payout floor while keeping upside open. | Upfront premium is lost if the option isn't exercised. |

A forward contract works like fixing a sales price ahead of time: you trade away potential price upside for rate certainty. An option works like property insurance: you pay an upfront premium hoping you won't need it, but keep the protection if market rates drop.

## 3. Assumptions & Testable Hypothesis

**Key Assumptions:**
* The EUR 4,500,000 payout will arrive on schedule at month 12 without default or delay.
* Credit lines are available to execute a money-market hedge if needed.
* Baseline spot rate remains EUR/USD 1.10.

**Hypothesis:**
I expect a **forward contract** to deliver the highest net dollar payout relative to risk. Paying an upfront option premium in current volatility conditions will likely drag down final net cash proceeds compared to a clean fixed rate lock.

**How We Falsify This Hypothesis:**
This hypothesis will be wrong if option pricing is low enough that the net payout floor (after subtracting the premium) beats the forward rate, or if foreign interest rate gaps make a money-market hedge significantly cheaper.

## 4. Next Steps & Project Roadmap
Before making a final recommendation, we will run this analysis through a quantitative model built across four remaining stages:

1. **Stage 2 (Model Spec):** Design the tab layout, inputs, and formula logic on paper.
2. **Stage 3 (Model Build & Audit):** Build the workbook in Excel and test for calculation errors.
3. **Stage 4 (Market Data):** Plug in live exchange rates, forward points, and option volatility.
4. **Stage 5 (Decision Memo):** Stress-test outputs and deliver the final trade recommendation.
