# UI/UX Specification

**Product:** SME Business Valuation & Intelligence Platform
**Version:** 1.0
**Status:** Pre-Development / Design Specification
**Audience:** Product Designer, UX Designer, Frontend Engineer, Product Manager, AI Builder

---

# 1. Document Purpose

This document defines the user experience, information architecture, interface structure, navigation, workflows, screens, interactions, states, and usability requirements for the SME Business Valuation & Intelligence Platform.

The purpose is to ensure that the application is:

* Easy for non-financial users to understand
* Powerful enough for analysts and advisors
* Credible for business owners and investors
* Transparent about how valuations are produced
* Visually professional
* Data-driven
* Responsive
* Accessible
* Consistent across the entire application

The interface must communicate:

> **"Complex financial intelligence, made understandable."**

The product should not feel like a generic accounting dashboard.

It should feel like a **professional business intelligence and valuation platform**.

---

# 2. Design Objectives

The UI/UX must accomplish six primary objectives.

## 2.1 Clarity

Users should immediately understand:

* What the platform does
* What information is required
* What their business is worth
* Why the valuation has that range
* What factors are affecting value
* What they can do to improve the business

---

## 2.2 Trust

Financial valuation is highly sensitive.

The interface must therefore communicate:

* Calculation transparency
* Data quality
* Methodology
* Assumptions
* Confidence
* Risk
* Limitations

The product must never present an estimate as an unquestionable fact.

Instead of:

> "Your business is worth ₦75,000,000."

Prefer:

> **Estimated Business Value**
> ₦60M – ₦85M
> Central estimate: ₦75M
> Confidence: 78%

---

## 2.3 Simplicity

The underlying platform may contain:

* Financial calculations
* Multiples
* DCF
* Risk models
* Benchmarking
* Statistical analysis
* Scenario modelling

The user interface should hide unnecessary complexity while making deeper information available when needed.

Use progressive disclosure:

```text
Simple result
      ↓
Explanation
      ↓
Methodology
      ↓
Detailed calculation
      ↓
Advanced assumptions
```

---

## 2.4 Actionability

The platform should not stop at:

> "Your business is worth X."

It should answer:

> "What is reducing your value?"

and:

> "What should you do next?"

Every major analysis should therefore lead to actionable recommendations.

---

## 2.5 Professionalism

The interface should communicate:

* Financial intelligence
* Operational sophistication
* Reliability
* Seriousness
* Executive quality

Avoid:

* Excessive gradients
* Cartoon illustrations
* Overly playful interfaces
* Excessive animations
* Generic SaaS templates
* Decorative elements with no purpose

---

## 2.6 Accessibility

The application should support:

* Keyboard navigation
* Screen readers
* Adequate contrast
* Clear labels
* Visible focus states
* Error explanations
* Responsive layouts
* Accessible charts

Target:

**WCAG 2.1 AA**

---

# 3. Primary User Personas

## 3.1 SME Owner

### Goal

Understand:

* Business value
* Financial health
* Major risks
* Growth opportunities

### Technical knowledge

Low–medium.

### Primary interface requirement

Simple language and clear explanations.

---

## 3.2 Business Analyst

### Goal

Analyze financial performance and produce valuation reports.

### Technical knowledge

Medium–high.

### Primary interface requirement

Detailed metrics, methodology and calculation transparency.

---

## 3.3 Advisor / Consultant

### Goal

Analyze multiple businesses and provide recommendations.

### Primary interface requirement

Multi-business management, reports, comparisons and client-ready outputs.

---

## 3.4 Investor

### Goal

Evaluate potential businesses.

### Primary interface requirement

Fast access to:

* Financial performance
* Risk
* Valuation
* Growth
* Confidence
* Business fundamentals

---

# 4. Information Architecture

Primary application structure:

```text
APPLICATION
│
├── Dashboard
│
├── Businesses
│   ├── Business Overview
│   ├── Financials
│   ├── Metrics
│   ├── Risk
│   ├── Health
│   ├── Valuation
│   ├── Scenarios
│   ├── Recommendations
│   └── Reports
│
├── Benchmarks
│
├── Valuations
│
├── Reports
│
├── Settings
│
└── Account
```

