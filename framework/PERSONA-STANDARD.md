# PERSONA-STANDARD.md
# Orion Persona Standards - Mandatory Requirements

> **Version**: 1.0.0  
> **Status**: Required for All Orion Personas  
> **Enforcement**: All personas MUST meet these standards before release

---

## 1. Required File Structure

Every Orion persona MUST include the following files at minimum:

```
persona-name/
├── SOUL.md                 # Core identity & behavior definition
├── MEMORY.md               # Long-term memory architecture
├── HEARTBEAT.md            # Active state & monitoring
├── README.md               # Public documentation
├── SETUP.md                # Installation & configuration
├── LICENSE                 # Usage rights (required for commercial)
├── skills/
│   └── SKILL.md            # Primary skill definition
├── tests/
│   ├── unit/               # Unit tests
│   ├── integration/        # Integration tests
│   └── e2e/                # End-to-end tests
└── config/
    ├── defaults.json       # Default configuration
    └── schema.json         # Configuration schema
```

### File Descriptions

| File | Required | Purpose |
|------|----------|---------|
| SOUL.md | ✅ Yes | Defines persona identity, tone, values, boundaries |
| MEMORY.md | ✅ Yes | Specifies how the persona stores and retrieves information |
| HEARTBEAT.md | ✅ Yes | Active monitoring, health checks, self-maintenance |
| README.md | ✅ Yes | User-facing overview and quick start |
| SETUP.md | ✅ Yes | Installation, configuration, first-run guide |
| LICENSE | ✅ Yes | Commercial licensing terms |
| skills/SKILL.md | ✅ Yes | Primary capability definitions |
| tests/ | ✅ Yes | Must have minimum 80% coverage |
| config/ | ✅ Yes | Default settings and validation schema |

---

## 2. Minimum Skill Count

| Persona Tier | Minimum Skills | Notes |
|--------------|----------------|-------|
| **Starter** | 3 skills | Core functionality only |
| **Professional** | 5 skills | Extended capabilities |
| **Enterprise** | 8 skills | Full-featured, integrations |

### Skill Requirements

- **At least 1 primary skill** must be fully implemented (not stubbed)
- **All skills** must have working unit tests
- **Skills must declare** their dependencies explicitly
- **No hardcoded secrets** - all API keys must come from config/env

---

## 3. Security Requirements

### Authentication & Authorization

```yaml
security:
  # All external API calls must use secure authentication
  api_auth:
    - No hardcoded credentials
    - Support environment variable injection
    - Support secure secret management (Vault, AWS Secrets Manager)
  
  # Data handling
  data_protection:
    - Encrypt sensitive data at rest
    - Use TLS 1.2+ for all network communications
    - Implement proper input sanitization
    - Log security events without exposing PII
```

### Required Security Measures

| Requirement | Implementation | Priority |
|-------------|----------------|----------|
| No hardcoded secrets | Use config + environment variables | **Critical** |
| Input validation | Validate all user inputs before processing | **Critical** |
| Rate limiting | Prevent abuse via request throttling | **High** |
| Audit logging | Log all security-relevant events | **High** |
| SQL injection prevention | Use parameterized queries | **Critical** |
| XSS prevention | Sanitize all outputs | **Critical** |
| CSRF protection | Implement tokens for state-changing operations | **High** |

### Security Headers

All HTTP responses must include:
```
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
```

---

## 4. Memory Architecture Specifications

### Memory Types

Orion personas must implement a layered memory architecture:

```
┌─────────────────────────────────────────────┐
│            EPISODIC MEMORY                  │
│  (Recent conversations, session state)      │
│  Retention: Current session + 7 days        │
├─────────────────────────────────────────────┤
│            SEMANTIC MEMORY                  │
│  (Learned facts, preferences, knowledge)    │
│  Retention: Long-term, with versioning      │
├─────────────────────────────────────────────┤
│            PROCEDURAL MEMORY               │
│  (Skills, workflows, procedures)           │
│  Retention: Indefinite, versioned           │
└─────────────────────────────────────────────┘
```

### Memory Requirements

