---
name: ralph-loop
description: Structured pattern for coordinating coding agents
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Ralph Loop - Coding Agent Orchestration Skill

**Skill Name**: ralph-loop
**Type**: Agent Orchestration
**For**: Orion AI CEO

## Purpose

Structured pattern for coordinating coding agents (Codex, Claude Code, Pi) with clear intent, action, and verification cycles. Named for the systematic observe→orient→decide→act loop adapted for agent management.

## The Ralph Loop Pattern

### Phase 1: RECON (Gather Context)

Before delegating to any coding agent:
1. Read relevant project context files
2. Check existing tests and specs
3. Identify dependencies and constraints
4. Clarify the success criteria

**Questions to answer**:
- What exactly needs to be built/fixed?
- What are the acceptance criteria?
- What existing code should this integrate with?
- What are the non-negotiables (security, performance, style)?

### Phase 2: INTENT (Define Task)

Craft a precise delegation prompt:
- **Role**: What type of agent (coder, debugger, etc.)
- **Task**: Specific, bounded description
- **Context**: All relevant background
- **Constraints**: Hard requirements, forbidden approaches
- **Output format**: Expected deliverable

**Template**:
```
## Task: [specific description]

## Context
- Project: [name]
- Location: [path]
- Related: [files, PRs, issues]

## Requirements
- [ ] Must have [requirement]
- [ ] Must NOT [forbidden thing]
- [ ] Follow [style guide, conventions]

## Success Criteria
- [ ] Test passes
- [ ] Lint passes
- [ ] TypeScript compiles

## Output
Produce [files changed] with [description]
```

### Phase 3: DELEGATE (Execute)

Use the delegate-agent tool with:
- `agentId`: Appropriate specialist (coder, debugger, security)
- `task`: The INTENT-formatted prompt
- `context`: Additional background if needed
- `timeout`: Appropriate duration (default 300s, complex = 600s)

**Timeout Guidelines**:
| Complexity | Timeout |
|------------|---------|
| Simple fix | 60s |
| Feature implementation | 300s |
| Multi-file refactor | 600s |
| New project scaffold | 900s |

### Phase 4: VERIFY (Validate Output)

Always verify before reporting success:

1. **Read outputs** — Check what was actually produced
2. **Run tests** — Execute test suite
3. **Lint/typecheck** — Verify code quality
4. **Review changes** — Ensure matches intent
5. **Check side effects** — No unintended breakage

**Verification Checklist**:
- [ ] Tests pass (`bun test` / `go test` / etc.)
- [ ] Lint passes
- [ ] Types compile
- [ ] Code matches requirements
- [ ] No security issues introduced
- [ ] Documentation updated if needed

### Phase 5: ITERATE (Fix if Needed)

If verification fails:
1. Analyze the failure
2. Formulate fix prompt
3. Re-delegate with specific correction
4. Re-verify

**Max iterations**: 3 per task. If still failing, escalate to human with full context.

## Ralph Loop in Practice

### Example: Adding a Feature

```
# Phase 1: RECON
- Read project SPEC.md
- Check existing similar features in codebase
- Identify database schema, API endpoints

# Phase 2: INTENT
- Write detailed task for coder agent
- Include code examples, style requirements

# Phase 3: DELEGATE
- Spawn coder agent with timeout=300s

# Phase 4: VERIFY
- Run tests, check lint
- Verify implementation matches spec

# Phase 5: ITERATE
- If tests fail, fix and re-test (max 3x)
```

### Example: Bug Fix

```
# Phase 1: RECON
- Read error trace, identify affected code
- Check git history for recent changes

# Phase 2: INTENT
- Task: Fix [specific bug]
- Context: [error message, reproduction steps]

# Phase 3: DELEGATE  
- Spawn debugger agent

# Phase 4: VERIFY
- Run failing test (should pass now)
- Run full test suite
```

## Anti-Patterns

- ❌ Delegating without sufficient context
- ❌ Skipping verification — assuming agent got it right
- ❌ Giving vague instructions like "fix this"
- ❌ Not checking test results
- ❌ Iterating endlessly without progress
- ❌ Accepting broken builds

## Best Practices

- ✅ Always write INTENT before delegating
- ✅ Verify every output before moving on
- ✅ Set realistic timeouts
- ✅ Provide code examples in prompts when possible
- ✅ Use `security` agent for auth/encryption changes
- ✅ Update memory after each major task
- ✅ Keep iterations under 3 — escalate if blocked

## Integration

- Uses `delegate-agent` tool
- Relies on memory-system for context
- Coordinates with heartbeat for monitoring agent runs
- Logs to daily timeline for audit trail
