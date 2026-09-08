# Development Roadmap

## System Design

## A. USER INTERFACE LAYER

The user-facing application.

Users interact with:

* Dashboard.
* Business profiles.
* Financial input forms.
* Risk assessment.
* Valuation reports.
* Scenario simulator.
* Insights.

Recommended technologies:

* Next.js.
* React.
* TypeScript.
* Tailwind CSS.

---

## B. API LAYER

The API layer connects the frontend with the business logic.

Responsibilities:

* Authentication.
* Data validation.
* Business data management.
* Valuation requests.
* Report generation.
* Subscription management.

---

## C. VALUATION ENGINE

The core calculation system.

Modules:

```text
ValuationEngine
|
|-- InputValidator
|
|-- FinancialNormalizer
|
|-- BusinessClassifier
|
|-- RevenueValuationEngine
|
|-- EBITDAMultipleEngine
|
|-- DCFEngine
|
|-- AssetValuationEngine
|
|-- RiskAssessmentEngine
|
|-- MultipleAdjustmentEngine
|
|-- MethodWeightingEngine
|
|-- EnterpriseValueCalculator
|
|-- EquityValueCalculator
|
|-- ConfidenceEngine
|
|-- ScenarioEngine
|
+-- RecommendationEngine
```

The valuation engine should remain independent from the frontend.

This allows the company to eventually provide:

* Web application.
* Mobile application.
* API access.
* Accountant dashboards.
* Financial institution integrations.

---

## Technology Strategy

### Initial Technology Stack

Frontend:

* Next.js.
* React.
* TypeScript.
* Tailwind CSS.

Backend:

* Node.js.
* TypeScript.

Database:

* PostgreSQL or Firebase/Firestore for early MVP.

Calculation Engine:

* TypeScript initially.
* Python for advanced financial modelling and data science later.

Authentication:

* Firebase Authentication or Clerk.

Payments:

* Paystack.
* Flutterwave.

Reports:

* PDF generation service.

AI:

* OpenAI or other LLM providers for explanations and report narratives.

---

## AI Strategy

Artificial intelligence should support the product rather than replace the financial valuation engine.

The valuation itself should primarily be produced using deterministic financial models.

AI can assist with:

### Valuation Explanation

Explain why a business received a particular valuation.

### Financial Data Assistance

Help users understand requested financial information.

### Risk Identification

Identify unusual financial patterns.

### Report Writing

Generate readable professional reports.

### Recommendations

Explain potential business improvement opportunities.

### AI Business Advisor

Eventually:

> Why did my business valuation decrease this quarter?

The AI could analyse historical platform data and explain:

> Your estimated valuation decreased primarily because revenue growth slowed from 28% to 12%, while EBITDA margin declined from 16% to 11%.

---

## Development Roadmap

### Phase 1: MVP

Core functionality:

* User authentication.
* Business profile.
* Financial data input.
* EBITDA valuation.
* Revenue multiple valuation.
* Basic risk questionnaire.
* Valuation range.
* Basic dashboard.

---

### Phase 2: Advanced Valuation

Add:

* DCF.
* Asset valuation.
* Financial normalization.
* Method weighting.
* Confidence scoring.

---

### Phase 3: Business Intelligence

Add:

* Business health score.
* Value drivers.
* Risk dashboard.
* Historical valuation tracking.

---

### Phase 4: Value Improvement Engine

Add:

* Improvement recommendations.
* Value impact simulations.
* Business improvement plans.

---

### Phase 5: Professional Platform

Add:

* Professional PDF reports.
* Accountant dashboard.
* Consultant dashboard.
* Multi-client management.

---

### Phase 6: Data Intelligence

Add:

* Industry benchmarking.
* Comparable businesses.
* SME financial database.
* Market intelligence.