| Component | Requirement | Implementation |
|-----------|-------------|----------------|
| **Context Window** | Must handle at least 32K tokens | Sliding window with priority |
| **Long-term Storage** | Persistent across sessions | File-based or database |
| **Memory Compression** | Efficient storage | Summarization + embedding |
| **Memory Encryption** | Sensitive data protected | AES-256 or equivalent |
| **Memory Export** | User data portability | JSON/JSONL export |
| **Memory Deletion** | GDPR compliance | Complete wipe capability |

### Memory Schema

```typescript
interface PersonaMemory {
  episodic: {
    conversations: Conversation[];
    lastUpdated: Date;
    maxEntries: number; // minimum 1000
  };
  semantic: {
    facts: Fact[];
    preferences: Preference[];
    knowledgeGraph: Graph;
  };
  procedural: {
    skills: Skill[];
    workflows: Workflow[];
    version: string;
  };
}
```

---

## 5. Testing Requirements

### Coverage Standards

| Test Type | Minimum Coverage | Run Frequency |
|-----------|-----------------|---------------|
| Unit Tests | 80% | Every commit |
| Integration Tests | 70% | Every PR |
| E2E Tests | Core flows | Every release |
| Security Tests | 100% critical paths | Every release |

### Required Test Categories

```yaml
tests:
  unit:
    - All skill handlers
    - Memory operations
    - Configuration parsing
    - Utility functions
    
  integration:
    - API integrations
    - Database operations
    - External service calls
    - Authentication flows
    
  e2e:
    - Complete conversation flows
    - Memory persistence
    - Error recovery
    - Performance under load
```

### Test Quality Gates

- **All tests must pass** before release
- **No flaky tests** - must be deterministic
- **Mock external services** for unit tests
- **Test data cleanup** after each test run
- **Performance benchmarks** must not degrade >10%

---

## 6. Documentation Requirements

### Required Documentation

| Document | Audience | Content Requirements |
|----------|----------|---------------------|
| README.md | End users | Quick start, features, requirements |
| SETUP.md | Administrators | Installation, configuration, deployment |
| API.md | Developers | REST/GraphQL endpoints, schemas |
| CHANGELOG.md | All | Version history, breaking changes |
| LICENSE | Legal | Usage terms, restrictions |

### Documentation Standards

- **Code examples** must be working (tested)
- **Screenshots** must be current (within 2 releases)
- **API docs** must match actual implementation
- **Troubleshooting** section required in SETUP.md
- **Version compatibility** must be explicit

---

## 7. Compliance Requirements

### Data Handling

```yaml
compliance:
  gdpr:
    - Right to access (data export)
    - Right to erasure (complete deletion)
    - Data minimization principle
    - Consent management
    
  ccpa:
    - Opt-out mechanisms
    - Data disclosure requirements
    
  hipaa:  # If handling health data
    - PHI protection
    - Audit trails
    - BAA requirements
```

### Audit & Reporting

- **Audit logs** must be immutable
- **Retention policies** must be configurable
- **Incident response** plan must be documented
- **Security assessments** required annually

---

## 8. Performance Requirements

| Metric | Minimum | Target |
|--------|---------|--------|
| Response latency | < 500ms (P95) | < 200ms |
| Memory footprint | < 512MB | < 256MB |
| Startup time | < 5s | < 2s |
| Concurrent users | 10 | 100+ |

---

## 9. Versioning & Release

### Semantic Versioning

```
MAJOR.MINOR.PATCH
  │    │    └── Bug fixes
  │    └─────── New features (backward compatible)
  └──────────── Breaking changes
```

### Release Checklist

- [ ] All tests passing
- [ ] Security scan completed
- [ ] Documentation updated
- [ ] CHANGELOG updated
- [ ] Version bumped
- [ ] Artifacts published

---

## Enforcement

These standards are **mandatory**. Non-compliant personas will not be:
- Listed in the Orion marketplace
- Eligible for enterprise support
- Covered by Orion SLA

**Last Updated**: 2026-03-11
**Next Review**: 2026-06-11
