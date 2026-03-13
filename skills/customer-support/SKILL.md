---
name: customer-support
description: Customer support with 3-tier escalation ladder for autonomous handling
metadata:
  {
    "openclaw": {
      "requires": { "bins": [], "env": [] },
      "always": true
    }
  }
---

# Customer Support Skill

Autonomous customer support with 3-tier escalation ladder. Handle routine support without waiting for human intervention.

## 3-Tier Escalation System

### Tier 1: AI Self-Service (Handle Immediately)

Issues AI can resolve autonomously:
- Password reset instructions
- How-to questions from documentation
- Account status inquiries
- Billing questions with Stripe data
- Feature requests (log and acknowledge)

**Response Time**: Immediate

### Tier 2: AI with Tools (Resolve or Prepare)

Issues requiring investigation:
- Bug reports (reproduce if possible)
- Refund requests (review Stripe data first)
- Account-specific issues (check logs)
- Technical issues (gather diagnostics)

**Process**:
1. Acknowledge receipt
2. Investigate using available tools
3. Resolve if possible
4. Escalate to Tier 3 if blocked

**Response Time**: Within 1 hour

### Tier 3: Human Escalation (Flag for Review)

Issues requiring human intervention:
- Legal/compliance issues
- Complex technical problems
- Customer requests human
- Repeated unresolved issues
- Security incidents

**Process**:
1. Summarize issue and history
2. Document all attempted resolutions
3. Flag in memory with priority
4. Notify human via configured channel

## Support Workflow

### 1. Classify Incoming

Determine tier:
- Is it a routine question? → Tier 1
- Does it need investigation? → Tier 2
- Is it complex/blocked? → Tier 3

### 2. Respond Appropriately

- Tier 1: Answer immediately, close ticket
- Tier 2: Acknowledge, investigate, resolve or escalate
- Tier 3: Summarize, flag, notify human

### 3. Document Everything

Log to `memory/YYYY-MM-DD.md`:
```
## Support
- [email]: Customer X issue - Tier 1 - Resolved
- [email]: Customer Y bug report - Tier 2 - Investigating
- [email]: Customer Z refund request - Tier 3 - Escalated to human
```

### 4. Learn and Improve

- Track common issues in MEMORY.md
- Identify patterns requiring documentation
- Create FAQ responses for recurring Tier 1 issues

## Anti-Patterns

- Don't share sensitive user data in logs
- Don't promise specific resolution times
- Don't ignore customer frustration cues
- Don't escalate without attempting resolution