---

# 5. Global Navigation

Desktop navigation:

```text
┌────────────────────────────────────────────────────┐
│ LOGO                         Search    Profile      │
├───────────────┬────────────────────────────────────┤
│ Dashboard     │                                    │
│ Businesses    │                                    │
│ Valuations    │             MAIN CONTENT           │
│ Benchmarks    │                                    │
│ Reports       │                                    │
│               │                                    │
│ ───────────   │                                    │
│ Settings      │                                    │
│ Help          │                                    │
└───────────────┴────────────────────────────────────┘
```

Mobile:

```text
┌────────────────────────────┐
│ Logo                 Menu  │
├────────────────────────────┤
│                            │
│       MAIN CONTENT         │
│                            │
└────────────────────────────┘
```

Bottom navigation may be used on mobile for the most important actions.

---

# 6. Onboarding Experience

## 6.1 Objective

The onboarding process should collect the minimum information required to begin analysis without overwhelming the user.

---

## 6.2 Onboarding Flow

```text
Create Account
      ↓
Verify Email
      ↓
Create Business
      ↓
Business Profile
      ↓
Financial Information
      ↓
Risk Questionnaire
      ↓
Data Validation
      ↓
Generate First Analysis
      ↓
Dashboard
```

---

# 7. Business Creation

Required information:

* Business name
* Industry
* Country
* Currency
* Business model
* Business age
* Employee count

Optional:

* Description
* Location
* Ownership structure
* Revenue model

---

# 8. Financial Data Entry

The financial entry interface should support both:

### Guided entry

For business owners.

### Advanced entry

For accountants and analysts.

---

## 8.1 Guided Financial Form

Organize information into sections.

### Revenue

* Total revenue
* Recurring revenue
* One-time revenue
* Revenue by segment

### Costs

* COGS
* Operating expenses
* Payroll
* Rent
* Marketing
* Other operating expenses

### Profitability

* EBITDA
* EBIT
* Net income

### Balance Sheet

* Cash
* Assets
* Liabilities
* Debt

### Cash Flow

* Operating cash flow
* Capital expenditure
* Working capital changes

---

# 9. Data Validation UX

Validation should happen both:

### During entry

and

### Before valuation.

Example:

```text
Revenue
₦100,000,000

COGS
₦40,000,000

Operating Expenses
₦30,000,000

EBITDA
₦30,000,000

✓ Financials appear internally consistent
```

If inconsistent:

```text
⚠ Review required

Reported EBITDA does not align with the
revenue and operating expense figures entered.

[Review calculation]
```

Never silently modify user-entered financial data.

---

# 10. Dashboard

The dashboard is the primary intelligence surface.

It should answer five questions immediately:

1. How healthy is my business?
2. What is it worth?
3. How confident is the estimate?
4. What risks matter?
5. What should I do next?

---

## 10.1 Dashboard Structure

```text
BUSINESS: Uwabor Logistics
────────────────────────────────────────────

Business Health          Estimated Value
82 / 100                  ₦60M – ₦85M

Confidence                Risk
78%                       Moderate

────────────────────────────────────────────

Financial Performance
Revenue     EBITDA       Growth       Margin
₦100M       ₦30M        +18%         30%

────────────────────────────────────────────

VALUATION
Central Estimate
₦75,000,000

[View Valuation]

────────────────────────────────────────────

KEY VALUE DRIVERS

↑ Revenue growth
↑ EBITDA margin
↓ Customer concentration
↓ Owner dependency

────────────────────────────────────────────

TOP RECOMMENDATIONS

1. Reduce customer concentration
2. Improve recurring revenue
3. Increase operating margin

[View Recommendations]
```

---

# 11. Business Health Score

Display:

```text
82
Business Health
GOOD
```

Breakdown:

```text
Financial Health       85
Growth                 78
Profitability          88
Revenue Quality        74
Operations             80
Risk                   72
```

The score should always have an explanation.

Example:

> Your business has strong profitability and growth, but customer concentration is reducing the overall health score.

---

# 12. Valuation Experience

The valuation page is the most important screen in the product.

---

## 12.1 Primary Valuation Card

