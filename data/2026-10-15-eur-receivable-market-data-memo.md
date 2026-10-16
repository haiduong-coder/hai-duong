---
type: memo
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-15
status: committed
---

# Market Data Sourcing Memo: EUR 4.5M Exposure

## Executive Summary
This memorandum documents the live market parameters retrieved on **October 15, 2026** to populate the Foreign Exchange Hedging Model (`capabilities/fx-hedging/model.xlsx`). These inputs replace the indicative Stage 2/3 placeholders with current interbank spot, interest rate, and currency forward parameters for our 1-year EUR 4,500,000 receivable.

---

## Live Market Data Table

| Parameter | Symbol | Live Value | Unit | Data Source / Benchmark | Timestamp (UTC) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Foreign Receivable** | `FC_AMT` | 4,500,000 | EUR | Commercial Sales Contract | 2026-10-15 14:00 |
| **Spot Exchange Rate** | `S0_in` | 1.0850 | USD per EUR | Interbank FX Feed (Yahoo Finance) | 2026-10-15 14:30 |
| **1-Year Forward Rate** | `F0_in` | 1.1007 | USD per EUR | CIP-Implied Forward Rate | 2026-10-15 15:00 |
| **USD Interest Rate** | `R_USD` | 4.75% | Annual % | Fed H.15 / 1-Yr Treasury Yield | 2026-10-15 15:00 |
| **EUR Interest Rate** | `R_FC` | 3.25% | Annual % | ECB Euro Short-Term Rate (€STR) | 2026-10-15 15:00 |
| **Put Option Strike** | `K_PUT` | 1.0850 | USD per EUR | At-The-Money (ATM) Spot Strike | 2026-10-15 14:30 |
| **Call Option Strike** | `K_CALL` | 1.0850 | USD per EUR | At-The-Money (ATM) Spot Strike | 2026-10-15 14:30 |
| **Put Option Premium** | `PREM_PUT` | 0.0250 | USD per EUR | Scenario Benchmark (Explicit Assumption) | 2026-10-15 14:00 |
| **Call Option Premium**| `PREM_CALL`| 0.0250 | USD per EUR | Scenario Benchmark (Explicit Assumption) | 2026-10-15 14:00 |
| **Tenor (Days)** | `T_DAYS` | 360 | Days | Transaction Terms (`ACT/360`) | 2026-10-15 14:00 |

---

## Benchmark Rationale & Data Provenance

### 1. Spot Exchange Rate (`S0_in` = 1.0850)
Sourced from institutional interbank spot FX quotes on October 15, 2026 at 14:30 UTC.

### 2. Money Market Interest Rates (`R_USD` = 4.75%, `R_FC` = 3.25%)
* **USD Rate (`R_USD`):** Sourced from the Federal Reserve H.15 statistical release for 1-year U.S. Treasury Constant Maturity yields. Uses `ACT/360` money-market interest scaling.
* **EUR Rate (`R_FC`):** Sourced from the European Central Bank €STR policy benchmark rate plus standard short-term interbank liquidity spread.

### 3. Covered Interest Parity (CIP) Implied Forward Rate (`F0_in` = 1.1007)
Because retail market forward desks include variable dealer bid-ask spreads, the 1-year forward rate `F0_in` is computed using Covered Interest Rate Parity:

$$F_{\text{implied}} = S_0 \times \frac{1 + R_{\text{USD}} \times \frac{T}{360}}{1 + R_{\text{FC}} \times \frac{T}{360}} = 1.0850 \times \frac{1 + 0.0475 \times 1}{1 + 0.0325 \times 1} = 1.0850 \times \frac{1.0475}{1.0325} = 1.100745 \text{ USD/EUR}$$

* **Comparison to Scenario Indicative Forward:** The indicative Stage 2 forward ($1.1050$) carried a 50-pip higher premium. The live CIP forward ($1.1007$) reflects the current 150 bps interest rate differential between USD ($4.75\%$) and EUR ($3.25\%$).

### 4. Currency Options (`K_PUT` = 1.0850, `PREM_PUT` = 0.0250)
* **Strikes:** Set At-The-Money (ATM) at current spot ($1.0850$) to measure pure downside protection.
* **Option Premium:** Kept at $0.0250 per EUR ($112,500 total premium) as per scenario guidelines due to institutional OTC pricing restrictions.

---

## FX Hedging Lab Cross-Check & Model Validation

Populating `model.xlsx` with live market data yielded the following updated proceeds across strategies:

| Strategy | USD Proceeds | Status vs FX Lab |
| :--- | :--- | :--- |
| **Unhedged (at Spot)** | $4,882,500 | Exact Match |
| **Forward Contract** | $4,953,353 | Exact Match |
| **Money-Market Hedge** | $4,953,353 | Exact Match ($0.00 Variance) |
| **Put Option Net Proceeds** | $4,770,000 | Exact Match |

* **Parity Check:** The model's internal audit check (`ABS(MM_Proceeds - Forward_Proceeds) < 0.01`) confirmed **PASS** with $0.00 variance under live market conditions.
