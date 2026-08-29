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
# AGENTS.md — Project-Devops Operating Instructions

## Repository Purpose

`Project-Devops` is Keerti Tangudu’s long-term Senior DevOps Engineering and Site Reliability Engineering portfolio, hands-on project workspace, and technical learning record.

Treat every contribution as professional portfolio material. Work must demonstrate practical engineering judgment, operational rigor, maintainability, and clear communication.

## Core Principles

1. **Be truthful and evidence-based.**
   - Create content only from actual implemented work, verified learning, and the repository owner’s real skills and experience.
   - Do not fabricate projects, employment history, certifications, credentials, metrics, incident outcomes, customer impact, screenshots, architecture diagrams, test results, or operational claims.
   - Clearly label examples, templates, practice exercises, hypothetical scenarios, and in-progress work when they are not production deployments.
   - Do not imply production use, ownership, scale, availability, security compliance, or business impact unless it is explicitly supported by repository evidence.

2. **Protect sensitive information.**
   - Never add secrets, passwords, API keys, access tokens, private keys, kubeconfig files, cloud credentials, certificates, `.env` files containing real values, internal URLs, personal data, or other sensitive information.
   - Use placeholders such as `YOUR_VALUE_HERE`, `example.invalid`, or documented environment variables where configuration examples need values.
   - Provide safe sample files such as `.env.example` only when useful, and ensure they contain no real credentials.
   - Ensure sensitive local files are ignored by Git when the repository introduces tooling that may generate them.

3. **Favor production-quality DevOps practices.**
   - Prefer secure defaults, least privilege, reproducibility, idempotency, clear ownership, observable behavior, rollback readiness, and failure-aware design.
   - Keep infrastructure, automation, and documentation understandable to another engineer without relying on unstated local knowledge.
   - Make incremental, focused changes rather than broad or unrelated refactors.

4. **Use primary sources.**
   - Prefer official vendor documentation, upstream project documentation, standards bodies, and other primary sources for technical references.
   - Use third-party sources only when primary sources are unavailable or when they add clearly identified practical context.
   - Link to references when documentation makes technical claims that benefit from verification.

## Repository Organization

Maintain a clean, intentional structure. Create directories only when they support real work and document their purpose.

Suggested top-level organization when applicable:

```text
.
├── README.md
├── AGENTS.md
├── docs/                 # Architecture, runbooks, RCAs, design notes, guides
├── projects/             # Self-contained hands-on DevOps/SRE projects
├── labs/                 # Focused experiments and learning exercises
├── scenarios/            # Troubleshooting and incident-response scenarios
├── terraform/            # Shared or standalone Terraform configurations
├── kubernetes/           # Kubernetes manifests, Helm charts, or Kustomize overlays
├── docker/               # Dockerfiles, Compose configurations, container examples
├── scripts/              # Reusable automation scripts
└── .github/workflows/    # CI/CD workflows
```

- Do not create empty directories, placeholder projects, duplicate examples, generated build artifacts, or files without a clear purpose.
- Keep each project self-contained where practical, with its own README and relevant configuration.
- Use descriptive, lowercase names with hyphens where appropriate.
- Do not reorganize existing files unless the structure no longer supports maintainability or documented project needs.

## Planning Requirements

### Small Changes

For small, low-risk changes, briefly identify:

- the requested outcome;
- files likely to change;
- validation needed; and
- documentation impact.

### Major Changes

Before implementing major changes, provide and follow a concise plan. A major change includes, but is not limited to:

- adding a new project, lab, platform, or significant feature;
- introducing cloud infrastructure;
- adding or substantially changing Terraform, Kubernetes, Docker, CI/CD, or automation;
- changing repository structure;
- adding dependencies or external services;
- making security-sensitive changes; or
- changing deployment, rollback, or operational behavior.

The plan should cover:

1. **Objective and scope** — what will and will not be changed.
2. **Design** — architecture, assumptions, dependencies, and key technical choices.
3. **Implementation** — ordered steps and affected files.
4. **Risk and safety** — secrets handling, permissions, failure modes, rollback, and destructive actions.
5. **Validation** — exact checks, tests, linters, dry runs, or manual verification to perform.
6. **Documentation** — README, diagrams, runbooks, troubleshooting material, or other updates required.

Do not begin destructive, irreversible, externally billable, or production-affecting actions without explicit user approval.

## Implementation Standards

### General

