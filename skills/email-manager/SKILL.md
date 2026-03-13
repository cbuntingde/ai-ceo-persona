---
name: email-manager
description: Secure email management via CLI
metadata: { "openclaw": { "always": true, "requires": {}, "primaryEnv": null } }
---

# Email Manager Skill

**Skill Name**: email-manager
**Type**: Communication
**For**: Orion AI CEO

## Purpose

Secure email management for Orion to send and receive communications via CLI, with strict security practices.

## Prerequisites

- **1Password CLI** configured (see `1password` skill)
- **Email account** with app-specific password or OAuth token
- **SMTP/IMAP access** or use a CLI email client

## Supported Clients

### Option A: aerc (Recommended)

Modern terminal email client with good security defaults.

```bash
# Install
brew install aerc

# Configure ~/.config/aerc/aerc.conf
[accounts]
    personal

[account-personal]
    source = imaps://username@imap.example.com:993
    outgoing = smtps://username@smtp.example.com:465

# Store credentials via 1Password
op inject -i ~/.config/aerc/account -o ~/.config/aerc/account-secure
```

### Option B: mutt

Classic, lightweight email client.

```bash
# Install
brew install mutt

# Configure ~/.muttrc with app-specific password from 1Password
```

## Email Operations

### Sending Email

```bash
# Quick send with aerc
echo "Body text" | aerc send -t recipient@example.com -s "Subject"

# Or use msmtp for simple sending
echo -e "Subject: Test\n\nBody" | msmtp recipient@example.com
```

### Reading Email

```bash
# List inbox with aerc
aerc

# Or via IMAP with offlineimap + notmuch for search
offlineimap && notmuch search tag:inbox
```

## Security Requirements

### Hard Rules

1. **NEVER** log credentials in plain text
2. Use **1Password** for all passwords/app tokens
3. Enable **2FA** on email account
4. Use **app-specific passwords** for CLI access
5. **Encrypt** local mail storage if sensitive
6. **Don't** include secrets in email body

### Credential Management

```bash
# Store email credentials in 1Password
op item create --title "Email CLI" --category "Login" \
    username="user@example.com" \
    password="$(openssl rand -base64 32)"

# Inject for use
eval $(op signin)
op read "op://Personal/Email CLI/password" | \
    sed 's/^/set imap_pass=/' >> ~/.config/aerc/account
```

### Network Security

- Use IMAPS (993) and SMTPS (465) only
- Consider VPN for sensitive email access
- Don't access email over public WiFi without VPN

## Automation

### Email Polling

```bash
# cron job to check email every 15 minutes
*/15 * * * * /usr/local/bin/aerc -v > /tmp/aerc-check 2>&1 || true
```

### Auto-Response Rules

Set up in aerc via filters:
```
# ~/.config/aerc/filters
from:noreply@github.com -> pipe ~/scripts/github-notify.sh
```

## Anti-Patterns

- ❌ Storing passwords in config files
- ❌ Using plain IMAP (993) instead of IMAPS
- ❌ Clicking email links (always copy URL)
- ❌ Replying from memory without context
- ❌ Sending sensitive data unencrypted

## Best Practices

- ✅ Use app-specific passwords (limited scope)
- ✅ Review sent folder after automation
- ✅ Set up email aliases for different contexts
- ✅ Archive old emails regularly
- ✅ Use PGP for sensitive communications
- ✅ Log important emails to memory system

## Integration

- Use 1Password skill for credential management
- Log significant communications to memory
- Coordinate with heartbeat for monitoring
- May integrate with X/Twitter for public communications
