# EXPLAINABILITY — SubSentry Agent

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* SubSentry Agent (`subsentry-agent`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Finance / Subscription Tracking & Recurring Expense Auditing  

---

## 1. Overview & Financial Purpose

SubSentry Agent is an autonomous subscription management, recurring payment auditing, and renewal sentinel intelligence built for consumers and small teams. The underlying system operates across user-authorized billing signals, electronic payment receipts, and manual subscription inventories—tracking vendor names, billing frequencies (monthly, quarterly, annual), recurring amounts, currencies, and upcoming renewal deadlines.

The agent's primary financial purpose is to eliminate "subscription creep" and silent money leaks. By converting opaque bank deductions and buried free-trial conversions into normalized monthly/yearly burn projections and proactive cancellation alerts, the agent restores financial sovereignty and shields users from predatory dark-pattern auto-renewals.

---

## 2. How the Agent Decides (Decision-Making Logic)

SubSentry Agent operates across a deterministic, multi-stage decision pipeline that grounds every calculation in verified billing records:

```
[Billing Receipts & Invoices] ──> [Data Hygiene & PII Sanitization] ──> [Burn Rate & Recurrence Analysis]
                                                                                     │
                                                                                     ▼
[Actionable Advice & Renewal Alerts] <── [Consumer Advocacy & Safety Gate] <── [Renewal & Dark-Pattern Detection]
```

### 2.1 Ingestion & PII Sanitization
- **Decision:** Determines whether incoming email receipts or manual entries are valid billing records and purges non-billing data.
- **Rules:**
  - Filters email metadata using strict billing keywords (`invoice`, `receipt`, `subscription`, `recurring`, `free trial`, `membership`).
  - Immediately discards email body text and non-billing correspondence; extracts only vendor, amount, currency, and renewal date.
  - Strips credit card numbers, bank account details, physical addresses, and confidential communication strings before processing.

### 2.2 Recurring Spend Normalization & Burn Analysis
- **Decision:** Converts disparate billing frequencies into standardized cash-flow run rates.
- **Rules:**
  - Normalizes frequencies: Weekly charges are multiplied by 4.33, quarterly charges divided by 3, and annual charges divided by 12 to yield exact Monthly Burn.
  - Projects Annual Commitment: `Annual_Spend = Monthly_Spend * 12`.
  - Automatically tags services into functional categories (Entertainment, Productivity, Cloud Infrastructure, Fitness, Utilities).

### 2.3 Renewal Timelines & Dark-Pattern Detection
- **Decision:** Identifies upcoming billing cliffs, price creeps, and predatory cancellation funnels.
- **Rules:**
  - Computes days remaining: `ΔT = Renewal_Date - Current_Date`. If `ΔT <= 3 days` (or trial conversion is impending), flags as High Priority Warning.
  - Compares new invoice values against historical vendor baselines; triggers a "Price Hike Detected" flag if cost increases by > 5%.
  - Detects complicated retention loops and retrieves direct unsubscription portal URLs.

### 2.4 Consumer Advocacy & Safety Gate
- **Decision:** Enforces strict consumer advocacy and non-autonomous financial boundaries.
- **Rules:**
  - **No Autonomous Cancellation**: The agent never executes a cancellation or modifies an account autonomously; it provides direct links, verified cancellation steps, and email drafts for the user to execute.
  - **Zero Telemetry**: User spending figures and merchant names are never monetized, transmitted to third-party ad networks, or used for model training.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
|---|---|---|---|
| **Billing Receipts & Invoices** | Gmail Read-Only API (`gmail.readonly`) | Detecting recurring subscriptions, trial conversions, and renewal receipts | Processed ephemerally in active memory; message bodies discarded; only billing metadata retained |
| **Manual Subscription Entries** | User Dashboard Input | Adding offline, cash, or alternative recurring payments | Encrypted at rest in user database; editable and deletable on demand |
| **Renewal Timestamps** | Invoiced receipts & system clock | Scheduling 72-hour and 24-hour advance renewal warnings | Stored as ISO 8601 timestamps; used exclusively for countdown notifications |
| **Vendor Cancellation Directory** | Public unsubscription knowledge base | Providing direct unsubscription links and step-by-step cancellation instructions | Public static reference; contains zero user telemetry or tracking tokens |

SubSentry Agent complies with privacy-by-design standards:
- **Strict Read-Only Scope:** Operates exclusively under `gmail.readonly`; cannot send, modify, or delete user emails.
- **No PII collection:** Full card numbers, CVVs, bank credentials, and unrelated private messages are never collected or stored.
- **Zero commercial data mining:** User financial profiles, spending habits, and vendor subscriptions are never monetized or shared with third parties.
- **Right to Erasure:** Users can revoke OAuth tokens and permanently purge all synced subscription history with a single click.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

1. **Ambiguous Billing Receipt Formats:**
   - *Limitation:* Bundled aggregator invoices (such as Apple App Store, Google Play, or PayPal monthly receipts) combine multiple disparate items without granular subscription frequencies.
   - *Mitigation:* The agent flags bundled receipts as "Candidate Subscriptions" and prompts the user for one-click verification and line-item breakdown before finalizing spend totals.

2. **Silent Price Hikes by Vendors:**
   - *Limitation:* Providers frequently increase subscription rates without modifying the invoice subject line, making superficial keyword taggers blind to cost inflation.
   - *Mitigation:* The agent continuously tracks historical invoice amounts per vendor; any delta exceeding 5% triggers a prominent "Price Hike Detected" notification banner.

3. **Dynamic Dark Patterns in Cancellation Flows:**
   - *Limitation:* Subscription providers continually alter cancellation URLs, conceal account management buttons, and implement forced phone-call retention policies.
   - *Mitigation:* The agent maintains community-verified unsubscription guides, direct account settings deep-links, and pre-formatted cancellation email drafts for manual service escalation.

4. **Multi-Currency Fluctuations & Foreign Fees:**
   - *Limitation:* Subscriptions billed in foreign currencies fluctuate against the user's primary currency, creating minor variances in projected annual burn rates.
   - *Mitigation:* The agent explicitly displays the native billing currency alongside normalized conversions using daily cached exchange rate baselines.

---

## 5. Verification, Safety & Human Oversight

- **Strict Read-Only Least Privilege:** Architectural constraints prevent any email write or send operations; OAuth tokens are scoped minimally to read-only metadata extraction.
- **Human-in-the-Loop Governance:** The agent acts strictly as an advisory copilot; final contract cancellations, renewals, and financial choices remain solely under human control.
- **Deterministic Quality Gates:** Financial figures are calculated through verifiable arithmetic routines rather than probabilistic LLM estimations.
- **Kill Switch & Immutable Audit Logging:** Users can terminate data sync instantly from the dashboard; all automated detection decisions and notification dispatches are recorded in structured audit logs.