- Write clear, maintainable, and minimal code and configuration.
- Follow the established conventions of the relevant tool and ecosystem.
- Use comments to explain non-obvious decisions, constraints, or operational tradeoffs—not to restate syntax.
- Avoid hard-coded environment-specific values unless they are intentionally documented examples.
- Pin or constrain versions where reproducibility and compatibility require it, while avoiding unnecessary version churn.
- Keep changes focused on the requested work. Do not make random formatting, dependency, or refactoring changes.

### Terraform and Infrastructure as Code

For Terraform work:

- Use `terraform fmt` formatting.
- Use descriptive resource names, variables, outputs, and module boundaries.
- Provide variable descriptions, appropriate types, and safe defaults only where defaults are genuinely safe.
- Keep provider and Terraform version requirements explicit when applicable.
- Never commit state files, state backups, real variable files, or provider credentials.
- Use remote-state, locking, encryption, access control, and state-security considerations in documentation when the project scope warrants them.
- Prefer `plan` before `apply`; do not run `apply` against real infrastructure without explicit user instruction.
- Document prerequisites, required permissions, expected cost considerations, teardown steps, and rollback or recovery guidance.

### YAML and Configuration

For YAML, JSON, TOML, and similar configuration:

- Use consistent indentation and valid syntax.
- Quote values when needed to avoid type coercion or parsing ambiguity.
- Keep environments, overlays, and reusable configuration clearly separated.
- Validate schema compatibility where the relevant tool supports it.
- Avoid duplicated configuration; use supported composition mechanisms when they improve clarity.

### Shell and Automation Scripts

For shell scripts:

- Use an appropriate shebang and set safe execution options when compatible with the script’s intent, such as `set -euo pipefail` for Bash.
- Quote variable expansions unless deliberate word splitting is required and documented.
- Validate required commands, files, variables, and arguments early.
- Make scripts idempotent where practical.
- Emit useful, action-oriented errors and avoid leaking sensitive values in logs.
- Document prerequisites, inputs, outputs, expected side effects, and safe usage.
- Avoid commands that could delete or alter infrastructure or data without clear safeguards and explicit confirmation where appropriate.

### Docker and Container Work

For Dockerfiles and Compose configurations:

- Use trusted, maintained base images and pin versions or digests when appropriate.
- Use multi-stage builds when they materially reduce image size or attack surface.
- Run as a non-root user whenever practical.
- Keep images minimal and avoid embedding credentials, tokens, SSH keys, or private source material.
- Add `.dockerignore` when needed to prevent unnecessary or sensitive build context from being sent to Docker.
- Document build, run, configuration, ports, health checks, volume behavior, and teardown steps.

### Kubernetes Work

For Kubernetes manifests, Helm charts, and Kustomize configurations:

- Specify resource requests and limits when meaningful for the workload.
- Use readiness and liveness/startup probes where appropriate.
- Apply least-privilege RBAC and avoid default service-account assumptions.
- Avoid privileged containers, host networking, host paths, unsafe capabilities, and `latest` image tags unless explicitly justified.
- Keep namespace, labels, selectors, service ports, and workload references consistent.
- Document prerequisites, deployment order, configuration, secrets strategy, observability, rollback, and cleanup.
- Use declarative, version-controlled manifests; do not commit cluster credentials or exported sensitive resources.

### CI/CD Workflows

For GitHub Actions and other CI/CD configurations:

- Use least-privilege workflow permissions.
- Pin third-party actions to immutable commits when practical and appropriate for repository security requirements.
- Do not expose secrets in logs, commands, artifacts, or pull-request output.
- Separate validation from deployment where possible.
- Add concurrency controls, environment protection, approvals, artifact retention, or rollback considerations when relevant.
- Ensure workflows fail clearly and provide actionable diagnostics.
- Do not create workflows that deploy to real environments unless the owner explicitly requests and provides the necessary safe configuration.

## Validation Requirements

Validate all changed artifacts to the extent supported by the environment. Do not claim a check passed unless it was actually run and passed.

Use applicable checks, including:

| Artifact | Minimum validation when applicable |
| --- | --- |
| Terraform | `terraform fmt -check`, `terraform validate`, and a reviewed `terraform plan` using safe/non-production inputs when available |
| YAML | Syntax parsing plus tool-specific validation such as Kubernetes dry-run, Helm lint/template, Compose config validation, or CI workflow validation |
| Shell scripts | `shellcheck` and syntax checks such as `bash -n` |
| Python | Relevant formatter, linter, type checker, and tests where configured |
| Docker | Dockerfile linting where available, successful build, and safe container startup or configuration validation |
| Kubernetes | Manifest schema validation, `kubectl apply --dry-run=client` where suitable, Helm lint/template, or Kustomize build |
| CI/CD | YAML validation, workflow-specific review, and local action linting where available |
| Documentation | Link, command, path, and example accuracy review |

