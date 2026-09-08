# Statistical Analysis

## 1. Data Normalization

Different business variables exist on different numerical scales.

Example:

* Revenue: ₦500,000,000
* Growth: 25%
* Employees: 40
* Risk Score: 68

For statistical models, variables may require normalization.

### Min-Max Normalization

```
Normalized Value = (X − Minimum) / (Maximum − Minimum)
```

This converts values to a range between 0 and 1.

### Z-Score Standardization

```
Z = (X − Mean) / Standard Deviation
```

This may be useful when comparing business metrics with industry datasets.

---

## 2. Mean

```
Mean = Sum of All Values / Number of Values
```

Useful for:

* Average industry revenue.
* Average EBITDA margin.
* Average valuation multiples.

---

## 3. Median

The median represents the middle value in a sorted dataset.

Median may be preferable to mean when industry data contains extreme outliers.

### Example

Business multiples: 2×, 2.5×, 3×, 3.2×, 15×

The mean may be distorted by the 15× outlier.

The median provides a more representative central value.

---

## 4. Standard Deviation

Standard deviation measures the dispersion of data.

### Formula

```
Standard Deviation = √[Σ(X − Mean)² / N]
```

Potential use cases:

* Revenue volatility.
* Profit volatility.
* Industry multiple variability.
* Valuation confidence.

High volatility may indicate higher business risk.

---

## 5. Percentiles

Percentiles can be used to create valuation ranges.

### Example

| Percentile | Value  |
| ---------- | -----: |
| 10th       |   ₦50M |
| 50th       |   ₦70M |
| 90th       |   ₦95M |

The platform could present:

* Conservative Valuation: ₦50M
* Expected Valuation: ₦70M
* Optimistic Valuation: ₦95M

---

## 6. Outlier Detection

The platform should eventually detect unusual financial inputs.

Examples:

* Revenue significantly higher than industry benchmarks.
* EBITDA margin unusually high.
* Negative operating expenses.
* Unrealistic growth rates.

Statistical methods may include:

* Z-score analysis.
* Interquartile range.
* Standard deviation thresholds.

Outliers should trigger validation warnings rather than automatically rejecting user data.

---

## 7. Correlation Analysis

As historical data grows, the platform may analyse relationships between variables.

Example:

* Does revenue growth correlate with valuation?
* Does recurring revenue correlate with higher valuation multiples?

### Correlation Coefficient

```
−1 ≤ r ≤ +1
```

Where:

* +1 = Strong positive relationship.
* 0 = No relationship.
* −1 = Strong negative relationship.

---

## 8. Regression Analysis

Regression can eventually allow the platform to identify statistical relationships between business characteristics and valuations.

### Example

```
Valuation = β0 + β1(Revenue) + β2(EBITDA) + β3(Growth) + β4(Risk)
```

Possible future models include:

* Linear Regression.
* Multiple Regression.
* Ridge Regression.
* Lasso Regression.
* Random Forest Regression.
* Gradient Boosting Regression.

Regression should not replace established valuation methodologies during early development.

Instead, it may become an additional predictive layer.
