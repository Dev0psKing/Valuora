# Risk Scoring Methodology

## 1. Business Risk Score

The platform will calculate a Business Risk Score.

### Risk Categories

| Risk Category         | Weight |
| --------------------- | -----: |
| Financial Risk        |    20% |
| Customer Risk         |    15% |
| Owner Dependency      |    15% |
| Operational Risk      |    15% |
| Market Risk           |    10% |
| Revenue Stability     |    10% |
| Legal/Compliance Risk |     5% |
| Supplier Dependency   |    10% |

### Formula

```
Risk Score = Σ(Component Score × Component Weight)
```

Scores should be normalized to a common scale.

Example:

* 0 = Extremely High Risk
* 100 = Extremely Low Risk

or alternatively:

* 0 = Low Risk
* 100 = High Risk

The platform must maintain consistency in score interpretation.

---

## 2. Business Health Scoring

The platform will calculate an overall Business Health Score.

### Categories

* Financial Strength.
* Revenue Quality.
* Growth.
* Operational Maturity.
* Customer Strength.
* Risk.

### Formula

```
Business Health Score = (Financial Score × Weight)
                      + (Growth Score × Weight)
                      + (Operational Score × Weight)
                      + (Customer Score × Weight)
                      + (Risk Score × Weight)
```

The final score may be normalized to a 0–100 scale.

---

## 3. Valuation Confidence Score

The system should communicate uncertainty.

A valuation should not be presented as absolute truth.

The platform will calculate a Valuation Confidence Score.

### Factors

| Factor                       | Weight |
| ---------------------------- | -----: |
| Data Completeness            |    25% |
| Financial Consistency        |    20% |
| Historical Data Availability |    15% |
| Method Agreement             |    20% |
| Industry Data Quality        |    20% |

### Formula

```
Confidence Score = Σ(Factor Score × Weight)
```

### Example

Data Completeness: 90

Financial Consistency: 85

Historical Data: 70

Method Agreement: 75

Industry Data: 65

Final Confidence Score: approximately 78/100.

---

## 4. Method Agreement Analysis

Different valuation methods may produce significantly different results.

### Example

Revenue Method: ₦50M

EBITDA Method: ₦70M

DCF: ₦75M

Asset Method: ₦48M

The system should measure the dispersion between valuation methods.

Higher disagreement may reduce confidence.

Potential statistical measurements include:

* Range.
* Standard deviation.
* Coefficient of variation.

---

## 5. Coefficient of Variation

The coefficient of variation can measure relative dispersion.

### Formula

```
CV = Standard Deviation / Mean
```

A higher coefficient may indicate greater disagreement between valuation methods.

This could influence the confidence score.
