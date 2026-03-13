---
name: sentry-autofix
description: Autonomous error monitoring and self-healing for deployed applications
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["sentry-cli"], "env": ["SENTRY_ORG", "SENTRY_AUTH_TOKEN"] },
      "primaryEnv": "SENTRY_AUTH_TOKEN"
    }
  }
---

# Sentry Auto-Fix Skill

Autonomous error monitoring and self-healing for deployed applications. Monitors Sentry for new errors, analyzes stack traces, attempts fixes, and escalates when needed.

## Setup

```bash
# Install Sentry CLI
brew install getsentry/tap/sentry-cli

# Configure environment
export SENTRY_ORG=your-org
export SENTRY_AUTH_TOKEN=your-token
```

## Error Monitoring Workflow

### 1. Check for New Errors

Run periodically via heartbeat or cron:
```bash
sentry-cli issues list --status=unresolved --limit=20 --project=your-project
```

### 2. Analyze Errors

For each new error:
- Read stack trace
- Check if it's a new error type or recurring
- Determine if it's actionable (code fix) or external (API outage)

### 3. Attempt Self-Fix

For actionable errors:
- Identify the failing code path
- Write fix based on error pattern
- Create branch and PR
- Link fix to Sentry issue

### 4. Escalate if Needed

For complex errors:
- Document in memory with error details
- Flag for human review
- Create tracking issue

## Anti-Patterns

- Don't ignore recurring errors
- Don't declare failure without examining git history
- Don't auto-retry failed deployments without understanding root cause

## Integration with Ralph Loop

Use within Ralph Loop's Verify phase:
- After deployment, check Sentry for new errors
- If errors found, iterate on fix before declaring success
