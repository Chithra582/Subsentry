# Explainability & Transparency Report: SubSentry Agent

> **Specification:** OpenGAP v0.1.0  
> **Domain:** Finance / Subscription Tracking & Recurring Expense Auditing  
> **Target System:** SubSentry (Subscription Management Dashboard)  
> **Audit Status:** Qualified for HiDevs GitAgent Passport  

---

## 1. Overview & Operational Purpose

**SubSentry Agent** is an autonomous subscription management, spend auditing, and renewal alert agent designed for **SubSentry**. The agent tackles subscription creep—the gradual accumulation of recurring charges and forgotten free trials that erode personal budgets.

Core capabilities:
1. **Billing Signal Ingestion**: Extracts recurring charge metadata from Gmail receipts using read-only OAuth scopes.
2. **Spend Normalization & Forecasting**: Aggregates disparate weekly, monthly, and yearly cycles into unified burn rate metrics.
3. **Renewal Sentinel & Trial Cliff Alerts**: Dispatches preemptive warnings ahead of renewal deadlines and free trial conversions.
4. **Cancellation Pathfinder**: Delivers targeted, direct unsubscription guides to bypass dark-pattern retention traps.

---

## 2. How the Agent Decides (Decision-Making Logic)

```
Incoming Signal (Gmail Ingestion / Manual Subscription Entry / Sync Trigger)
  │
  ├── 1. Privacy Filter & PII Sanitization
  │      ├── Filter emails against subscription/billing keywords
  │      ├── Discard unrelated correspondence immediately
  │      └── Redact credit card numbers, passwords, and personal identifiers
  │
  ├── 2. Subscription Metadata Extraction
  │      ├── Extract Vendor Name (e.g. Netflix, GitHub, Spotify)
  │      ├── Extract Amount & Currency (e.g. $14.99 USD)
  │      ├── Identify Billing Frequency (Weekly, Monthly, Annual)
  │      └── Determine Next Renewal Date & Trial Status (Active Trial vs. Paid)
  │
  ├── 3. Financial Aggregation & Burn Calculation
  │      ├── Normalize monthly equivalent: Monthly_Spend = sum(cost * freq_multiplier)
  │      ├── Project annual commitment: Annual_Spend = Monthly_Spend * 12
  │      └── Assign spend category (Entertainment, Software, Utilities, Fitness)
  │
  ├── 4. Renewal & Trial Monitoring Engine
  │      ├── Calculate days until next renewal: ΔT = Renewal_Date - Current_Date
  │      ├── If ΔT <= 3 days (or trial expiration approaching):
  │      │   └── Trigger Priority Renewal Warning Notification
  │      └── If user marks "Want to Cancel":
  │          └── Route to Cancellation Pathfinder for direct unsubscription steps
  │
  └── 5. Audit Trail & Human Oversight
         ├── Store sanitized subscription record in user database
         └── Await user confirmation for manual edits or unlinking
```

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
|---|---|---|---|
| **Billing Receipts & Invoices** | Gmail Read-Only API (`gmail.readonly`) | Detecting recurring subscriptions and renewal notices | Scanned ephemerally; message bodies discarded; only billing metadata retained |
| **Manual Subscriptions** | User Dashboard Inputs | Adding offline or non-email recurring services | Encrypted at rest in MongoDB; editable and deletable by user |
| **Renewal Timestamps** | Invoiced receipts & user calendars | Triggering 72h / 24h countdown alerts | Stored as ISO timestamps; logged for audit scheduling |
| **Vendor Metadata** | Open-source subscription directory | Categorizing services and generating cancellation links | Public reference database; contains zero user telemetry |

---

## 4. Known Limitations & Failure Modes

### 1. Ambiguous Billing Receipt Formats
*Limitation:* Aggregator receipts (such as Apple App Store or PayPal monthly digests) bundle multiple services together without itemized billing frequency details.  
*Mitigation:* The agent flags multi-item aggregator invoices as "Candidate Subscriptions" and prompts the user for one-click verification and itemization before inclusion in spend calculations.

### 2. Silent Price Hikes by Vendors
*Limitation:* Vendors frequently update subscription prices without changing the email subject line, making simple keyword taggers blind to cost increases.  
*Mitigation:* The agent tracks historical invoice amounts per vendor; any delta exceeding 5% triggers a prominent "Price Hike Detected" notification banner.

### 3. Dynamic Dark Patterns in Cancellation Flows
*Limitation:* Subscription providers continually modify cancellation URLs and user flows to deter unsubscription.  
*Mitigation:* The agent maintains crowdsourced community-validated unsubscription pathways and provides fallback direct account settings links and customer support templates.

### 4. Multi-Currency Fluctuations
*Limitation:* Subscriptions billed in foreign currencies can introduce variance in local monthly burn rate projections.  
*Mitigation:* The agent records native billing currencies and utilizes daily cached exchange rates, explicitly displaying the currency conversion basis to the user.

---

## 5. Verification, Safety & Human Oversight

1. **Strict Read-Only Guarantee**: The agent is architecturally blocked from modifying, deleting, or sending emails. OAuth scopes are rigidly audited.
2. **Human-in-the-Loop Cancellation**: The agent never terminates a subscription autonomously. It furnishes the user with instructions, links, and renewal dates, leaving execution in human hands.
3. **Instant Account Unlinking**: Users can revoke Gmail access and delete all stored subscription records with a single click at any time.
4. **Zero Financial Telemetry**: Subscription data is strictly used for personal dashboard calculations and is never monetized, traded, or shared with third-party advertisers.
