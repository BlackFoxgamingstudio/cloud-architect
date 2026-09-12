# Changelog — Sovereign Cloud Architect

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign Cloud Architect (PKG-006)
- Core microservice on port `8784`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: IaCGenerator, CostEstimator, SRERunbookBuilder, TopologyPlanner, ComplianceChecker
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-cloud-architect --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Cloud Infrastructure & SRE
AI-assisted cloud architecture advisor for AWS/GCP/Azure. Generates IaC (Terraform/Pulumi), cost estimates, SRE runbooks, and multi-region HA topologies.

[Unreleased]: https://github.com/BlackFoxgamingstudio/cloud-architect/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/cloud-architect/releases/tag/v1.0.0
