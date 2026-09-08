# Backend Architecture

**Product:** SME Business Valuation & Intelligence Platform
**Version:** 1.0
**Status:** Pre-Development

---

# 1. Architecture Style

The MVP should use a:

> **Modular Monolith**

rather than immediately adopting microservices.

---

# 2. Backend Structure

```text
backend/
│
├── src/
│   ├── auth/
│   ├── users/
│   ├── businesses/
│   ├── financials/
│   ├── metrics/
│   ├── risk/
│   ├── valuation/
│   ├── benchmarks/
│   ├── scenarios/
│   ├── recommendations/
│   ├── reports/
│   ├── ai/
│   ├── audit/
│   └── shared/
│
├── tests/
├── migrations/
├── config/
└── scripts/
```

---

# 3. Module Structure

Each module should follow:

```text
module/
├── controller
├── service
├── repository
├── domain
├── validation
└── tests
```

The exact framework implementation may differ.

---

# 4. Controller Layer

Responsibilities:

* Receive HTTP request
* Authenticate
* Validate request
* Call service
* Format response

Controllers must not contain complex financial formulas.

---

# 5. Service Layer

Responsibilities:

* Business logic
* Workflow orchestration
* Domain operations

Example:

```text
ValuationService
    ↓
FinancialService
    ↓
BenchmarkService
    ↓
ValuationEngine
```

---

# 6. Domain Layer

Contains:

* Financial entities
* Valuation entities
* Risk entities
* Domain rules
* Calculation abstractions

This layer should remain independent from HTTP.

---

# 7. Repository Layer

Responsible for database interaction.

Examples:

```text
BusinessRepository
FinancialRepository
ValuationRepository
BenchmarkRepository
AuditRepository
```

---

# 8. Valuation Service

Conceptual:

```text
createValuation(businessId)
        ↓
loadInputs()
        ↓
validateInputs()
        ↓
calculateMetrics()
        ↓
loadBenchmarks()
        ↓
runMethods()
        ↓
applyAdjustments()
        ↓
calculateFinalValue()
        ↓
calculateConfidence()
        ↓
persistSnapshot()
```

---

# 9. AI Service

The AI service should accept structured context.

Example:

```json
{
  "valuation": {
    "central": 75000000,
    "low": 60000000,
    "high": 85000000
  },
  "riskScore": 31,
  "confidence": 78,
  "drivers": []
}
```

It should not have unrestricted authority over the database.

---

# 10. Job System

Future asynchronous jobs:

```text
ReportGenerationJob
ScenarioAnalysisJob
BenchmarkRefreshJob
AIAnalysisJob
BatchValuationJob
```

---

# 11. Caching

Cache candidates:

* Industry benchmarks
* Static metadata
* Frequently requested dashboard metrics

Do not cache mutable financial calculations without clear invalidation rules.

---

# 12. Database Access

Use parameterized queries or a safe ORM/query builder.

Never construct SQL from untrusted user input.

---

# 13. Transactions

Use transactions for:

* Valuation creation
* Financial record updates
* Major business deletion
* Report generation state changes

---

# 14. API Security

Require:

* Authentication
* Authorization
* Input validation
* Rate limiting
* HTTPS
* Secure headers
* Request size limits

---

# 15. Role-Based Access

Example:

```text
OWNER
ADMIN
ANALYST
ADVISOR
INVESTOR
SUPER_ADMIN
```

Authorization must be checked server-side.

---

# 16. Multi-Tenancy

Businesses should be isolated logically.

Every request involving a business must verify that the authenticated user has access.

Never trust a `businessId` supplied by the client without authorization checking.

---

# 17. API Error Contract

All errors should follow the API specification.

Example:

```json
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_DATA",
    "message": "Additional financial information is required."
  }
}
```

---

# 18. Health Endpoint

Production should expose a basic health mechanism.

Example:

```text
GET /health
```

Potential checks:

* Application
* Database
* Queue
* Critical dependencies

Do not expose sensitive infrastructure details.

---

# 19. Configuration

Use environment variables/secrets management.

Examples:

```text
DATABASE_URL
AUTH_SECRET
AI_API_KEY
STORAGE_BUCKET
```

Never commit secrets.

---

# 20. Testing

Backend tests:

```text
Unit
Integration
API
Authorization
Database
Financial
End-to-End
```

Financial engine tests have highest correctness priority.

---

# 21. Deployment

Recommended environments:

```text
Development
    ↓
Staging
    ↓
Production
```

Production should never be the first place a database migration is tested.

---

# 22. Backend Principle

> **Business logic should live in the backend/domain layer, not inside controllers or frontend components.**