---
type: memo
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-23
status: committed
---

# Market Data Sourcing Memo: EUR 13.0M Royalty Receivable (chip-design-ip)

## Executive Summary
This memorandum documents the market parameters retrieved on **October 23, 2026** for Scenario 26 (`chip-design-ip`). These inputs replace placeholder numbers in `capabilities/fx-hedging/model.xlsx`.

---

## Live Market Data Table

| Parameter | Symbol | Value | Unit | Data Source / Benchmark | Timestamp (UTC) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Receivable Amount** | `FC_AMT` | 13,000,000 | EUR | IP Royalty Invoice | 2026-10-23 14:00 |
| **Spot Exchange Rate** | `S0_in` | 1.0850 | USD per EUR | Interbank FX Feed | 2026-10-23 14:30 |
| **Indicative Forward** | `F0_in` | 1.1705 | USD per EUR | Scenario Contract Quote | 2026-10-23 14:30 |
| **CIP Implied Forward**| `F0_cip` | 1.0931 | USD per EUR | Calculated CIP Rate | 2026-10-23 15:00 |
| **USD Interest Rate** | `R_USD` | 4.75% | Annual % | Fed H.15 6-Month Yield | 2026-10-23 15:00 |
| **EUR Interest Rate** | `R_FC` | 3.25% | Annual % | ECB €STR Benchmark Rate | 2026-10-23 15:00 |
| **Put Option Strike** | `K_PUT` | 1.0850 | USD per EUR | ATM Strike | 2026-10-23 14:30 |
| **Call Option Strike** | `K_CALL` | 1.0850 | USD per EUR | ATM Strike | 2026-10-23 14:30 |
| **Put Option Premium** | `PREM_PUT` | 0.0194 | USD per EUR | Scenario Quote ($252,200 Total) | 2026-10-23 14:30 |
| **Call Option Premium**| `PREM_CALL`| 0.0296 | USD per EUR | Scenario Quote ($384,800 Total) | 2026-10-23 14:30 |
| **Tenor** | `T_DAYS` | 182 | Days | 6-Month Contract Horizon | 2026-10-23 14:00 |

---

## CIP Implied Forward Calculation

$$\text{F}_{\text{CIP}} = 1.0850 \times \frac{1 + \left(0.0475 \times \frac{182}{360}\right)}{1 + \left(0.0325 \times \frac{182}{360}\right)} = 1.0850 \times \frac{1.024014}{1.016431} = 1.093095 \text{ USD/EUR}$$

* **Indicative vs CIP Analysis:** The indicative forward quote ($1.1705$) reflects a substantial market forward premium over theoretical CIP ($1.0931$).