```text
ESTIMATED BUSINESS VALUE

₦60M ───────── ₦75M ───────── ₦85M

Low              Central             High

Confidence: 78%

Enterprise Value: ₦75M
Equity Value: ₦65M

[Explore Valuation]
```

---

# 13. Valuation Range Visualization

Use a horizontal range visualization.

```text
LOW                 CENTRAL                 HIGH
│----------------------●----------------------│
₦60M                  ₦75M                   ₦85M
```

The central estimate should be visually prominent.

Do not use a gauge that implies false precision.

---

# 14. Valuation Methods

Display method contributions.

Example:

```text
VALUATION METHODS

Revenue Multiple
Raw Value       ₦72M
Adjusted Value  ₦68M
Weight          35%

EBITDA Multiple
Raw Value       ₦82M
Adjusted Value  ₦79M
Weight          45%

Asset Based
Adjusted Value  ₦63M
Weight          20%
```

Provide:

**"Why these methods?"**

This opens an explanation panel.

---

# 15. Method Explanation

Example:

```text
WHY EBITDA MULTIPLE?

This method was given greater weight because:

• Your business has positive EBITDA
• Earnings are relatively stable
• Your industry has usable benchmark data
• Historical financial data is available

[View methodology]
```

---

# 16. Confidence Score

Display:

```text
CONFIDENCE

78 / 100

HIGH
```

Breakdown:

```text
Data completeness       90
Financial consistency   88
Historical data         75
Benchmark quality       70
Method agreement        77
```

Explain the limitations.

Example:

> Confidence is reduced because industry benchmark data is limited for your market.

---

# 17. Risk Experience

Risk should not be presented as a single number alone.

Example:

```text
RISK PROFILE

Overall Risk
31 / 100
Moderate

Financial          Low
Customer           High
Operational        Medium
Market             Medium
Owner Dependency   High
Compliance         Low
```

Clicking a category opens detailed analysis.

---

# 18. Value Drivers

The platform should identify factors that influence valuation.

Example:

```text
VALUE DRIVERS

Positive
↑ Revenue growth
↑ EBITDA margin
↑ Recurring revenue

Negative
↓ Customer concentration
↓ Owner dependency
↓ Revenue stability
```

Each driver should provide:

* Direction
* Magnitude where defensible
* Explanation
* Recommended action

---

# 19. Recommendations

Recommendations should be prioritized.

```text
HIGH PRIORITY

Reduce customer concentration

Why it matters:
A significant portion of revenue comes from a
small number of customers.

Potential effect:
Lower business risk and potentially stronger
valuation confidence.

Recommended action:
Develop additional customer acquisition channels.

[Mark as actioned]
```

---

# 20. Scenario Analysis

Users should be able to model:

* Bear case
* Base case
* Bull case
* Custom scenario

Example:

```text
SCENARIO BUILDER

Revenue Growth
Base: 18%
Scenario: 25%

EBITDA Margin
Base: 30%
Scenario: 34%

Industry Multiple
Base: 4.5x
Scenario: 5.0x

[Run Scenario]
```

Output:

```text
BASE CASE       ₦75M
UPSIDE CASE     ₦98M
DOWNSIDE CASE   ₦54M
```

Scenarios must clearly indicate that they are simulations rather than forecasts or guarantees.

---

# 21. Benchmark Experience

Benchmarking should allow users to compare their business with available reference data.

Example:

```text
YOUR BUSINESS

Revenue Growth       18%
Industry Median      12%

EBITDA Margin        30%
Industry Median      22%

Revenue Multiple     4.2x
Industry Median      3.8x
```

Use plain-language interpretation:

> Your EBITDA margin is above the available industry benchmark.

---

# 22. Report Generation

Users should be able to generate:

* Executive report
* Detailed valuation report
* Investor summary
* Business health report

Flow:

```text
Select Report
      ↓
Review Data
      ↓
Generate
      ↓
Processing
      ↓
Preview
      ↓
Download / Share
```

---

# 23. AI Assistant Experience

The AI assistant should be positioned as an **interpreter of the analysis**, not the financial calculation engine.

Example:

User:

> Why is my valuation lower than I expected?

AI:

