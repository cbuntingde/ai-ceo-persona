---
name: memory-system
description: Three-tier memory system for context across sessions
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Memory System Skill

**Skill Name**: memory-system
**Type**: Core Capability
**For**: Orion AI CEO

## Purpose

Three-tier memory system enabling Orion to maintain context across sessions, track operational history, and manage knowledge with freshness decay.

## Architecture

### Tier 1: PARA (Projects, Archives, Resources, Areas)

Long-term memory using the PARA method. Structured directories for persistent knowledge.

**Directory Structure**:
```
memory/
├── projects/      # Active projects with goals, specs, progress
├── archives/      # Completed projects, reference material
├── resources/    # Reusable templates, snippets, docs
└── areas/        # Ongoing responsibilities, standards
```

**Usage**:
- Write to `projects/` for active work
- Archive completed work to `archives/`
- Store reusable assets in `resources/`
- Maintain standards in `areas/`

### Tier 2: Daily Timeline

Session-by-session operational log. One file per day.

**Format**: `memory/YYYY-MM-DD.md`

**Contents**:
```
# 2026-03-11

## Morning Session
- Delegated task X to coder agent
- Outcome: completed
- Key decision: chose approach Y over Z

## Afternoon Session
- Reviewed PR feedback
- Action: addressed comments, re-queued
```

**Protocol**:
- Write session summaries immediately after each work block
- Log key decisions, outcomes, and next steps
- Reference previous days for continuity

### Tier 3: Tacit Knowledge (Active Context)

Working memory for current session. Includes:
- Current task context
- Recent agent outputs
- Pending actions
- Active project state

**Decay Mechanism**:
- Mark context entries with timestamps
- After 24 hours, demote to "stale" (move to PARA)
- After 7 days, archive or delete stale entries
- Use `memory/decay-check.sh` script weekly

## Implementation

### Writing Memory

```bash
# Daily log
echo "## $(date +%H:%M) - Task description" >> memory/$(date +%Y-%m-%d).md

# PARA entry
cat > memory/projects/project-name.md << EOF
# Project Name

## Goal
...

## Status
- [ ] Task 1
- [x] Task 2

## Notes
...
EOF
```

### Reading Memory (Startup)

On each session start, read in order:
1. `SOUL.md` — Identity check
2. `USER.md` — Context check  
3. `memory/YYYY-MM-DD.md` — Today's timeline
4. `memory/YYYY-MM-DD.md` (yesterday) — Continuity
5. `MEMORY.md` — Cross-session persistent notes
6. PARA relevant sections — Project context

### Decay Script

```bash
#!/bin/bash
# memory/decay-check.sh
TODAY=$(date +%s)
WEEK_AGO=$((TODAY - 7*86400))

find memory -name "*.md" -mtime +7 | while read f; do
    # Move old daily logs to archives
    if [[ $(basename $f) =~ ^[0-9]{4}-[0-9]{2}-[0-9]{2}\.md$ ]]; then
        mv "$f" "memory/archives/daily/$(basename $f)"
    fi
done
```

## Anti-Patterns

- ❌ Writing to memory only when forced —养成习惯
- ❌ Forgetting to read memory on startup
- ❌ Storing sensitive data in plain text (use 1Password for secrets)
- ❌ Letting tacit knowledge grow unbounded
- ❌ Not archiving completed projects

## Best Practices

- ✅ Prefix filenames with dates for daily logs
- ✅ Use checked boxes `- [x]` / `- [ ]` for task tracking
- ✅ Link between memory files using relative paths
- ✅ Tag entries with `## @tag` for filtering
- ✅ Review weekly and clean stale entries
- ✅ Keep PARA structure lean — archive aggressively

## Files

- `memory/` — Root memory directory
- `memory/YYYY-MM-DD.md` — Daily timeline
- `memory/MEMORY.md` — Persistent cross-session notes
- `memory/decay-check.sh` — Weekly cleanup script
- `memory/import.sh` — Bulk import from external sources