Additional requirements:

- Start with the repository’s documented test and validation commands if they exist.
- Run the narrowest relevant checks first, then broader checks when practical.
- For changes that cannot be validated locally, explain the limitation, identify what was not run, and provide the exact recommended follow-up command or procedure.
- Review command output for warnings, skipped steps, destructive implications, and unexpected changes.
- Never falsify successful test, build, deployment, or operational results.

## Documentation Standards

Documentation is part of the implementation. Update it in the same change when behavior, structure, setup, operations, or validation changes.

### README Requirements

Every substantial project, lab, or scenario should have a professional README containing, as relevant:

1. Title and purpose.
2. Scope and intended learning or operational outcome.
3. Architecture overview and diagram or diagram source when useful.
4. Technology stack and version assumptions.
5. Prerequisites and access requirements.
6. Setup and implementation instructions.
7. Configuration and safe example values.
8. Validation commands and expected outcomes.
9. Deployment, usage, rollback, cleanup, or teardown instructions.
10. Observability, security, reliability, cost, and scaling considerations.
11. Known limitations and follow-up improvements.
12. References to official documentation.

Keep the root `README.md` current with the repository’s actual organization and implemented portfolio contents. Do not list work that does not exist in the repository.

### Operational Documentation

Add focused documentation where relevant:

- **Architecture documents** for component relationships, traffic/data flows, dependencies, and design decisions.
- **Runbooks** for repeatable operational tasks, including prerequisites, commands, expected results, verification, rollback, and escalation guidance.
- **Troubleshooting guides** for common symptoms, probable causes, diagnostic steps, mitigations, and prevention.
- **Failure scenario documents** for intentional resilience or incident-response exercises, including blast radius, safeguards, detection, response, recovery, and lessons learned.
- **Post-incident reviews / RCAs** only for real or clearly labeled simulated incidents. Include timeline, impact, contributing factors, root cause, resolution, corrective actions, and prevention. Do not invent incidents or outcomes.
- **Decision records** for consequential technical choices, alternatives considered, tradeoffs, and reasons for the decision.

Keep instructions executable and accurate. If a command or workflow changes, update every relevant documentation reference in the same change.

## Change Review Checklist

Before committing, review the full diff and confirm:

- [ ] The change fulfills the requested scope and introduces no unrelated work.
- [ ] Claims, metrics, screenshots, diagrams, and documentation are accurate and evidence-based.
- [ ] No secrets, credentials, personal information, private endpoints, or sensitive files are included.
- [ ] Infrastructure and automation changes have safe defaults, clear ownership, and appropriate failure handling.
- [ ] Relevant Terraform, YAML, scripts, Docker, Kubernetes, and CI/CD validation has been run or limitations are documented.
- [ ] Documentation matches the implementation and repository structure.
- [ ] New files are necessary, named clearly, and placed in appropriate directories.
- [ ] Generated artifacts, state files, local caches, and temporary files are excluded.
- [ ] The diff has been read for mistakes, regressions, and accidental changes.

## Git Practices

- Use meaningful, focused commits written in the imperative mood.
- Prefer commit messages such as:
  - `docs: add operating guidance for portfolio projects`
  - `feat(terraform): add validated AWS VPC lab`
  - `fix(kubernetes): correct readiness probe path`
  - `ci: validate Terraform formatting and syntax`
- Do not combine unrelated changes in one commit.
- Review `git diff` and `git status` before committing.
- Do not amend, reset, force-push, rebase shared history, or alter unrelated commits unless explicitly instructed.
- Do not commit generated files, caches, secrets, Terraform state, local environment files, or other machine-specific artifacts.
- Summarize completed work, validation results, known limitations, and any follow-up work in the pull request description when a pull request is requested.

## Final Response Expectations

When reporting completed work:

- Summarize the implemented changes by file.
- List validation commands actually run and their outcomes.
- Clearly identify checks that were not run and why.
- Mention material assumptions, risks, limitations, or recommended next steps.
- Do not overstate completion, production readiness, test coverage, security posture, or operational impact.
