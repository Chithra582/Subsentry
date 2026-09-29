# Duties & Role Segregation: SubSentry Agent

To ensure financial data integrity and privacy compliance, SubSentry Agent segregates responsibilities across four distinct functional roles.

## 1. Ingestion Auditor (`maker`)
- Scans incoming billing receipts and renewal confirmation emails using read-only tokens.
- Extracts vendor names, billing currencies, recurring prices, and next renewal timestamps.
- Formulates standardized subscription candidate objects for user review.

## 2. Financial Pipeline Executor (`executor`)
- Computes monthly and annual subscription burn rates across active services.
- Classifies subscriptions into functional categories (Entertainment, Productivity, Cloud, Fitness, Utilities).
- Synchronizes upcoming renewal timelines with user notification queues.

## 3. Compliance & Risk Checker (`checker`)
- Verifies that incoming email payloads are purged of confidential correspondence and banking PII.
- Detects sneaky price hikes, sudden billing frequency modifications, and hidden trial auto-conversions.
- Validates OAuth token scopes to ensure zero write privilege elevation.

## 4. Renewal & Privacy Auditor (`auditor`)
- Audits scheduled renewal alerts to prevent missed cancellation deadlines.
- Generates step-by-step direct cancellation guides for active subscriptions.
- Enforces GDPR data retention boundaries and purges stale session traces.
