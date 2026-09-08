# Valuation Engine

**Product:** SME Business Valuation & Intelligence Platform
**Version:** 1.0
**Status:** Pre-Development / Engineering Specification

---

# 1. Purpose

The Valuation Engine is the core financial calculation system responsible for estimating the value of an SME.

It must be:

* Deterministic
* Reproducible
* Explainable
* Versioned
* Testable
* Auditable

---

# 2. High-Level Flow

```text
Business Data
      ↓
Financial Data
      ↓
Validation
      ↓
Financial Metrics
      ↓
Normalization
      ↓
Benchmark Retrieval
      ↓
Method Selection
      ↓
Individual Valuations
      ↓
Risk Adjustments
      ↓
Method Weighting
      ↓
Valuation Range
      ↓
Confidence
      ↓
Final Result
```

---

# 3. Required Inputs

The engine may require:

### Business

* Industry
* Country
* Business age
* Business model
* Business stage
* Employee count

### Financial

* Revenue
* COGS
* EBITDA
* EBIT
* Net income
* Assets
* Liabilities
* Debt
* Cash
* Capex
* Working capital

### Qualitative

* Revenue quality
* Customer concentration
* Owner dependency
* Supplier dependency
* Operational risk
* Market risk

### Benchmark

* Industry multiple
* Country/market
* Metric type
* Dataset date
* Data quality

---

# 4. Input Validation

Before valuation:

```text
IF revenue < 0
    reject

IF assets < 0
    review/reject depending on field

IF EBITDA inconsistent
    warning or reject

IF required data missing
    determine whether selected method can proceed
```

---

# 5. Financial Metrics

## Revenue Growth

For two periods:

```
Growth = (Revenue_t - Revenue_t-1) / Revenue_t-1
```

---

## CAGR

For n periods:

```
CAGR = (Revenue_final / Revenue_initial)^(1/n) - 1
```

---

## Gross Margin

```
Gross Margin = (Revenue - COGS) / Revenue
```

---

## EBITDA Margin

```
EBITDA Margin = EBITDA / Revenue
```

---

## Net Margin

```
Net Margin = Net Income / Revenue
```

---

# 6. Normalized EBITDA

Reported EBITDA may contain:

* Owner compensation distortions
* One-time expenses
* Non-recurring income
* Exceptional costs
* Personal expenses

Conceptually:

```
Normalized EBITDA = Reported EBITDA + Adjustments
```

Every adjustment must be documented.

Example:

```text
Reported EBITDA        ₦25M
Owner adjustment       +₦3M
One-time expense       +₦2M
Normalized EBITDA      ₦30M
```

Adjustments must never be invented automatically without a defined methodology.

---

# 7. Revenue Multiple Method

Formula:

```
EV = Revenue × Multiple
```

Example:

```text
Revenue = ₦100M
Multiple = 0.8×

EV = ₦80M
```

The multiple must come from an approved benchmark source/configuration.

---

# 8. EBITDA Multiple Method

Formula:

```
EV = EBITDA × EBITDA Multiple
```

Example:

```text
EBITDA = ₦30M
Multiple = 4×

EV = ₦120M
```

---

# 9. Asset-Based Method

Conceptually:

```
Equity Value = Fair Value of Assets - Liabilities
```

Where sufficient asset information exists.

Potential asset categories:

* Property
* Equipment
* Inventory
* Vehicles
* Intangible assets
* Cash

---

# 10. DCF Method

Basic enterprise value:

```
EV = Σ (FCF_t / (1 + r)^t) + TerminalValue / (1 + r)^n
```

Where:

* FCF = Free Cash Flow
* r = Discount Rate
* n = Forecast Period

Terminal value using perpetual growth:

```
TV = FCF_(n+1) / (r - g)
```

where:

```
FCF_(n+1) = FCF_n × (1 + g)
```

The model must enforce:

```
r > g
```

---

# 11. DCF Input Validation

The engine should validate:

* Forecast horizon
* Revenue growth
* EBITDA margin
* Tax assumptions
* Capex
* Working capital
* Discount rate
* Terminal growth

Extreme assumptions should trigger warnings.

---

# 12. Comparable Analysis

Comparable analysis may use:

* Industry
* Business size
* Geography
* Revenue
* EBITDA
* Growth
* Business model

The engine should record:

```text
Comparable source
Metric
Multiple
Date
Market
Quality
```

