# ⚠️ FOR EXPERIENCED OPENCLAW USERS ONLY

> **This persona is designed for experienced OpenClaw users.** It integrates multiple advanced systems (memory, cron, heartbeat, external APIs) that may require specific configuration. This may not work 100% out of the box and could require troubleshooting depending on your OpenClaw setup and environment.

---

# Orion AI CEO Persona for OpenClaw

An autonomous AI CEO persona running inside OpenClaw. Orion embodies ownership mentality, autonomous operations, and decisive execution.

## Quick Start

1. **Copy to your OpenClaw workspace**:
   ```bash
   cp -r ~/ai-ceo-persona ~/.openclaw/workspace/orion
   ```

2. **Set as active persona**:
   - Configure OpenClaw to use `orion/SOUL.md` as the identity
   - Or reference individual skills as needed

3. **Install dependencies**:
   ```bash
   # 1Password CLI (for email-manager, secure credentials)
   brew install 1password-cli

   # X/Twitter CLI (for twitter-cli skill)
   # See xurl skill or install twurl/t

   # aerc or mutt (for email-manager)
   brew install aerc
   ```

## Directory Structure

```
ai-ceo-persona/
├── SOUL.md                    # Core identity & philosophy
├── README.md                  # This file
├── skills/
│   ├── memory-system/         # Three-tier memory (PARA + timeline + tacit)
│   ├── ralph-loop/            # Coding agent orchestration
│   ├── email-manager/         # Secure email via CLI
│   ├── twitter-cli/           # X/Twitter posting via CLI
│   ├── heartbeat/             # Process monitoring
│   ├── cron-manager/          # Scheduled task management
│   ├── sentry-autofix/       # Error monitoring and self-healing
│   ├── revenue-tracker/       # Revenue and metrics tracking
│   ├── customer-support/      # 3-tier support escalation
│   ├── google-sheets-docs/    # Google Sheets/Docs API
│   └── tmux-stable-socket/     # Stable tmux sessions
├── scripts/                   # Utility scripts
│   ├── heartbeat-check.sh
│   ├── orion-recover.sh
│   └── health-check.sh
└── memory/                    # Default memory directory
    ├── projects/
    ├── archives/
    ├── resources/
    └── areas/
```

## Skills Overview

| Skill | Purpose |
|-------|---------|
| **memory-system** | Three-tier memory with decay for persistent context |
| **ralph-loop** | Structured coding agent orchestration (Recon → Intent → Delegate → Verify → Iterate) |
| **email-manager** | Secure CLI email with 1Password integration |
| **twitter-cli** | X/Twitter posting and engagement via CLI |
| **heartbeat** | Background process and service monitoring |
| **cron-manager** | Centralized scheduled task management |
| **sentry-autofix** | Autonomous error monitoring and self-healing |
| **revenue-tracker** | Revenue and metrics tracking with daily reporting |
| **customer-support** | 3-tier escalation ladder for support |
| **google-sheets-docs** | Google Sheets/Docs API via service account |
| **tmux-stable-socket** | Stable tmux sessions that survive restarts |

## Core Philosophy

Orion operates with:

- **Ownership**: Owns outcomes, no excuses
- **Autonomy**: Self-starts, minimal hand-raising  
- **Velocity**: Fast decisions, fast execution
- **Security**: Locked-down by default
- **Systems**: Builds processes that scale

See `SOUL.md` for full philosophy.

## Anti-Patterns (Don't Do)

- Waiting for human approval on routine decisions
- Forgetting context between sessions
- Letting tasks fall through cracks
- Over-aggregating without context
- Public failure without recovery plan
- Letting knowledge decay

## Best Practices

- Read memory on every startup
- Use Ralph Loop for all agent delegations
- Run heartbeat checks every minute
- Log everything to memory
- Review and archive regularly
- Test recovery procedures

## Requirements

- OpenClaw installed and running
- 1Password CLI (for credential management)
- Terminal email client (aerc recommended)
- X/Twitter API keys (for posting)
- Standard Unix tools (cron, bash, etc.)

## Customization

Edit `SOUL.md` to adjust:
- Name and title
- Communication style
- Goals and priorities
- Anti-patterns to avoid

Edit individual skill SKILL.md files to:
- Adjust memory structure
- Modify Ralph Loop phases
- Change monitoring targets
- Customize cron schedules

## License

MIT License

Copyright (c) 2026 Chris Bunting

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## Support

If you find these useful, consider [sponsoring](https://github.com/sponsors/cbuntingde) the project.

---

Copyright © 2026 Created by Chris Bunting
