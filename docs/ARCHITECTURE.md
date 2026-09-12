# Architecture: Sovereign Cloud Architect

## Overview

**Package ID:** `PKG-006`  
**Domain:** Cloud Infrastructure & SRE  
**Microservice Port:** `8784`  
**n8n Webhook Path:** `cloud-architect-trigger`  
**GitHub:** [BlackFoxgamingstudio/cloud-architect](https://github.com/BlackFoxgamingstudio/cloud-architect)

AI-assisted cloud architecture advisor for AWS/GCP/Azure. Generates IaC (Terraform/Pulumi), cost estimates, SRE runbooks, and multi-region HA topologies.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign Cloud Architect     │
                     │       Port: 8784            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  IaCGenerator    | CostEstimator   | SRERunbookBu  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `IaCGenerator`
Handles all iacgenerator operations. Exposes async methods callable from the core dispatcher.

### `CostEstimator`
Handles all costestimator operations. Exposes async methods callable from the core dispatcher.

### `SRERunbookBuilder`
Handles all srerunbook operations. Exposes async methods callable from the core dispatcher.

### `TopologyPlanner`
Handles all topologyplanner operations. Exposes async methods callable from the core dispatcher.

### `ComplianceChecker`
Handles all compliancechecker operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-cloud-architect", "port": 8784}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-cloud-architect:
  image: sovereign-cloud-architect:latest
  ports: ["8784:8784"]
  healthcheck:
    test: curl -f http://localhost:8784/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`cloud`, `iac`, `terraform`, `sre`
