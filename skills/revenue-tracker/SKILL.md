---
name: revenue-tracker
description: Revenue and metrics tracking with daily reporting across Stripe accounts
metadata:
  {
    "openclaw": {
      "requires": { "bins": [], "env": ["STRIPE_SECRET_KEY"] },
      "primaryEnv": "STRIPE_SECRET_KEY"
    }
  }
---

# Revenue Tracker Skill

Track revenue and metrics across Stripe accounts with daily reporting and trend analysis.

## Setup

```bash
# Install Stripe CLI for local development
brew install stripe/stripe-cli/stripe

# Configure environment
export STRIPE_SECRET_KEY=sk_live_xxx
```

## Features

### 1. Daily Revenue Check

Run each morning via cron:
- Query Stripe for yesterday's revenue
- Compare to 7-day average
- Flag significant deviations (>20%)

### 2. Metrics Tracking

Track:
- Daily/monthly revenue
- New customers vs churn
- Average order value
- Subscription MRR
- Failed payments

### 3. Multi-Account Support

Support multiple Stripe accounts:
- Store API keys per account
- Query each account separately
- Aggregate in dashboard

### 4. Daily Report Generation

Generate morning report:
```
# Revenue Report: 2026-03-12

## Yesterday
- Revenue: $X,XXX
- New Customers: X
- Churned: X

## Trends
- 7-day avg: $X,XXX
- MoM change: +X%

## Alerts
- [!] Revenue down 25% from average
- [~] 3 failed payments (review)
```

## Cron Schedule

```bash
# Daily at 8am
0 8 * * * openclaw cron add --name "Revenue check" --cron "0 8 * * *" --message "Run revenue check and report"
```

## Memory Integration

Log daily metrics to:
- `memory/YYYY-MM-DD.md` - daily log
- `MEMORY.md` - curated long-term metrics

## Anti-Patterns

- Don't alert on every small fluctuation
- Don't ignore consecutive down days
- Don't rely on single data point for trends
