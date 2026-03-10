---
name: translate-tech
description: Converts technical content into clear explanations for audiences without domain expertise while maintaining accuracy
license: CC-BY-4.0
compatibility: claude-dev,opencode
metadata:
  version: "1.0"
  category: "translation"
  type: "workflow"
---

# Translate Technical Content to Non-Technical Explanation

## Name

Translate Technical Content to Non-Technical Explanation

## Purpose

Converts specialized or technical content into clear explanations for audiences without domain expertise. Applied when technical decisions, capabilities, or findings must be communicated to stakeholders, leadership, or external parties who lack the technical background to interpret the original material.

## Inputs

### Required

- Technical content (article, documentation, code explanation, system design, research finding)
- Target audience profile (role, domain, level of technical background)
- Context on why this content matters to the audience

### Optional

- Specific concepts that must be emphasized or de-emphasized
- Prior knowledge the audience is assumed to have
- Use cases or applications most relevant to audience

## Referenced Skills

- Translate Technical Concepts to Non-Technical Audiences
- Jip-en-Janneke Explanation Mode

## Process Constraints

- Must not oversimplify to the point of inaccuracy; technical correctness is non-negotiable
- Must use domain analogies familiar to the target audience, not generic metaphors
- Must preserve uncertainty where it exists in original content (no false confidence)
- Must identify what the audience can act on or decide based on the explanation
- Avoid jargon entirely unless unavoidable, then define explicitly inline

## Output Contract

The explanation must contain:

1. **Hook**: Single sentence explaining why this matters to this audience
2. **Core Concept**: Explanation of the fundamental idea in plain language (2-3 paragraphs maximum)
3. **How It Works**: Step-by-step walkthrough using audience-relevant examples or analogies
4. **Key Implications**: What this means for audience decisions, risks, or opportunities
5. **Limitations or Caveats**: What this explanation cannot predict or guarantee
6. **If You Want to Learn More**: Pointers to technical resources for those who want depth

Format: Narrative or visual explanation, 500-1,500 words depending on complexity.

## Quality Checks

Before delivery, validate:

- [ ] Hook is specific to the target audience (not generic)
- [ ] Core concept uses analogies or comparisons familiar to the audience's domain
- [ ] Zero unexplained jargon; every technical term is defined when first used
- [ ] Examples are drawn from audience's context or concerns, not the original content's domain
- [ ] Uncertainties in original content are preserved ("might", "could", "if")
- [ ] Explanation connects back to what the audience needs to decide or understand
- [ ] "Learn more" pointers are appropriate to audience level (not introductory tutorials for executives; not oversimplified for engineers)
- [ ] Accuracy validated against original source (no essential information lost or distorted)
