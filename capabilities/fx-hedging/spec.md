---
type: specification
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-23
status: committed
---

# Technical Specification: FX Hedging Model (chip-design-ip)

## Model Overview
This specification defines the input contract, formula logic, named ranges, and audit checks for the Foreign Exchange Hedging Model (`capabilities/fx-hedging/model.xlsx`).

---

## Named Range Input Contract

| Named Range | Description | Data Type | Default Value | Unit |
| :--- | :--- | :--- | :--- | :--- |
| `FC_AMT` | Foreign Currency Amount | Currency | `13000000` | EUR |
| `S0_in` | Spot Rate at Inception | Exchange Rate | `1.0850` | USD per EUR |
| `F0_in` | 182-Day Forward Rate | Exchange Rate | `1.1705` | USD per EUR |
| `R_USD` | USD Interest Rate | Percentage | `0.0475` | Annual % |
| `R_FC` | EUR Interest Rate | Percentage | `0.0325` | Annual % |
| `K_PUT` | Put Option Strike Price | Exchange Rate | `1.0850` | USD per EUR |
| `K_CALL` | Call Option Strike Price | Exchange Rate | `1.0850` | USD per EUR |
| `PREM_PUT` | Put Option Premium | Currency/Unit | `0.0194` | USD per EUR |
| `PREM_CALL` | Call Option Premium | Currency/Unit | `0.0296` | USD per EUR |
| `T_DAYS` | Days to Settlement | Integer | `182` | Days |

---

## Required Tab Architecture & Color Standards

1. **`Cover`**: Project metadata, status, author, and scenario tag.
2. **`Legend_Key`**: Color standards (Yellow = Input, Green = Calculation, Gray = Summary).
3. **`Inputs`**: Named Range input contract table.
4. **`Hedge_Calculations`**: Core strategy formulas.
5. **`Sensitivity_Analysis`**: Scenario grid across spot movements ($\pm 5\%$).
6. **`Validation_Checks`**: Parity audit checks.

---

## Calculation Formula Specifications

1. **Unhedged Proceeds:** `=FC_AMT * S0_in`
2. **Forward Contract Proceeds:** `=FC_AMT * F0_in`
3. **Money Market Hedge Pipeline (`ACT/360` scaling):**
   * *Step 1 (EUR Borrowed):* `=FC_AMT / (1 + R_FC * T_DAYS / 360)`
   * *Step 2 (USD Spot Converted):* `=Step1_Result * S0_in`
   * *Step 3 (Final USD Proceeds):* `=Step2_Result * (1 + R_USD * T_DAYS / 360)`
4. **Option Net Proceeds:** `=FC_AMT * MAX(S_T, K_PUT) - (FC_AMT * PREM_PUT)`
5. **Covered Interest Parity Implied Forward:** `=S0_in * (1 + R_USD * T_DAYS / 360) / (1 + R_FC * T_DAYS / 360)`
