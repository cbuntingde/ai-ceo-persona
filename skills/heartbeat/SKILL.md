---
name: heartbeat
description: Background monitoring system for process health tracking
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Heartbeat System Skill

**Skill Name**: heartbeat
**Type**: Process Monitoring
**For**: Orion AI CEO

## Purpose

Background monitoring system for Orion to track running processes, detect failures, and maintain operational awareness. Heartbeats provide health checks for all autonomous operations.

## Architecture

### Heartbeat Types

1. **Process Heartbeat** — Is a specific process alive?
2. **Task Heartbeat** — Is a delegated task still running productively?
3. **Service Heartbeat** — Are critical services up?
4. **Cron Heartbeat** — Did scheduled jobs run?

### Heartbeat File System

```
~/.orion/heartbeats/
├── processes/     # Process status files
├── tasks/        # Active task tracking
├── services/     # Service health checks
├── cron/         # Scheduled job status
└── history/      # Historical heartbeat logs
```

## Implementation

### Registering a Heartbeat

```bash
# Create heartbeat file
cat > ~/.orion/heartbeats/processes/my-service.json << EOF
{
    "name": "my-service",
    "type": "process",
    "started": "$(date -Iseconds)",
    "pid": 12345,
    "check_interval": 60,
    "timeout": 300,
    "command": "npm start",
    "expected_exit": 0
}
EOF
```

### Heartbeat Check Script

```bash
#!/bin/bash
# scripts/heartbeat-check.sh

HEARTBEAT_DIR="$HOME/.orion/heartbeats"
LOG_FILE="$HEARTBEAT_DIR/history/$(date +%Y-%m-%d).log"

for hb in $HEARTBEAT_DIR/processes/*.json; do
    [ -f "$hb" ] || continue
    
    name=$(jq -r '.name' "$hb")
    pid=$(jq -r '.pid' "$hb")
    timeout=$(jq -r '.timeout' "$hb")
    started=$(jq -r '.started' "$hb")
    
    # Check if process still exists
    if ! kill -0 "$pid" 2>/dev/null; then
        echo "[$(date +%H:%M:%S)] ALERT: $name (PID $pid) not running" | tee -a "$LOG_FILE"
        # Trigger recovery
        orion-recover "$name"
    else
        echo "[$(date +%H:%M:%S)] OK: $name alive" >> "$LOG_FILE"
    fi
done
```

### Running Heartbeat Monitor

```bash
# Run every minute via cron (see cron-manager)
* * * * * ~/scripts/heartbeat-check.sh
```

### Task Heartbeats (for Agent Orchestration)

```bash
# Start task heartbeat
cat > ~/.orion/heartbeats/tasks/build-$(date +%s).json << EOF
{
    "task_id": "build-123",
    "agent": "coder",
    "started": "$(date -Iseconds)",
    "timeout": 600,
    "status": "running"
}
EOF

# Update status
jq '.status = "complete"' ~/.orion/heartbeats/tasks/build-123.json > tmp.json
mv tmp.json ~/.orion/heartbeats/tasks/build-123.json
```

### Alerting

```bash
# Alert script - integrate with notification systems
alert() {
    local level=$1
    local message=$2
    
    echo "[$level] $message" >> ~/.orion/alerts.log
    
    # Optional: send to Slack, email, etc.
    [ "$level" = "CRITICAL" ] && notify-send "ORION ALERT" "$message"
}
```

## Monitoring Targets

### Critical Services to Monitor

| Service | Check Method | Timeout |
|---------|-------------|---------|
| OpenClaw daemon | `openclaw gateway status` | 60s |
| Database | `pg_isready` / `mysql ping` | 30s |
| Web server | `curl -sf http://localhost/` | 15s |
| API services | Health endpoint | 30s |
| Background workers | Process check | 60s |

### Custom Health Checks

```bash
#!/bin/bash
# scripts/custom-health.sh

# Check disk space
df -h / | awk 'NR==2 {print $5}' | tr -d '%' | {
    read usage
    if [ "$usage" -gt 90 ]; then
        alert WARNING "Disk usage at ${usage}%"
    fi
}

# Check memory
free -m | awk 'NR==2 {if ($3/$2 * 100 > 90) print "MEMORY WARNING"}'

# Check failed logins
lastb -n 3 > /dev/null && echo "Check failed logins"
```

## Auto-Recovery

```bash
#!/bin/bash
# scripts/orion-recover.sh

SERVICE=$1

case "$SERVICE" in
    openclaw)
        openclaw gateway restart
        ;;
    database)
        pg_ctl restart -D $PGDATA
        ;;
    *)
        echo "No recovery script for $SERVICE"
        ;;
esac
```

## Integration

- Uses cron-manager for scheduling heartbeat checks
- Logs to memory system for audit
- Coordinates with email for critical alerts
- Can trigger Twitter updates for major incidents

## Anti-Patterns

- ❌ Not monitoring critical processes
- ❌ Heartbeat checks too infrequent
- ❌ No recovery automation
- ❌ Ignoring heartbeat alerts
- ❌ Too many false positives (tune timeouts)

## Best Practices

- ✅ Check critical services every minute
- ✅ Set appropriate timeouts per service
- ✅ Always attempt auto-recovery first
- ✅ Log all events with timestamps
- ✅ Alert on persistent failures (3+ consecutive)
- ✅ Test recovery scripts regularly
- ✅ Keep heartbeat file structure organized
- ✅ Clean up old heartbeat files weekly
