---
name: google-sheets-docs
description: Google Sheets and Docs API access via service account JWT authentication
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["gcloud"], "env": ["GOOGLE_SERVICE_ACCOUNT_KEY"] },
      "primaryEnv": "GOOGLE_SERVICE_ACCOUNT_KEY"
    }
  }
---

# Google Sheets & Docs Skill

Access Google Sheets and Docs via service account JWT. Read/write spreadsheets, create documents, and maintain collaborative records.

## Setup

### 1. Create Service Account

```bash
# Create service account
gcloud iam service-accounts create openclaw-agent \
  --display-name="OpenClaw Agent"

# Download key
gcloud iam service-accounts keys create ~/openclaw-key.json \
  --iam-account=openclaw-agent@PROJECT.iam.gserviceaccount.com
```

### 2. Share Resources

Share Google Sheets/Docs with the service account email:
```
openclaw-agent@PROJECT.iam.gserviceaccount.com
```

### 3. Set Environment

```bash
export GOOGLE_SERVICE_ACCOUNT_KEY=$(cat ~/openclaw-key.json)
```

## Usage

### Google Sheets

- Read data from spreadsheets
- Write/update cell values
- Create new sheets
- Format cells and ranges

### Google Docs

- Create new documents
- Read document content
- Insert paragraphs/text
- Update specific sections (never full document replacement)

## Best Practices

### Targeted Edits Only

Never replace entire document. Instead:
- Use find/replace for specific text
- Append to specific sections
- Read first, then modify exact location

### Section-Based Editing

Structure documents with clear sections:
```
# Document
## Data Section
{{DATA_GOES_HERE}}

## Notes Section
{{NOTES_GOES_HERE}}
```

Update only the section markers.

## Anti-Patterns

- Don't overwrite entire documents
- Don't store credentials in code
- Don't share service account broadly
- Don't ignore API quotas
