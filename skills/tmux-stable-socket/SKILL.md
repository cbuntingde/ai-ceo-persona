---
name: tmux-stable-socket
description: Stable tmux socket that survives macOS restarts (macOS reaps /tmp)
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["tmux"], "env": [] },
      "always": true
    }
  }
---

# Tmux Stable Socket Skill

Configure tmux with a stable socket that survives macOS restarts. macOS reaps `/tmp`, so use `~/Library/Application Support/tmux/` instead.

## Problem

macOS periodically cleans `/tmp`, which is where tmux sockets live by default. This kills long-running tmux sessions.

## Solution

Use a socket in a persistent directory:

```bash
# Create persistent socket directory
mkdir -p ~/Library/Application\ Support/tmux

# Use custom socket
tmux -L orion -S ~/Library/Application\ Support/tmux/orion
```

## Configuration

### 1. Set TMUX Variable

Add to your shell profile:
```bash
export TMUX_SOCKET=~/Library/Application\ Support/tmux/orion
alias tmux="tmux -S $TMUX_SOCKET"
```

### 2. Ralph Loop Integration

When running long coding sessions:
```bash
# Start session with stable socket
tmux new-session -d -s ralph-loop -n main

# Attach to session
tmux attach-session -t ralph-loop
```

### 3. Session Recovery

After restart, check for existing sessions:
```bash
tmux list-sessions
tmux attach-session -t ralph-loop
```

## Usage with Ralph Loop

All Ralph Loop agents should run in tmux with stable socket:

1. Check if session exists
2. If not, create with stable socket
3. Run agent in session
4. Detach (session persists)
5. Reattach after restart

## Anti-Patterns

- Don't use default `/tmp` socket
- Don't run long agents outside tmux
- Don't assume session survived restart without checking
- Don't forget to cleanup old sessions
