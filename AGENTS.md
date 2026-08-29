# AGENTS.md — Repository Operating Instructions

## Purpose

### What is this repository for?

`KeertiTangudu/Project-Devops` is a long-term Senior DevOps Engineering portfolio and hands-on project workspace. Treat every contribution as production-quality engineering work that is clear, credible, reproducible, secure, maintainable, and suitable for professional review.

### Where do these instructions apply?

These instructions apply throughout the repository unless a more-specific `AGENTS.md` exists in a subdirectory.

## Core Principles

### What must Codex prioritize?

Prioritize correctness, security, maintainability, reproducibility, operational usefulness, and clear documentation.

### What claims may Codex make?

Only make original, factual claims grounded in the repository owner's actual skills, verified experience, and hands-on work. Do not fabricate or imply unverified projects, employers, roles, experience, certifications, credentials, metrics, incident outcomes, customer details, or technical results.

### What should Codex do when information is missing?

Use clearly labeled placeholders, assumptions, or TODO items. Do not invent details to fill gaps.

### What sensitive information is prohibited?

Never add, expose, log, commit, or document secrets, credentials, access tokens, API keys, private keys, certificates, kubeconfigs, production endpoints, personally identifiable information, or other sensitive data.

### What implementation style should Codex use?

Prefer simple, clear, and maintainable solutions. Avoid unnecessary complexity, unrelated refactors, random edits, generated clutter, duplicate content, and unused files.

## Planning

### When is a plan required?

Create and follow a concise plan before a major change, including a new project, lab, environment, platform, service, infrastructure technology, shared workflow, repository structure, architecture, or production-like policy, permission, or network configuration.

### What must a major-change plan include?

A plan must state the objective and scope; assumptions, prerequisites, and constraints; expected files; architecture and implementation approach; security and operational considerations; validation steps and acceptance criteria; and required documentation updates.

### How should Codex handle small changes?

Use sound judgment and keep planning proportionate to the change's risk and scope.

## Repository and Infrastructure Standards

### How should the repository be organized?

Keep the structure intentional, navigable, and scalable. Group related documentation, infrastructure code, manifests, scripts, and validation instructions together. Use descriptive names; do not use vague names such as `test`, `new`, `misc`, `temp`, or `final`.

### Which local or generated files may be committed?

Commit local-state, editor, cache, build, and environment-specific files only when they are deliberately required and documented. Update `.gitignore` when necessary to prevent local artifacts or sensitive files from being committed.

### When should reusable components be created?

Prefer reusable modules, templates, and scripts only when they reduce meaningful duplication without obscuring intent.

### What standards apply to infrastructure as code?

Use declarative, readable, and maintainable configuration. Pin tool, provider, action, image, and module versions where practical. Document versions, prerequisites, inputs, outputs, state expectations, and safe deployment procedures. Do not hard-code sensitive values or private environment-specific identifiers; use variables, secret-management integrations, and safe placeholder examples instead.

### How should examples and environments be identified?

Clearly distinguish examples, sandbox or lab configurations, and production-oriented configurations.

## Technology-Specific Requirements

### What must Codex do when Terraform changes?

Run `terraform fmt`, initialize as needed, and run `terraform validate`. Run and review `terraform plan` only when credentials, provider access, and the target environment make it safe. Never commit Terraform state, state backups, or sensitive plan output. Document backend, state locking, provider requirements, variables, permissions, and safe destroy or rollback considerations. Use least-privilege IAM.

### What must Codex do when YAML or other configuration changes?

Maintain valid syntax, consistent indentation, clear names, explicit values, and environment-appropriate settings. Validate with an appropriate parser or linter when available. Avoid unnecessary duplication, and comment only to explain non-obvious operational choices.

### What must Codex do when shell scripts or automation change?

Use a clear shebang and safe error handling such as `set -euo pipefail`, unless a documented exception is necessary. Quote variables, validate inputs, make scripts idempotent where feasible, and require explicit confirmation or flags for destructive actions. Never print secrets or pass them unsafely in command-line arguments. Run `shellcheck` when available and document usage, prerequisites, expected behavior, and failure handling.

### What must Codex do when Docker files change?

Use trusted, minimal, version-pinned base images where practical. Use multi-stage builds when they materially reduce image size or attack surface. Run containers as a non-root user unless documented otherwise. Do not bake in secrets or environment-specific configuration. Add `.dockerignore` when relevant, validate with `docker build`, run applicable smoke tests, and document build, run, health-check, configuration, and troubleshooting procedures.

### What must Codex do when Kubernetes resources change?

