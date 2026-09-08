# Financial Modelling

## 1. Financial Normalization

Small businesses often contain financial information that does not accurately represent sustainable business performance.

Examples include:

* Personal expenses.
* Excessive owner compensation.
* One-time expenses.
* Exceptional income.
* Related-party transactions.

The system must support normalized financial calculations.

### Formula

```
Normalized EBITDA = Reported EBITDA + Add-backs − Adjustments
```

Possible add-backs include:

* One-time legal expenses.
* Personal expenses.
* Unusual operational expenses.

Possible deductions include:

* Missing market-rate owner compensation.
* Temporary unusual income.

The valuation engine may use normalized EBITDA instead of reported EBITDA.

---

## 2. Time Value of Money

The Discounted Cash Flow model requires understanding that money today is worth more than the same amount in the future.

The system must support:

* Present value.
* Future value.
* Compounding.
* Discounting.

### Present Value

```
PV = FV / (1 + r)^n
```

Where:

* PV = Present Value
* FV = Future Value
* r = Discount Rate
* n = Number of Periods

### Future Value

```
FV = PV × (1 + r)^n
```

---

## 3. Discounted Cash Flow Model

DCF estimates business value by calculating the present value of expected future cash flows.

The system will project future Free Cash Flow.

### Example

| Year   | Free Cash Flow |
| ------ | -------------: |
| Year 1 |           ₦10M |
| Year 2 |           ₦12M |
| Year 3 |           ₦15M |
| Year 4 |           ₦17M |
| Year 5 |           ₦20M |

Each cash flow is discounted.

### Formula

```
Present Value of Cash Flow = FCF / (1 + WACC)^n
```

The total enterprise value is calculated by adding:

* Present value of projected cash flows.
* Present value of terminal value.

---

## 4. Free Cash Flow

The platform must calculate Free Cash Flow where sufficient financial information exists.

### Simplified Formula

```
Free Cash Flow = EBIT × (1 − Tax Rate) + Depreciation − Capital Expenditure − Change in Working Capital
```

The exact implementation may vary depending on available business data.

---

## 5. Weighted Average Cost of Capital

WACC represents the average cost of financing a business through debt and equity.

### Formula

```
WACC = (E / (D+E) × Re) + (D / (D+E) × Rd × (1 − T))
```

Where:

* E = Market Value of Equity
* D = Market Value of Debt
* Re = Cost of Equity
* Rd = Cost of Debt
* T = Tax Rate

WACC may be used as the discount rate in DCF calculations.

---

## 6. Capital Asset Pricing Model

CAPM may be used to estimate the cost of equity.

### Formula

```
Re = Rf + Beta × (Rm − Rf)
```

Where:

* Rf = Risk-Free Rate
* Rm = Expected Market Return
* Beta = Market Risk Measure

For private SMEs, adjustments and alternative methodologies may be required.

---

## 7. Terminal Value

DCF models require estimating business value beyond the explicit forecast period.

One approach is the Gordon Growth Model.

### Formula

```
Terminal Value = FCF(n+1) / (WACC − Growth Rate)
```

Where:

* FCF(n+1) = Expected Free Cash Flow after forecast period.
* Growth Rate = Long-term sustainable growth.