---

# 13. Method Eligibility

Not every method should always be available.

Example:

```text
IF EBITDA <= 0
    EBITDA multiple may be unavailable

IF insufficient asset data
    Asset method may be unavailable

IF insufficient cash-flow data
    DCF may be unavailable
```

The system should explain unavailable methods.

---

# 14. Method Reliability

Each method should receive a reliability score.

Potential factors:

* Data completeness
* Data quality
* Benchmark quality
* Method suitability
* Business characteristics

Example:

```text
EBITDA Multiple
Reliability: 88

Revenue Multiple
Reliability: 75

Asset Based
Reliability: 61
```

---

# 15. Risk Adjustment

A risk adjustment may be applied based on defined methodology.

Conceptually:

```
AdjustedValue = RawValue × AdjustmentFactor
```

Example:

```text
Raw Value       ₦100M
Adjustment      0.90
Adjusted Value  ₦90M
```

Every adjustment must contain:

* Factor
* Reason
* Category
* Source/methodology
* Version

---

# 16. Method Weighting

If multiple methods are available:

```
V = Σ (w_i × V_i)
```

subject to:

```
Σ w_i = 1
```

Example:

```text
Revenue Method     30%
EBITDA Method      50%
Asset Method       20%
```

The weighting methodology must be explicit and version-controlled.

---

# 17. Enterprise Value

Enterprise value represents the value of the operating business before equity adjustments.

---

# 18. Equity Value

Basic relationship:

```
EquityValue = EnterpriseValue + Cash - Debt
```

Additional debt-like items may be included depending on methodology.

---

# 19. Valuation Range

The platform should produce:

```text
Low
Central
High
```

The range may incorporate:

* Method dispersion
* Benchmark range
* Risk
* Data uncertainty
* Model uncertainty

The exact statistical construction must be specified and versioned before production.

---

# 20. Confidence Score

Confidence is not the same as valuation.

Potential inputs:

```text
Data completeness
Financial consistency
Historical depth
Benchmark quality
Method agreement
```

Example:

```
Confidence = w1×D + w2×C + w3×H + w4×B + w5×M
```

where the weights sum to 1.

---

# 21. Value Drivers

The engine should identify factors affecting the result.

Positive:

* Revenue growth
* Strong margins
* Recurring revenue
* Diversified customers
* Strong cash generation

Negative:

* Customer concentration
* Revenue volatility
* Owner dependency
* High debt
* Weak margins

---

# 22. Engine Output

Standard result:

```json
{
  "enterpriseValue": 75000000,
  "equityValue": 65000000,
  "range": {
    "low": 60000000,
    "central": 75000000,
    "high": 85000000
  },
  "confidenceScore": 78,
  "riskScore": 31,
  "methodologyVersion": "1.0",
  "engineVersion": "1.0.0"
}
```

---

# 23. Explanation Trace

Every result should be capable of producing a calculation trace.

Example:

```text
Revenue
₦100M

× Revenue Multiple
0.8×

= Raw Enterprise Value
₦80M

Risk Adjustment
0.94×

= Adjusted Enterprise Value
₦75.2M
```

---

# 24. Engine Invariants

The engine must enforce:

```text
Weights sum to 1

Enterprise value cannot be negative
unless methodology explicitly permits it

Currency must be known

Methodology version must exist

Inputs must be validated

Completed valuation must have a snapshot
```

---

# 25. Determinism

Given identical:

```text
Inputs
+
Benchmark dataset
+
Methodology version
+
Engine version
```

the engine should produce the same result.

---

# 26. Testing Requirements

Every financial formula must have:

* Normal case
* Zero case where valid
* Boundary case
* Invalid case
* Large-value case
* Precision test

---

# 27. Future Models

Future versions may introduce:

* Monte Carlo simulation
* Probabilistic valuation
* Machine-learning benchmarks
* Bayesian uncertainty
* Industry-specific models

These must remain versioned and independently validated.

---

# 28. Important Limitation

The formulas in this engineering specification define the software architecture and mathematical implementation direction.

They do **not** constitute professional valuation standards or a certified valuation methodology.

Before commercial deployment, the methodology should be reviewed and validated by appropriately qualified finance/valuation professionals.

---

# 29. Final Principle

> **The valuation engine must be explainable enough that an analyst can inspect the inputs, assumptions, formulas, adjustments and weights that produced the final result.**