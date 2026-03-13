---
name: cron-manager
description: Centralized management for scheduled tasks
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Cron Manager Skill

**Skill Name**: cron-manager
**Type**: Scheduling
**For**: Orion AI CEO

## Purpose

Centralized management for all scheduled tasks (cron jobs). Ensures Orion's autonomous operations run reliably without manual intervention.

## Architecture

### Cron Structure

```
~/.orion/crontab/
├── active/           # Currently enabled jobs
├── disabled/         # Paused jobs
├── logs/             # Job execution logs
├── scripts/          # Job runner scripts
└── templates/        # Reusable job templates
```

### Job Naming Convention

Format: `PURPOSE-FREQUENCY-TIME.sh`

Examples:
- `memory-daily-0900.sh` — Daily at 9am
- `heartbeat-minute-*.sh` — Every minute
- `cleanup-weekly-sun.sh` — Weekly on Sunday

## Managing Cron Jobs

### Adding a New Cron Job

```bash
# 1. Create the script
cat > ~/.orion/crontab/scripts/my-task.sh << 'EOF'
#!/bin/bash
# My scheduled task
echo "Running at $(date)" >> ~/.orion/crontab/logs/my-task.log
# ... task commands ...
EOF

chmod +x ~/.orion/crontab/scripts/my-task.sh

# 2. Add to crontab
crontab -l | { cat; echo "0 9 * * * ~/.orion/crontab/scripts/my-task.sh"; } | crontab -

# 3. Verify
crontab -l
```

### Listing All Orion Cron Jobs

```bash
# Filter crontab for Orion-specific jobs
crontab -l | grep -E "orion|ai-ceo|~/.orion"
```

### Disabling/Enabling Jobs

```bash
# Disable (comment out)
crontab -l | sed 's/^#.*my-task/# DISABLED: my-task/' | crontab -

# Or move to disabled/
mv ~/.orion/crontab/active/my-task.sh ~/.orion/crontab/disabled/
```

### Removing Jobs

```bash
# Remove specific line
crontab -l | grep -v "my-task.sh" | crontab -

# Or delete script
rm ~/.orion/crontab/scripts/my-task.sh
```

## Standard Orion Cron Jobs

### Essential Jobs

```bash
# Heartbeat monitor - every minute
* * * * * ~/.orion/scripts/heartbeat-check.sh >> ~/.orion/crontab/logs/heartbeat.log 2>&1

# Memory decay check - weekly Sunday at 3am
0 3 * * 0 ~/memory/decay-check.sh >> ~/memory/logs/decay.log 2>&1

# Cleanup old logs - daily at 2am
0 2 * * * find ~/.orion/crontab/logs -type f -mtime +30 -delete

# Email check - every 15 minutes
*/15 * * * * /usr/local/bin/aerc -v > /tmp/aerc-check 2>&1 || true
```

### Optional Jobs

```bash
# Health check - every 5 minutes
*/5 * * * * ~/scripts/health-check.sh >> ~/logs/health.log 2>&1

# Backup memory - daily at midnight
0 0 * * * tar -czf ~/memory/backups/$(date +\%Y-\%m-\%d).tar.gz ~/memory/

# Social engagement - twice daily
0 8,18 * * * ~/scripts/engage.sh >> ~/logs/engage.log 2>&1

# Git repo sync - hourly
0 * * * * cd ~/projects/main && git fetch --all >> ~/logs/git-sync.log 2>&1
```

## Cron Syntax Reference

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 6) (Sunday=0)
│ │ │ │ │
* * * * * command
```

### Common Examples

| Cron | Meaning |
|------|---------|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour |
| `0 9 * * *` | Daily at 9am |
| `0 9 * * 1-5` | Weekdays at 9am |
| `*/15 * * * *` | Every 15 minutes |
| `0 0 * * *` | Daily at midnight |
| `0 3 * * 0` | Weekly Sunday 3am |

## Logging & Monitoring

### Job Output

```bash
# Capture all output
* * * * * /path/to/script.sh >> ~/.orion/crontab/logs/script.log 2>&1

# Capture errors only
* * * * * /path/to/script.sh 2>> ~/.orion/crontab/logs/errors.log
```

### Log Rotation

```bash
# In crontab entry
0 0 * * * /usr/bin/find ~/.orion/crontab/logs -name "*.log" -mtime +7 -exec gzip {} \;
```

### Monitoring Cron Health

```bash
# Check if cron is running
pgrep -x cron || echo "CRON NOT RUNNING" | mail admin@example.com

# Check last run times
ls -la ~/.orion/crontab/logs/
```

## Anti-Patterns

- ❌ Not logging job output
- ❌ Jobs running without error handling
- ❌ Overlapping runs (same job runs twice)
- ❌ Hardcoded paths
- ❌ Not checking job success/failure
- ❌ Too many frequent jobs (resource waste)

## Best Practices

- ✅ Always use full paths in cron commands
- ✅ Redirect output to log files
- ✅ Set appropriate frequency (don't over-run)
- ✅ Use `run-parts` for directory-based jobs
- ✅ Check cron log: `grep CRON /var/log/syslog`
- ✅ Test jobs manually before adding to cron
- ✅ Keep crontab in version control
- ✅ Use lock files to prevent overlaps

## Lock Files (Prevent Overlap)

```bash
#!/bin/bash
LOCKFILE="/tmp/my-script.lock"

# Check if already running
if [ -f "$LOCKFILE" ]; then
    PID=$(cat "$LOCKFILE")
    if kill -0 "$PID" 2>/dev/null; then
        exit 0  # Already running
    fi
fi

# Create lock
echo $$ > "$LOCKFILE"
trap "rm -f $LOCKFILE" EXIT

# ... do work ...
```

## Integration

- Works with heartbeat for monitoring job execution
- Uses memory-system for logging job history
- Coordinates with email for failure alerts
- Can trigger Twitter updates for milestones
