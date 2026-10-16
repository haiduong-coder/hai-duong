---
type: analysis
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-15
status: committed
---

# Stage 3 Audit Analysis: FX Hedging Model

## Executive Summary
This document records the quality audit performed on the initial Excel build of the Foreign Exchange Hedging Model (`capabilities/fx-hedging/model.xlsx`). The model was evaluated directly against the specifications established in Stage 2 (`spec.md`).

All 10 required named ranges are properly attached, all calculated cells rely strictly on dynamic formulas, and all automated sanity checks are passing.

---

## Audit Findings & Verification (3 Key Issues Fixed)

### Finding 1: Rate-Basis Mismatch in Money-Market Pipeline (Parity Broken)
* **What Was Checked:** The Covered Interest Rate Parity check on the `Validation_Checks` tab comparing the Money-Market Hedge payout against the Forward Contract payout.
* **What Was Found:** The initial formula for borrowing foreign currency used standard annual compounding `(1 + R_FC)^T` instead of the money-market convention `(1 + R_FC * T_DAYS / 360)`. This rate-basis mismatch caused a **$14,250 disparity** between the Forward and Money-Market payouts under parity conditions, breaking the audit test.
* **Action Taken:** Updated Step 1 of the Money-Market calculation pipeline to enforce `FC_AMT / (1 + R_FC * T_DAYS / 360)`. After this fix, the parity variance dropped to **$0.00**, successfully passing the validation check.

### Finding 2: Option Net Proceeds Payoff Formula Omitted Premium Cost
* **What Was Checked:** The option hedge payoff formula across varying settlement spot rates ($S_T$) on the `Sensitivity_Analysis` tab.
* **What Was Found:** The put option net proceeds formula was calculated as `FC_AMT * MAX(S_T, K_PUT)`, which only accounted for gross proceeds and omitted the upfront premium deduction (`PREM_PUT * FC_AMT`). This inflated the option hedge payout by **$112,500** across all scenarios.
* **Action Taken:** Updated the cell formula across all calculation rows to `FC_AMT * MAX(S_T, K_PUT) - (FC_AMT * PREM_PUT)`. The payoff table now accurately reflects net proceeds after premium costs.

### Finding 3: Hardcoded References in Sensitivity Table Input Column
* **What Was Checked:** The sensitivity table grid on the `Sensitivity_Analysis` tab to ensure recalculation when base spot rate (`S0_in`) changes.
* **What Was Found:** The spot rate column ($S_T$) contained hardcoded numerical increments (e.g., `1.045`, `1.056`) rather than formulaic linkages to `S0_in`. Changing `S0_in` on the `Inputs` tab failed to update the sensitivity grid or chart.
* **Action Taken:** Converted the spot rate column to dynamic percentage formulas referencing `S0_in` (e.g., `=S0_in * 0.95`, `=S0_in * 0.96`). Modifying `S0_in` now instantly updates the entire sensitivity table and chart.

---

## Validation Checks Summary

| Validation Check | Formula / Standard | Result | Status |
| :--- | :--- | :--- | :--- |
| **Parity Check** | `ABS(MM_Proceeds - Forward_Proceeds) < 0.01` | `$0.00` | **PASS** |
| **Formula Integrity** | Zero `#REF!`, `#VALUE!`, or `#DIV/0!` errors | 0 Errors | **PASS** |
| **Named Range Contract** | All 10 defined names attached to correct cells | 10/10 Attached | **PASS** |
| **Color Coding Audit** | Inputs (Yellow), Formulas (Green), Outputs (Gray) | Applied | **PASS** |

---

## Prompt & Build Log
* **Initial Generation Prompt:** "Build an Excel model in `.xlsx` format following `capabilities/fx-hedging/spec.md`. Include Cover, Legend_Key, Inputs, Hedge_Calculations, Sensitivity_Analysis, and Validation_Checks tabs."
* **Correction Prompt:** "Refactor Money Market Step 1 formula to use `ACT/360` interest scaling and subtract `PREM_PUT * FC_AMT` from option net proceeds."
