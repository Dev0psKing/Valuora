# Mathematical Requirements

## 1. Basic Business Mathematics

Basic mathematics forms the foundation of the valuation engine.

The system must support calculations involving:

* Addition.
* Subtraction.
* Multiplication.
* Division.
* Percentages.
* Ratios.
* Percentage changes.
* Weighted averages.
* Compound growth.

These calculations will be used throughout the platform.

---

## 2. Percentage Calculations

Percentages will be used to measure:

* Revenue growth.
* Profit margins.
* Debt ratios.
* Customer concentration.
* Recurring revenue.
* Business risk.
* Valuation changes.

### Formula

Percentage:

```
Percentage = (Part / Total) × 100
```

### Example

If recurring revenue is ₦40M and total revenue is ₦100M:

```
Recurring Revenue Percentage = (40 / 100) × 100 = 40%
```

---

## 3. Percentage Change

The system must calculate changes between historical periods.

### Formula

```
Percentage Change = (New Value − Old Value) / Old Value × 100
```

### Example

Previous revenue: ₦100M

Current revenue: ₦120M

```
Revenue Growth = (120M − 100M) / 100M × 100 = 20%
```

The platform will use percentage change for:

* Revenue growth.
* EBITDA growth.
* Profit growth.
* Valuation growth.
* Customer growth.

---

## 4. Financial Ratios

Financial ratios are essential for analysing business performance.

The platform should calculate financial ratios automatically where sufficient data is available.

### 4.1 Gross Profit Margin

```
Gross Profit Margin = Gross Profit / Revenue × 100
```

Used to evaluate:

* Production efficiency.
* Cost structure.
* Pricing effectiveness.

### 4.2 EBITDA Margin

```
EBITDA Margin = EBITDA / Revenue × 100
```

Used to evaluate:

* Operational profitability.
* Earnings quality.
* Comparison with industry benchmarks.

### 4.3 Net Profit Margin

```
Net Profit Margin = Net Profit / Revenue × 100
```

Used to evaluate overall business profitability.

### 4.4 Debt-to-Equity Ratio

```
Debt-to-Equity Ratio = Total Debt / Total Equity
```

Used to evaluate financial leverage.

### 4.5 Debt-to-EBITDA Ratio

```
Debt-to-EBITDA = Total Debt / EBITDA
```

Used to evaluate the company's ability to service debt.

### 4.6 Current Ratio

```
Current Ratio = Current Assets / Current Liabilities
```

Used to measure short-term liquidity.

---

## 5. Growth Mathematics

The valuation engine must evaluate historical and projected business growth.

### 5.1 Annual Growth Rate

```
Growth Rate = (Current Period Value − Previous Period Value) / Previous Period Value × 100
```

Used for:

* Revenue.
* EBITDA.
* Net income.
* Customers.

### 5.2 Compound Annual Growth Rate (CAGR)

```
CAGR = (Ending Value / Beginning Value)^(1 / Number of Years) − 1
```

Example:

Revenue grows from ₦50M to ₦100M over five years. The system calculates the average annual compound growth rate.

CAGR may be used to evaluate:

* Historical business growth.
* Industry growth.
* Revenue projections.

---

## 6. Weighted Averages

The valuation engine will combine different valuation methods.

A simple arithmetic average should not always be used.

Different methods may have different levels of reliability depending on:

* Business type.
* Industry.
* Data availability.
* Financial maturity.
* Asset intensity.

Therefore, the platform requires weighted averages.

### Formula

```
Weighted Value = Σ(Value × Weight)
```

### Example

| Method  | Valuation | Weight |
| ------- | --------: | -----: |
| EBITDA  |      ₦70M |    40% |
| Revenue |      ₦60M |    30% |
| DCF     |      ₦75M |    20% |
| Assets  |      ₦50M |    10% |

```
Weighted Valuation = (70 × 0.40) + (60 × 0.30) + (75 × 0.20) + (50 × 0.10) = ₦66M
```

---

## 7. Financial Statement Mathematics

The platform must understand the relationship between major financial statement components.

Core financial variables include:

```
Revenue
− Cost of Goods Sold
= Gross Profit
− Operating Expenses
= Operating Profit
+ Depreciation
+ Amortization
= EBITDA
− Interest
− Taxes
= Net Income
```

The platform must validate relationships between user-entered values.