Use explicit namespaces, labels, selectors, resource requests and limits, and health probes where applicable. Apply least privilege through RBAC, service accounts, security contexts, network policies, and pod-security controls where relevant. Avoid mutable tags such as `latest`; separate base configuration from environment overlays or values. Validate with appropriate available tools, such as `kubectl apply --dry-run=client`, `kubeconform`, `kubeval`, Helm linting, or Kustomize build. Document deployment, rollback, scaling, upgrades, backups, security, troubleshooting, and likely failure behavior.

### What must Codex do when CI/CD configuration changes?

Keep workflows deterministic, auditable, and minimal. Pin third-party actions, images, and dependencies where feasible; use least-privilege permissions; and reference stored secret names without exposing values. Include appropriate quality gates, deployment boundaries, rollback expectations, and failure reporting. Do not automate production-impacting actions unless explicitly requested and safely designed. Document triggers, required secrets or variables, stages, artifacts, approvals, and rollback behavior.

## Validation

### What validation is required for every change?

Run the formatting, linting, syntax validation, tests, and build checks relevant to the changed files. Review output for warnings, errors, destructive actions, security concerns, and unexpected behavior.

### Which validation commands may be relevant?

Use only checks relevant to the change and available in the environment, for example:

```bash
terraform fmt -check -recursive
terraform validate
shellcheck path/to/script.sh
docker build -t project-image:local .
kubectl apply --dry-run=client -f path/to/manifest.yaml
helm lint path/to/chart
kustomize build path/to/overlay
yamllint .
```

### What if a required validation check cannot run?

State why it could not run and provide the exact command that should be run in a suitable environment. Never claim validation, deployment, benchmark, certification, or operational results that were not actually performed.

## Documentation and References

### When must documentation be updated?

Keep documentation synchronized with implementation. Update the root `README.md` for repository-wide structure, navigation, standards, or project-inventory changes. Add a project-level README for each self-contained project or lab.

### What should meaningful project documentation answer?

It should answer: What is the objective, scope, status, and intended outcome? What architecture, components, flows, trust boundaries, and dependencies exist? What prerequisites, versions, access, and setup are required? How is it implemented, deployed, validated, operated, secured, scaled, upgraded, backed up, recovered, and rolled back? What failure scenarios, detection signals, containment steps, recovery procedures, troubleshooting commands, limitations, and future work exist?

### When is an RCA appropriate, and what must it include?

Create an RCA only for a real incident or a clearly labeled simulation. It must answer: What happened, when, what was the impact, what was the root cause, which factors contributed, what corrective actions were taken, and how will recurrence be prevented? Never represent a simulation as a real incident.

### Which technical references should be used?

Prefer official documentation, upstream project documentation, vendor documentation, standards bodies, and other primary sources. Use secondary sources only when they add useful context and do not conflict with primary sources. Link directly to relevant sources where practical, verify version-sensitive claims, and summarize in original language rather than copying substantial text.

## Security

### How should repository content be treated?

Treat every repository file as potentially public.

### Which files and values must never be committed?

Do not commit secret-bearing `.env` files, cloud credential files, private keys, kubeconfigs, Terraform state, token files, production configuration exports, sensitive logs, or other sensitive material. Use safe templates, such as `.env.example` or `terraform.tfvars.example`, with clearly fake placeholder values when appropriate.

### Which identifying details require explicit confirmation before inclusion?

Do not include real personal email addresses, IP addresses, hostnames, account, subscription, or tenant IDs, customer information, or internal URLs unless the repository owner confirms they are public and safe to disclose.

### What must Codex do if sensitive information is discovered?

Stop, remove it from the working tree where safe, and notify the repository owner. Do not propagate it.

## Review, Commits, and Maintenance

### What must Codex review before committing?

Inspect `git status` and the full diff; confirm only intentional files changed; check for secrets, generated artifacts, temporary files, and sensitive data; run applicable validation; synchronize documentation; and review naming, structure, error handling, security, operations, and factual accuracy.

### How should commits be written?

Keep each commit focused on one coherent change and use a concise, imperative subject, such as `Add Kubernetes deployment validation guide`. Do not use vague messages such as `updates`, `fix`, `changes`, or `wip`. Do not commit unfinished, broken, or unrelated work unless explicitly requested, and do not rewrite shared history or force-push unless explicitly directed.

### How should this repository be maintained over time?

Keep dependencies, examples, versions, instructions, project indexes, READMEs, diagrams, runbooks, and validation evidence current. Remove obsolete, duplicated, unused, or misleading content; revalidate examples after tool or platform changes; record limitations and assumptions honestly; and prefer incremental, well-documented improvements over large undocumented changes.
