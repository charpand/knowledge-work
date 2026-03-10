# Skills Setup for GitHub Copilot & OpenCode (Knowledge-Work)

This repository includes knowledge-work skills compatible with both **GitHub Copilot** and **OpenCode** using a single source of truth with symlinks.

## Structure

- **`skills/`** - Single source of truth containing all 8 SKILL.md files
- **`.claude/skills/`** - Symlinks to `skills/` for GitHub Copilot discovery
- **`.opencode/skills/`** - Symlinks to `skills/` for OpenCode discovery

Each skill is a directory with a `SKILL.md` file containing YAML frontmatter and markdown content.

## Available Skills (8 total)

**Advisory & Standards** (2 skills)
conventional-commits, go-style

**Governance** (1 skill)
participation-evaluation

**Review Workflows** (3 skills)
review-engineering, review-policy, review-supply-chain

**Meetings & Translation** (2 skills)
summarize-meeting, translate-tech

## How It Works

Both Copilot and OpenCode discover skills by looking in their respective directories:

- Copilot finds `.claude/skills/<name>/SKILL.md`
- OpenCode finds `.opencode/skills/<name>/SKILL.md`

Both point to the same files via symlinks, eliminating duplication.

## Skill Format

```yaml
---
name: skill-name
description: Brief description of the skill's purpose
license: CC-BY-4.0
compatibility: claude-dev,opencode
metadata:
  version: "1.0"
  category: "advisory|governance|review|meetings|translation"
  type: "workflow|standard|advisory"
---
```

## Key Difference from ai-skills

**Knowledge-work skills are operational workflows, not reasoning frameworks:**

- **ai-skills**: Teaching how to think about technical problems (Mental Models, Thinking Steps, Quality Bars)
- **knowledge-work**: Reusable processes and procedures for knowledge work tasks (Review frameworks, advisory templates, meeting summaries)

Knowledge-work skills are discoverable alongside ai-skills for a unified agent skill ecosystem, but they serve a complementary purpose—operational execution rather than reasoning discipline.

## Distinction in Frontmatter

Knowledge-work skills use `type: "workflow"` or `type: "standard"` to indicate their operational nature, while ai-skills use the seven-section structure (Mental Model, Thinking Steps, etc.).

## Creating New Skills

Each skill MUST contain structured sections specific to its workflow type. See individual skill files for reference structure.

All skills must be agent-agnostic, organization-reusable, and time-stable. Ensure:

- Frontmatter is complete and accurate
- Sections are clear and actionable
- No jargon without explanation
- Quality checks are verifiable
