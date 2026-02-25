# CI/CD Pipeline and Software Supply Chain Security Reference

## Overview

The CI/CD pipeline is the conveyor belt of modern software delivery — and a
high-value attack target. A compromised pipeline (SolarWinds, Codecov, xz-utils)
injects malicious code that flows to all downstream consumers.

## CI/CD Threat Landscape

### CICD-SEC-1: Insufficient Flow Control

- **Threat:** Code reaches production without peer review or approval
- **Control:** Require pull request peer reviews on all protected branches
- **Verification:** Check branch protection rules enforce required reviewers

### CICD-SEC-2: Inadequate Identity and Access Management

- **Threat:** Compromised developer account pushes malicious code
- **Control:** Enable MFA for all human users; use SSO with conditional access
- **Verification:** Audit IAM policies quarterly; review access logs for anomalies

### CICD-SEC-3: Dependency Chain Abuse

- **Threat:** Malicious packages via dependency confusion, typosquatting, or hijacked maintainer accounts
- **Control:** Use lockfiles and version pinning; private registry mirrors; verify checksums
- **Verification:** Run SCA on every build; alert on new or changed transitive dependencies

### CICD-SEC-4: Poisoned Pipeline Execution

- **Threat:** Malicious scripts injected into pipeline configuration (e.g., modified CI YAML in PRs)
- **Control:** Pipeline configs stored in protected repos; changes require review
- **Verification:** Hash pipeline definitions and compare before execution

### CICD-SEC-5: Insufficient PBAC (Pipeline-Based Access Controls)

- **Threat:** Pipeline jobs have overly broad permissions to production systems
- **Control:** Apply least privilege to service accounts; use OIDC for cloud auth
- **Verification:** Audit pipeline service account permissions; review token scopes

### CICD-SEC-6: Insufficient Credential Hygiene

- **Threat:** Secrets hardcoded in code, configs, or environment variables; leaked in logs
- **Control:** Use dedicated secret managers (HashiCorp Vault, AWS Secrets Manager)
- **Verification:** Run secrets scanning (e.g., gitleaks, truffleHog) in pre-commit hooks and CI

### CICD-SEC-9: Improper Artifact Integrity Validation

- **Threat:** Tampered artifacts deployed to production
- **Control:** Sign all build artifacts and container images; generate provenance attestations
- **Verification:** Verify signatures at deployment; implement admission controllers for signed images

## Defensive Best Practices

### Identity and Access

- MFA for all human access to SCM and CI/CD systems
- Short-lived tokens or OIDC for machine-to-machine communication (not static API keys)
- Separate service accounts per pipeline stage with minimal permissions
- Audit trail for all administrative actions on CI/CD infrastructure

### Integrity and Provenance

- Sign all commits (GPG/SSH signing)
- Sign build artifacts and container images (cosign, Notation)
- Generate and publish SLSA provenance attestations
- Use lockfiles and checksum verification for all dependency downloads
- Implement Subresource Integrity (SRI) for client-side third-party scripts

### Isolation and Ephemeral Environments

- Execute build and test steps in isolated, ephemeral containers or VMs
- Destroy environments after each run to prevent cross-build contamination
- Use separate build environments for different trust levels (open-source vs. internal)
- Network-isolate build environments from production systems

### Security Scanning Integration

- **SAST** — Run on every commit/PR; fail builds on Critical/High findings
- **DAST** — Run against staging environment on each deployment
- **SCA** — Run on every build; alert on new vulnerabilities in dependencies
- **Deprecated/EOL Scanning** — Identify components reaching support end or using deprecated features
- **Secrets scanning** — Pre-commit hooks + CI stage; block PRs with detected secrets
- **Container scanning** — Scan base images and built images for OS-level vulnerabilities
- **IaC scanning** — Validate Terraform, CloudFormation, Kubernetes manifests for misconfigurations

## Software Supply Chain Maturity

### Level 1: Basic Hygiene

- Lockfiles in all projects
- SCA tool running in CI
- Known vulnerability alerts enabled (e.g., Dependabot, Snyk)

### Level 2: Verified Supply Chain

- SBOM generated for every release
- Artifact signing for all production deployments
- **Deprecated/EOL detection** as a mandatory build gate
- Private registry mirrors for critical dependencies
- Automated license compliance checking

### Level 3: Provenance and Attestation

- SLSA Level 2+ provenance for all artifacts
- Build environment fully ephemeral and reproducible
- Dependency review gate (new dependencies require security team approval)
- Real-time monitoring of dependency health (maintenance, CVE velocity)
