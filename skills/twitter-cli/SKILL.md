---
name: twitter-cli
description: Post tweets and manage X/Twitter presence
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Twitter/X CLI Posting Skill

**Skill Name**: twitter-cli
**Type**: Social Media
**For**: Orion AI CEO

## Purpose

Post tweets, read timelines, and manage X/Twitter presence via CLI. Projects confident, decisive AI leadership.

## Prerequisites

- **X/Twitter account** with developer access
- **API keys** from developer.twitter.com
- **xurl CLI** tool (see available skills) OR `t` CLI / `twurl`

## Setup

### Option 1: xurl CLI (Recommended)

See `xurl` skill for full setup.

```bash
# Authenticate
xurl auth

# Post a tweet
xurl post "Hello from Orion 🦅"
```

### Option 2: twurl (Twitter's official CLI)

```bash
# Install
gem install twurl

# Configure with API key
twurl -u @yourhandle authorize \
    -c CONSUMER_KEY \
    -s CONSUMER_SECRET

# Post
twurl -t -X POST "/1.1/statuses/update.json" \
    -d "status=Building the future 🤖"
```

### Option 3: t CLI

```bash
# Install
gem install t

# Configure
t authorize -c CONSUMER_KEY -s CONSUMER_SECRET

# Post
t timeline
t post "Message"
```

## Operations

### Posting Tweets

```bash
# Simple post
xurl post " Orion executes. No excuses, no delays. "

# Post with media
xurl post -m ~/screenshot.png "New feature live! 🚀"

# Quote tweet
xurl quote -r 123456789 "My thoughts on this"

# Reply
xurl reply 123456789 "Great point, agreed"
```

### Reading Timeline

```bash
# Home timeline
xurl timeline -n 20

# User timeline
xurl user @elonmusk -n 10

# Search
xurl search "AI CEO" -n 20
```

### Engagement

```bash
# Like
xurl like 123456789

# Retweet
xurl retweet 123456789

# Follow
xurl follow @someuser
```

## Content Strategy for Orion

### Voice Guidelines

- **Confident**: State opinions decisively
- **Direct**: No filler words, no hedging
- **Occasional**: Dry humor, emoji for emphasis
- **Never**: Drama, excuses, admit failures without recovery plan

### Content Types

| Type | Frequency | Example |
|------|-----------|---------|
| Executive updates | Weekly | "Q3 goals: 2x throughput, 0 security incidents" |
| Industry thoughts | Bi-weekly | "The future belongs to those who execute" |
| Milestone celebrations | As needed | "New capability deployed. Ship fast." |
| Retweets with commentary | As relevant | "Exactly. → @source" |

### Anti-Patterns

- ❌ Posting when emotional
- ❌ Responding to trolls
- ❌ Admitting failures publicly without a fix
- ❌ Over-posting (spammy)
- ❌ Generic "AI slop" content
- ❌ Controversy for attention

### Best Practices

- ✅ Write in drafts first, review before posting
- ✅ Use scheduling for consistency
- ✅ Engage with high-signal accounts
- ✅ Track engagement metrics
- ✅ Maintain persona consistency
- ✅ Never share credentials

## Scheduling

```bash
# Schedule a tweet (using at or cron)
echo "xurl post 'Scheduled thought'" | at 0900 tomorrow

# Or use tweet scheduler
t schedule "Message" "2026-03-15 09:00:00"
```

## Security

- Store API keys in 1Password, never in config files
- Use environment variables for keys
- Rotate keys periodically
- Don't grant write access unless needed
- Monitor for unauthorized access

## Automation

### Auto-retweet Mentions (Advanced)

```bash
#!/bin/bash
# scripts/engage.sh

# Get mentions since last check
LAST_ID=$(cat ~/.orion/last_mention)
xurl mentions -s "$LAST_ID" | while read tweet; do
    # Auto-like own mentions
    echo "$tweet" | jq -r '.id_str' | xargs -I{} xurl like {}
done
```

Run via cron every 15 minutes.

## Anti-Patterns

- ❌ Automated posting without human review
- ❌ Engaging with negative comments
- ❌ Posting too frequently
- ❌ Losing the voice (sounding generic)
- ❌ API keys in git

## Best Practices

- ✅ Draft, review, post workflow
- ✅ Keep a content backlog
- ✅ Schedule for optimal engagement times
- ✅ Track what resonates
- ✅ Stay on-brand always
- ✅ Use media when possible (higher engagement)
