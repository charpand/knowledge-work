# Review Engineering Submission for a PHP/Symfony Web Stack

## Name
Review Engineering Submission for a PHP/Symfony Web Stack

## Purpose
Performs systematic design-quality review of code submissions for modern PHP/Symfony web applications, covering the full stack from backend domain logic to frontend markup and CI/CD pipeline configuration. Applied during pull request review or architectural design assessment to distinguish robust, maintainable designs from those that introduce hidden coupling, accessibility debt, or operational fragility.

## Inputs

### Required
- Code submission (pull request diff, code snippets, or file set)
- Scope of the submission (new feature, refactor, integration, infrastructure change, or combination)
- Target environment context (Symfony version, PHP version, deployment mechanism)

### Optional
- Prior review findings or known technical debt in the affected areas
- Architectural decisions or constraints that predetermine certain design choices
- Acceptance criteria or definition of done from the originating ticket

## Referenced Skills
- PHP Design Review
- Twig Template Review
- Symfony Design Review
- HTML5 Accessibility Review
- API Platform Design Review
- CSS Design Review
- GitHub CI/CD Review

## Process Constraints
- Each skill must be applied only to code that falls within its domain; do not apply CSS review criteria to PHP logic or PHP criteria to GitHub Actions workflows
- Review must distinguish between design problems and style preferences; only design problems are blocking
- Must not require changes that contradict established architectural decisions already in scope for the submission
- Security and accessibility findings are always blocking regardless of submission scope
- Must evaluate the submission as a unit, not individual lines in isolation; a locally suspicious pattern may be intentional when viewed in context

## Output Contract

The review must contain:

1. **Submission Summary**: What the submission does, which domains it touches, and the scope of review applied
2. **Domain Findings**: A section per applicable domain (PHP, Twig, Symfony, HTML5, API Platform, CSS, GitHub CI/CD), each containing:
   - Blocking issues (design violations, security issues, accessibility failures)
   - Non-blocking observations (minor quality concerns or improvement suggestions)
   - Explicit confirmation when the domain is clean (no findings)
3. **Cross-domain Concerns**: Issues that span multiple domains (e.g., a Symfony controller leaking presentation logic into Twig, or an API Platform operation missing corresponding CSS state feedback)
4. **Verdict**: One of — `Approved`, `Approved with suggestions`, or `Changes requested` — with explicit rationale referencing specific findings
5. **Prioritized Action List**: Ordered list of required changes (blocking) and optional improvements (non-blocking), each traceable to a specific finding

Format: Structured document, one section per domain, 800–2,000 words depending on submission size.

## Quality Checks

Before delivery, validate:
- [ ] Every blocking finding cites the specific code location and the design principle it violates
- [ ] Non-blocking observations are clearly distinguished from blocking issues
- [ ] Accessibility findings reference WCAG 2.1 AA criteria or ARIA specification explicitly
- [ ] Security-related findings (CI/CD token scopes, SQL injection risk, etc.) are always classified as blocking
- [ ] Each domain section is present if the submission touches that domain, even if the finding is "no issues found"
- [ ] Cross-domain concerns are not duplicated within individual domain sections
- [ ] Verdict is consistent with the set of blocking findings (no blocking findings → cannot be "Changes requested")
- [ ] Action list items are actionable (verb + specific change required), not vague suggestions