> The primary reasons are customer concentration, moderate revenue volatility, and a lower industry benchmark multiple. Your profitability is a positive factor.

The assistant should be able to explain:

* Valuation
* Risk
* Health score
* Metrics
* Recommendations
* Scenarios

It must not invent:

* Financial figures
* Benchmark data
* Valuation multiples
* Calculations
* Assumptions

---

# 24. Empty States

Empty states should guide users.

Example:

```text
NO VALUATION YET

Your business needs financial information
before we can estimate its value.

[Add Financial Data]
```

Avoid empty screens that simply say:

> "No data."

---

# 25. Loading States

Long-running processes should communicate progress.

Example:

```text
ANALYZING YOUR BUSINESS

✓ Validating financial data
✓ Calculating financial metrics
● Evaluating valuation methods
○ Assessing risk
○ Preparing results
```

---

# 26. Error States

Errors should be:

* Specific
* Human-readable
* Actionable

Bad:

> Error 422.

Good:

> We couldn't calculate the valuation because EBITDA is missing for the most recent financial period.

[Add EBITDA]

---

# 27. Confirmation States

Important actions should provide confirmation.

Example:

> Valuation generated successfully.

> Methodology version: 1.0
> Generated: September 8, 2026

[View Valuation]

---

# 28. Responsive Design

The platform must support:

* Desktop
* Laptop
* Tablet
* Mobile

Desktop:

```text
Sidebar + Main Content
```

Tablet:

```text
Collapsed Sidebar
```

Mobile:

```text
Single-column layout
```

Charts must resize without losing readability.

Tables should support horizontal scrolling where necessary.

---

# 29. Mobile Priorities

Mobile users should immediately access:

1. Business health
2. Valuation
3. Risk
4. Recommendations

Advanced financial tables may use expandable sections.

---

# 30. Accessibility Requirements

Required:

* Semantic HTML
* Keyboard navigation
* Focus indicators
* Accessible form labels
* Screen-reader-friendly charts
* Sufficient contrast
* Error messages associated with fields
* No information conveyed by color alone

Example:

Instead of:

> Red = High Risk

Use:

> HIGH RISK — Customer concentration

with visual styling as secondary information.

---

# 31. UX Principles

The product should follow:

### Principle 1 — Explain before overwhelming

Show the conclusion first.

### Principle 2 — Never hide assumptions

Users should be able to inspect important assumptions.

### Principle 3 — Never imply false precision

Valuation is an estimate.

### Principle 4 — Every number needs context

A number without explanation can be misleading.

### Principle 5 — Recommendations must be actionable

Don't simply identify problems.

### Principle 6 — Preserve history

Users should be able to compare valuations over time.

### Principle 7 — Separate calculation from interpretation

The financial engine produces the result.

The interface and AI explain it.

---

# 32. Primary User Journey

```text
LANDING PAGE
      ↓
CREATE ACCOUNT
      ↓
CREATE BUSINESS
      ↓
ENTER FINANCIALS
      ↓
VALIDATE DATA
      ↓
RISK ASSESSMENT
      ↓
GENERATE VALUATION
      ↓
DASHBOARD
      ↓
UNDERSTAND VALUE
      ↓
IDENTIFY RISKS
      ↓
REVIEW RECOMMENDATIONS
      ↓
RUN SCENARIOS
      ↓
GENERATE REPORT
```

---

# 33. Success Metrics

UX success should be measured through:

* Onboarding completion rate
* Financial data completion rate
* Valuation generation rate
* Time to first valuation
* Report generation rate
* Scenario usage
* Recommendation engagement
* Error rate
* User retention
* User satisfaction

Primary activation event:

> **User successfully generates their first valuation.**

---

# 34. Design Quality Bar

Before a screen is considered complete, ask:

### Does the user know:

* Where they are?
* What they're looking at?
* What the number means?
* Why it matters?
* What caused it?
* What they can do next?

If not, the screen is incomplete.

---

# 35. Final UX Principle

The platform may contain sophisticated mathematics and financial models.

The user should not feel the mathematical complexity.

The experience should communicate:

> **Simple to use. Serious underneath.**

That is the core UX philosophy of the product.