---
type: analysis
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-23
status: committed
---

# Stage 5 Independent Validation Analysis (chip-design-ip)

## Executive Summary
This document presents the independent validation and step-by-step hand calculation verification for Scenario 26 (`chip-design-ip`), confirming full model accuracy across EUR 13,000,000 exposure calculations over a 182-day tenor.

---

## Hand-Verification Table (Step-by-Step Arithmetic)

### 1. Indicative Forward Contract Proceeds
* **Formula:** $\text{Proceeds} = \text{FC\_AMT} \times \text{F0\_in}$
* **Calculation:** $13,000,000 \times 1.1705 = \$15,216,500.00$
* **Workbook Reconciled Value:** **$15,216,500.00** (Variance: $0.00)

### 2. Money Market Hedge Pipeline (CIP Market Rates)
* **Step 1 — Borrow EUR:** $\frac{13,000,000}{1 + (0.0325 \times 182 / 360)} = \frac{13,000,000}{1.01643055} = 12,789,855.57 \text{ EUR}$
* **Step 2 — Spot Conversion:** $12,789,855.57 \times 1.0850 = \$13,876,993.29 \text{ USD}$
* **Step 3 — USD Investment:** $13,876,993.29 \times \left(1 + 0.0475 \times \frac{182}{360}\right) = 13,876,993.29 \times 1.02401389 = \$14,210,235.00$
* **Workbook Reconciled Value:** **$14,210,235.00** (Variance: $0.00)

### 3. Put Option Net Proceeds (at $S_T = 1.0850$)
* **Gross Proceeds:** $13,000,000 \times 1.0850 = \$14,105,000.00$
* **Premium Deducted:** $13,000,000 \times 0.0194 = \$252,200.00$
* **Net Proceeds:** $\$14,105,000.00 - \$252,200.00 = \$13,852,800.00$
* **Workbook Reconciled Value:** **$13,852,800.00** (Variance: $0.00)
