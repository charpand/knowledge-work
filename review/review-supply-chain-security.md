# Review Supply Chain Security for a Repository

## Name
Review Supply Chain Security for a Repository

## Purpose
Audits a repository's dependency hygiene and CI/CD pipeline trust posture to surface supply chain risks before they reach production. Applied during security review cycles, onboarding of new repositories, or in response to an upstream compromise. Produces a structured finding set covering every dependency ecosystem in use, GitHub Actions workflow integrity, and the repository's overall SLSA posture.

## Inputs

### Required
- Repository source tree (or pull request diff when auditing incremental changes)
- List of package ecosystems in use (e.g., npm, pip, Go modules, Maven, NuGet, Terraform, Docker)
- CI/CD configuration files (GitHub Actions workflow files, Dockerfile, Makefile targets that install packages)

### Optional
- Existing SBOM artifact (CycloneDX or SPDX format) for baseline comparison
- Previous audit findings or known exceptions
- Organizational policy on accepted SLSA level, trusted registries, and allowed third-party action publishers

## Referenced Skills
- GitHub CI/CD Review
- Define and Enforce a Quality Bar

## Process Constraints
- Audit every ecosystem present in the repository; do not skip a section because it appears low-risk
- GitHub Actions SHA pinning is a first-class requirement, not an optional hardening step; any mutable tag reference (`@v4`, `@main`, `@latest`) is a blocking finding regardless of publisher reputation
- Third-party actions without an explicit trust decision in organizational policy must be flagged even when SHA-pinned
- Treat each package manager lockfile as authoritative; discrepancies between manifest and lockfile are blocking
- Do not propose version upgrades or feature changes; scope is limited to supply chain integrity findings
- SLSA level assessment must be evidence-based; do not assign a level without citing the specific controls (or their absence) that determine it
- Findings must reference the specific file and line (or stanza) where the risk originates

## Output Contract

The audit must contain:

1. **Repository Overview**: Ecosystems detected, CI/CD platform(s) in use, and SLSA level assessment (SLSA 0–3) with rationale
2. **GitHub Actions Findings**: One entry per workflow file, covering:
   - SHA-pinned vs. mutable-tag action references (blocking if mutable)
   - Third-party actions without an explicit trust decision (blocking)
   - Secrets exposure patterns (e.g., `${{ github.event.inputs.* }}` used unsanitised in `run:` steps)
   - Workflow trigger surfaces that increase injection risk (`pull_request_target`, `workflow_dispatch` with unvalidated inputs)
3. **Ecosystem Findings**: One section per ecosystem present, each covering lockfile integrity, hash or checksum verification, known-vulnerability scan results, and provenance/attestation posture
4. **Container and OCI Findings**: Image digest pinning status, Cosign signature verification, and SBOM attestation availability
5. **SBOM Status**: Whether a CycloneDX or SPDX SBOM is generated in CI, attached to releases, and covers all first-party and transitive dependencies
6. **Cross-cutting Observations**: Patterns that span multiple ecosystems (e.g., no dependency review gate in any workflow, no signing key rotation process)
7. **Verdict**: One of — `Compliant`, `Compliant with observations`, or `Non-compliant` — with explicit rationale
8. **Prioritised Remediation List**: Ordered list of blocking findings (must fix) and non-blocking observations (should fix), each traceable to a specific finding above

Format: Structured Markdown document, one section per ecosystem plus the cross-cutting sections, 1,500–3,000 words depending on repository size.

## Quality Checks

Before delivery, validate:

### GitHub Actions
- [ ] Every `uses:` reference in every workflow file has been checked for SHA pinning; mutable tags are listed as blocking
- [ ] Third-party actions (outside `actions/`, `github/`, and explicitly trusted publishers) are individually enumerated with trust-decision status
- [ ] `pull_request_target` and `workflow_dispatch` triggers are reviewed for privilege escalation paths
- [ ] Secrets are not echoed, logged, or interpolated into shell commands without sanitisation

### npm / pnpm / yarn
- [ ] `package-lock.json`, `pnpm-lock.yaml`, or `yarn.lock` is committed and not in `.gitignore`
- [ ] `npm audit` (or `pnpm audit` / `yarn audit`) output reviewed; high/critical advisories are blocking
- [ ] `npm pack --dry-run` reviewed to confirm no unexpected files are included in published packages
- [ ] Provenance attestation (`npm publish --provenance`) is enabled for published packages, or absence is noted as an observation

### pip / Poetry / uv
- [ ] `requirements.txt` pins packages with `==` and includes hashes (`--hash=sha256:…`), or `poetry.lock` / `uv.lock` is committed
- [ ] `pip-audit` or `poetry audit` output reviewed; high/critical advisories are blocking
- [ ] Direct installs from Git URLs or non-PyPI sources are flagged and individually justified

### Maven / Gradle
- [ ] Dependency verification file (`gradle/verification-metadata.xml` for Gradle; enforced checksum plugin for Maven) is present and covers all resolved artifacts
- [ ] Snapshot dependencies in production builds are flagged as blocking
- [ ] Repository configuration restricts resolution to trusted registries (no wildcard `*` in repository list)

### NuGet
- [ ] `packages.lock.json` is committed and `RestoreLockedMode` is enabled
- [ ] Package Source Mapping (`nuget.config` `<packageSourceMapping>`) restricts each package to a single trusted feed
- [ ] Packages with NuGet signed-package verification disabled are flagged

### Go modules
- [ ] `go.sum` is committed and not in `.gitignore`
- [ ] `GONOSUMCHECK` and `GONOSUMDB` environment variables are not set to bypass the sum database in CI (or bypass is individually justified)
- [ ] `govulncheck` or `go mod audit` output reviewed; high/critical advisories are blocking
- [ ] `replace` directives pointing to local paths or unversioned forks are flagged

### Docker / OCI images
- [ ] Every `FROM` instruction and every `docker pull` in CI scripts uses a digest-pinned reference (`image@sha256:…`), not a mutable tag
- [ ] Images from third-party registries are verified with Cosign (`cosign verify`) before use where signatures are available
- [ ] SBOM attestation is attached to published images (`cosign attest` with CycloneDX or SPDX predicate)
- [ ] Base image source is a trusted registry; `latest` tag is blocking in production-bound images

### Terraform / OpenTofu modules
- [ ] All `module` blocks pin an explicit `version` constraint; open-ended constraints (`>= x`) are flagged as observations
- [ ] Registry sources are validated; `git::` or `github.com/` references without a pinned `ref` are blocking
- [ ] Provider `required_providers` block pins versions with `~>` or `=` constraints

### Homebrew / apt / system packages in CI
- [ ] `apt-get install` and `brew install` commands in CI scripts pin package versions where supported
- [ ] Scripts that `curl | bash` or pipe remote content to a shell are blocking findings
- [ ] Package installations over plain HTTP (not HTTPS) are blocking

### SBOM and SLSA
- [ ] SBOM generation step is present in CI and produces either a CycloneDX or SPDX document
- [ ] SBOM is attached to release artifacts or published to a dependency-track or similar inventory
- [ ] SLSA level claim (if any) is supported by documented build provenance controls
- [ ] Provenance attestation is generated and verifiable for published artifacts (`slsa-github-generator` or equivalent)

### Verdict consistency
- [ ] `Non-compliant` verdict is present if any blocking finding exists
- [ ] All blocking findings appear in the Prioritised Remediation List
- [ ] Non-blocking observations are clearly distinguished from blocking findings throughout
