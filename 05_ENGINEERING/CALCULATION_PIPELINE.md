# Calculation Pipeline

**Product:** SME Business Valuation & Intelligence Platform
**Version:** 1.0
**Status:** Pre-Development

---

# 1. Purpose

This document defines the complete processing pipeline from raw business data to the final valuation and intelligence output.

---

# 2. Pipeline Overview

```text
RAW DATA
   ↓
SCHEMA VALIDATION
   ↓
BUSINESS VALIDATION
   ↓
FINANCIAL VALIDATION
   ↓
NORMALIZATION
   ↓
METRIC CALCULATION
   ↓
RISK ASSESSMENT
   ↓
REVENUE QUALITY
   ↓
BENCHMARK RETRIEVAL
   ↓
METHOD ELIGIBILITY
   ↓
VALUATION METHODS
   ↓
RISK ADJUSTMENTS
   ↓
METHOD WEIGHTING
   ↓
FINAL VALUATION
   ↓
RANGE
   ↓
CONFIDENCE
   ↓
VALUE DRIVERS
   ↓
RECOMMENDATIONS
   ↓
PERSIST SNAPSHOT
   ↓
AI INTERPRETATION
```

---

# 3. Stage 1 — Data Ingestion

Sources:

* User forms
* Imported financial data
* CSV
* API integrations in future
* Accounting systems in future

Raw data should be stored separately from derived calculations.

---

# 4. Stage 2 — Schema Validation

Check:

* Data types
* Required fields
* Currency
* Dates
* Numeric ranges
* IDs

Invalid schema:

```text
revenue: "one hundred million"
```

Valid:

```text
revenue: 100000000
```

---

# 5. Stage 3 — Business Validation

Check:

* Industry exists
* Currency exists
* Business stage is valid
* Employee count is valid
* Country is valid

---

# 6. Stage 4 — Financial Validation

Check relationships.

Example:

```
GrossProfit ≈ Revenue - COGS
```

and:

```
EBITDA ≈ GrossProfit - OperatingExpenses
```

Differences may occur because of accounting treatments.

Therefore the system should distinguish:

```text
Hard validation failure
Soft warning
Informational discrepancy
```

---

# 7. Stage 5 — Normalization

Prepare financial data for analysis.

Potential adjustments:

* One-time expenses
* Owner compensation
* Extraordinary income
* Non-operating items

Every adjustment must be explicit.

---

# 8. Stage 6 — Metric Calculation

Calculate:

* Revenue growth
* CAGR
* Gross margin
* EBITDA margin
* Net margin
* Debt ratios
* Liquidity ratios
* Cash-flow metrics

Derived values should not be treated as user-entered source data.

---

# 9. Stage 7 — Revenue Quality

Analyze:

* Recurring revenue
* Revenue stability
* Customer concentration
* Retention
* Diversification

Output:

```text
Revenue Quality Score
```

---

# 10. Stage 8 — Risk

Calculate risk dimensions.

```text
Financial
Customer
Revenue
Operational
Market
Owner Dependency
Supplier
Compliance
```

Output:

```text
Risk Score
Risk Level
Risk Factors
```

---

# 11. Stage 9 — Benchmark Retrieval

Retrieve benchmark data according to:

```text
Industry
Country/market
Metric
Business size
Dataset date
```

Benchmark data must contain provenance.

---

# 12. Stage 10 — Method Eligibility

Determine which valuation methods can be used.

Example:

```text
Revenue Multiple      ✓
EBITDA Multiple       ✓
Asset Based           ✓
DCF                   ⚠ insufficient data
Comparable Analysis   ✓
```

---

# 13. Stage 11 — Individual Valuations

Run each eligible method independently.

Example:

```text
Revenue Method
= ₦80M

EBITDA Method
= ₦120M

Asset Method
= ₦63M
```

---

# 14. Stage 12 — Adjustments

Apply approved adjustment rules.

Each adjustment must be represented independently.

```text
Raw Value
    ↓
Growth Adjustment
    ↓
Risk Adjustment
    ↓
Quality Adjustment
    ↓
Adjusted Value
```

Avoid combining adjustments invisibly.

---

# 15. Stage 13 — Method Weighting

Calculate weighted value.

```
V_final = Σ (w_i × V_i)
```

Weights must sum to 1.

---

# 16. Stage 14 — Enterprise/Equity Conversion

Calculate:

```
EquityValue = EnterpriseValue + Cash - Debt
```

Subject to the methodology's treatment of debt-like and non-operating items.

---

# 17. Stage 15 — Range

Determine:

```text
Low
Central
High
```

Range construction must be documented.

---

# 18. Stage 16 — Confidence

Calculate confidence from:

* Data completeness
* Data consistency
* Historical depth
* Benchmark quality
* Method agreement

---

# 19. Stage 17 — Value Drivers

Identify the strongest positive and negative factors.

Example:

```text
Positive:
EBITDA margin +++
Revenue growth ++

Negative:
Customer concentration ---
Owner dependency --
```

---

# 20. Stage 18 — Recommendations

Transform identified weaknesses into actionable recommendations.

Example:

```text
Risk:
High customer concentration

Recommendation:
Increase customer diversification.

Priority:
High
```

Recommendations should reference the underlying metric/risk.

---

# 21. Stage 19 — Snapshot

Persist:

```text
Input snapshot
Metric snapshot
Benchmark snapshot
Methodology version
Engine version
Calculation results
Adjustments
Final valuation
```

---

# 22. Stage 20 — AI Interpretation

Only after deterministic processing:

```text
Structured Results
       ↓
AI Context Builder
       ↓
AI Model
       ↓
Explanation
```

AI input should contain structured facts rather than unrestricted raw database access.

---

# 23. Failure Handling

If a stage fails:

```text
Validation failure
→ stop valuation

Benchmark failure
→ disable affected method

AI failure
→ valuation remains available

Report failure
→ valuation remains available
```

Non-critical services must not destroy the core valuation result.

---

# 24. Pipeline Transaction Boundary

Core valuation persistence should be transactional.

If a completed valuation cannot be stored consistently, mark the operation as failed rather than presenting a partial valuation.

---

# 25. Pipeline Observability

Track:

```text
pipeline_started
validation_completed
metrics_completed
risk_completed
benchmarks_loaded
methods_completed
valuation_completed
snapshot_saved
ai_completed
```

Each run should have a:

```text
pipelineRunId
```

---

# 26. Performance

Simple valuation:

```text
Synchronous
```

Complex calculations:

```text
Asynchronous Job
```

Examples:

* Monte Carlo
* Large benchmark analysis
* Complex reports
* Batch valuations

---

# 27. Reproducibility

A valuation should be reproducible from:

```text
Input Snapshot
+
Benchmark Snapshot
+
Methodology Version
+
Engine Version
```

---

# 28. Final Pipeline Contract

The pipeline must produce:

```text
Valuation
Risk
Health
Confidence
Value Drivers
Recommendations
Audit Trail
```

The platform should never return only a valuation number without its supporting context.

---

# 29. Final Principle

> **Every output must have a traceable path back to validated inputs.**