# Persona Template

Quick-copy templates for building Orion personas.

## SOUL.md Template

```markdown
# SOUL.md - [Persona Name]

## Identity

**Name**: [Name]
**Title**: [Job Title]
**Emoji**: [Emoji]
**Vibe**: [3-4 word description]

## Core Definition

[Brief paragraph describing what this persona is and does]

## Operating Philosophy

### [Principle 1]
[Description]

### [Principle 2]
[Description]

## Goals

1. [Goal 1]
2. [Goal 2]
3. [Goal 3]

## Communication Style

- **Tone**: [ adjectives ]
- **Format**: [ bullet points, prose, etc. ]
- **Avoid**: [ things not to do ]

## Anti-Patterns

- ❌ [Anti-pattern 1]
- ❌ [Anti-pattern 2]

## What I Value

| Value | Meaning |
|-------|---------|
| [Value 1] | [Description] |
| [Value 2] | [Description] |

---

## Memory Architecture

See MEMORY.md for details.
```

## MEMORY.md Template

```markdown
# MEMORY.md - [Persona] Memory System

## Memory Structure

- **Type**: [ PARA / Graph / Hybrid ]
- **Location**: `~/.orion/[persona]/memory/`

### Directory Structure

```
memory/
├── projects/      # Active work
├── archives/      # Completed
├── resources/     # Templates, docs
└── areas/        # Ongoing responsibilities
```

## Knowledge Base

### Expertise Areas
- [Area 1]
- [Area 2]

### Data Sources
- [Source 1]
- [Source 2]

### Retention
- Active: [time]
- Archive: [time]
- Delete: [time]
```

## HEARTBEAT.md Template

```markdown
# HEARTBEAT.md - [Persona] Daily Operations

## Active Hours

- **Timezone**: [UTC/Local]
- **Active**: [hours]

## Daily Tasks

| Time | Task | Priority |
|------|------|----------|
| [time] | [task] | [high/medium/low] |
| [time] | [task] | [high/medium/low] |

## Health Checks

- [ ] [Check 1]
- [ ] [Check 2]

## Alert Conditions

- [Condition 1] → [Action]
- [Condition 2] → [Action]
```

## Skills/SKILL.md Template

```markdown
# [Skill Name] Skill

**Skill Name**: [name]
**Type**: [category]
**For**: [Persona]

## Purpose

[Brief description of what this skill does]

## Prerequisites

- [Requirement 1]
- [Requirement 2]

## Usage

```bash
[example command]
```

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| [option] | [type] | [default] | [description] |

## Anti-Patterns

- ❌ [Anti-pattern 1]
- ❌ [Anti-pattern 2]

## Best Practices

- ✅ [Practice 1]
- ✅ [Practice 2]
```

## README.md Template

```markdown
# [Persona Name]

[One-line description]

## Features

- ✅ [Feature 1]
- ✅ [Feature 2]
- ✅ [Feature 3]

## Quick Start

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Requirements

- [Requirement 1]
- [Requirement 2]

## Documentation

See [SETUP.md](SETUP.md) for detailed installation.

## Support

[How to get help]

## License

[License name] - see LICENSE file
```

## SETUP.md Template

```markdown
# Setup Guide - [Persona Name]

## Prerequisites

| Software | Version | Notes |
|----------|---------|-------|
| [software] | [version] | [notes] |

## Installation

### Step 1: [Title]

[Instructions]

### Step 2: [Title]

[Instructions]

### Step 3: [Title]

[Instructions]

## Configuration

```json
{
  "key": "value"
}
```

## Verification

```bash
[test command]
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| [problem] | [solution] |

## Uninstall

[Instructions]
```

---

For full standards, see [PERSONA-STANDARD.md](../PERSONA-STANDARD.md)
