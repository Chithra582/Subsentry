---
name: subscription-detector
description: Parse read-only billing emails and invoices to identify recurring subscriptions and trials.
---

# Subscription Detector Skill

## Overview
Scans incoming billing notifications and electronic receipts to detect active subscriptions, free trials, and recurring payment frequencies.

## Operations
1. Filters messages for billing keywords (e.g., "receipt", "subscription", "recurring", "free trial", "invoice").
2. Extracts vendor identity, billed amount, currency code, and renewal intervals.
3. Discards non-billing email content and redacts confidential customer PII.
4. Formulates structured subscription records for dashboard ingestion.
