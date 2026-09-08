# Frontend Architecture

**Product:** SME Business Valuation & Intelligence Platform
**Version:** 1.0
**Status:** Pre-Development

---

# 1. Purpose

The frontend provides the user-facing interface for:

* Business management
* Financial entry
* Valuation
* Risk
* Health
* Benchmarks
* Scenarios
* Recommendations
* Reports
* AI explanations

---

# 2. Architecture

Recommended structure:

```text
frontend/
│
├── app/
├── components/
├── features/
├── hooks/
├── services/
├── lib/
├── types/
├── utils/
└── styles/
```

---

# 3. Feature-Based Organization

```text
features/
├── auth/
├── businesses/
├── financials/
├── valuation/
├── risk/
├── health/
├── scenarios/
├── benchmarks/
├── reports/
└── ai/
```

---

# 4. State Management

Separate:

### Server state

* Businesses
* Financials
* Valuations
* Reports
* Recommendations

from:

### UI state

* Modal open
* Selected tab
* Form step
* Filters
* Temporary inputs

Avoid putting everything into one global store.

---

# 5. API Client

Create a centralized API client.

Responsibilities:

* Authentication headers
* Request handling
* Error normalization
* Retries where appropriate
* Response typing

---

# 6. Types

Financial objects should have explicit types.

Example conceptual model:

```text
FinancialStatement
Valuation
ValuationMethodResult
RiskAssessment
BusinessHealth
Recommendation
Scenario
```

Avoid excessive `any` usage.

---

# 7. Routing

Conceptual routes:

```text
/login
/register

/dashboard

/businesses
/businesses/:id
/businesses/:id/financials
/businesses/:id/valuation
/businesses/:id/risk
/businesses/:id/scenarios
/businesses/:id/recommendations
/businesses/:id/reports

/benchmarks
/settings
```

---

# 8. Form Architecture

Complex financial forms should use:

* Schema validation
* Field-level validation
* Section-level validation
* Save progress where appropriate
* Clear error messages

---

# 9. Financial Input UX

Never calculate authoritative financial outputs only in the browser.

Frontend may provide:

```text
Instant previews
Form feedback
Formatting
```

Backend remains authoritative.

---

# 10. Data Fetching

The frontend should:

* Show loading states
* Cache safe read operations
* Handle stale data
* Handle errors
* Avoid duplicate requests

---

# 11. Optimistic Updates

Use carefully.

Do not optimistically display:

* Final valuation
* Risk score
* Financial calculation results

unless the authoritative response has been received.

---

# 12. Chart Architecture

Charts should consume structured backend results.

Example:

```text
API
 ↓
Normalized chart data
 ↓
Chart component
```

Charts must not reconstruct financial formulas.

---

# 13. AI UI

AI responses should be separated from authoritative calculations.

Example:

```text
System Result
₦75M

AI Explanation
"The main factors affecting the result are..."
```

---

# 14. Error Boundaries

The application should prevent one failed component from crashing the entire dashboard.

Example:

```text
Benchmark failed
        ↓
Dashboard still works
```

---

# 15. Performance

Optimize:

* Bundle size
* Lazy-loaded reports
* Charts
* Large tables
* Images
* API requests

Avoid premature optimization.

---

# 16. Security

Frontend must never contain:

* API secrets
* Database credentials
* Private keys
* Administrative credentials

Client-side restrictions are not security boundaries.

---

# 17. Accessibility

Components must support:

* Keyboard navigation
* Screen readers
* Focus management
* Accessible labels
* Error messaging

---

# 18. Frontend Principle

> **The frontend presents intelligence; it does not become the source of financial truth.**