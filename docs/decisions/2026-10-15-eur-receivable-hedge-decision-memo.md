---
type: decision_memo
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-15
status: committed
---

# Corporate Treasury Decision Memo: EUR 4.5M Foreign Exchange Hedging Strategy

**TO:** Chief Financial Officer (CFO)  
**FROM:** Lead Corporate Treasury Analyst  
**DATE:** October 15, 2026  
**SUBJECT:** FX Hedging Recommendation for 1-Year EUR 4,500,000 Commercial Receivable  

---

## Executive Summary & Decision Headline

We recommend executing a **100% Forward Contract Hedge** at the current 1-year rate of **1.1008 USD/EUR**, locking in guaranteed revenue of **$4,953,432 USD** for the EUR 4,500,000 commercial receivable maturing October 2027.

This strategy completely eliminates downside currency risk, locks in a **$70,932 USD forward premium gain** over the unhedged spot baseline ($4,882,500 USD), and requires **$0 upfront cash outlay**. 

While purchasing an At-The-Money (ATM) Put Option provides upside participation in a potential Euro rally, it requires an immediate cash premium of **$112,500 USD** and reduces our guaranteed revenue floor to **$4,770,000 USD** ($183,432 USD below the forward lock). Given our corporate risk mandate prioritizing earnings predictability and liquidity preservation, the Forward Contract represents the optimal risk-adjusted strategy.

---

## Section A: Exposure Summary

* **Commercial Underlying:** Commercial sales export contract generating a EUR 4,500,000 cash inflow.
* **Settlement Horizon:** 12 months (360 days, maturing October 2027).
* **Functional Currency:** United States Dollar (USD).
* **Base Spot Exchange Rate ($S_0$):** 1.0850 USD/EUR.
* **Unhedged Baseline Nominal Value:** **$4,882,500 USD** (evaluated at current spot).

---

## Section B: Comparative Hedge Outcomes

Using live market rate parameters as of October 15, 2026 ($R_{\text{USD}} = 4.75\%$, $R_{\text{EUR}} = 3.25\%$, $S_0 = 1.0850$, $F_0 = 1.1008$), the model yields the following proceeds across hedging alternatives:

| Strategy | Execution / Contract Structure | Net USD Proceeds | Gain / (Loss) vs Unhedged Baseline | Upfront Cash Outlay |
| :--- | :--- | :--- | :--- | :--- |
| **1. Unhedged Baseline** | Unprotected market spot rate at maturity ($S_T$) | $4,882,500 (at $S_0$) | $0 (Reference) | $0 |
| **2. Forward Contract** | 1-Year OTC Bank Forward @ $1.1008$ | **$4,953,432** | **+$70,932** | **$0** |
| **3. Money-Market Hedge** | Borrow EUR 4.36M @ 3.25%, Convert to USD, Invest @ 4.75% | **$4,953,432** | **+$70,932** | **$0 (Net)** |
| **4. ATM Put Option** | Strike $1.0850$, Premium $0.0250/EUR | **$4,770,000 (Floor)** | **-$112,500 (Base Floor)** | **$112,500** |

### Key Findings
1. **Positive Carry Forward Premium:** Because U.S. short-term interest rates ($4.75\%$) exceed European rates ($3.25\%$) by 150 basis points, Covered Interest Rate Parity generates a favorable forward premium ($1.1008$ vs spot $1.0850$). Hedging via forward contract generates an additional **$70,932 USD** over current spot value.
2. **Forward vs. Money Market Parity:** The Money Market Hedge yields identical proceeds (**$4,953,432 USD**) to the Forward Contract. However, the Forward Contract is significantly easier to execute operationally, as it avoids executing simultaneous loan agreements, spot conversions, and deposit placements across two banking entities.
3. **Put Option Floor Cost:** The Put Option establishes a worst-case revenue floor of **$4,770,000 USD** after deducting the **$112,500 USD** premium.

---

## Section C: Sensitivity & Scenario Analysis Interpretation

To evaluate performance under extreme currency volatility, the model evaluated net proceeds across spot settlement scenarios ranging from **-5.0% (EUR collapse)** to **+5.0% (EUR rally)**:
