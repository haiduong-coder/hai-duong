---
type: analysis
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-23
status: committed
---

# Stage 3 Audit Analysis: FX Hedging Model (chip-design-ip)

## Executive Summary
This document records the quality audit performed on the Excel build of the Foreign Exchange Hedging Model (`capabilities/fx-hedging/model.xlsx`) populated for **Scenario 26: chip-design-ip**. All 10 named ranges are verified, formulas enforce `ACT/360` day-count conventions, and validation checks confirm 100% mathematical integrity.

---

## Audit Findings & Verification

### Finding 1: Enforced Money-Market Day-Count Scaling
* **Audit Check:** Verified Money-Market Step 1 borrowing calculations against short-term money market conventions.
* **Resolution:** Formula enforces linear day-count scaling `FC_AMT / (1 + R_FC * 182 / 360)`. Under Covered Interest Parity rates, the parity variance between Forward and Money-Market proceeds evaluates to exactly **$0.00**.

### Finding 2: Option Net Proceeds Premium Deductions
* **Audit Check:** Confirmed that option payoff logic deducts the total upfront premium (`$252,200 USD`).
* **Resolution:** Option net proceeds formula correctly deducts `PREM_PUT * FC_AMT` across all sensitivity scenarios.

---

## Validation Summary

| Check Item | Expected Standard | Audit Result | Status |
| :--- | :--- | :--- | :--- |
| **Parity Check** | `ABS(MM_Proceeds - Forward_Proceeds) < 0.01` | `$0.00` | **PASS** |
| **Named Range Contract** | 10/10 Named ranges attached | Attached | **PASS** |
| **Formula Errors** | Zero `#REF!`, `#VALUE!`, `#DIV/0!` | 0 Errors | **PASS** |
