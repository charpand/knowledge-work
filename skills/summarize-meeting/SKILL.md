---
name: summarize-meeting
description: Transforms meeting notes into structured decisions, commitments, and action points clarifying accountability and preventing scope creep
license: CC-BY-4.0
compatibility: claude-dev,opencode
metadata:
  version: "1.0"
  category: "meetings"
  type: "workflow"
---

# Summarize Meeting into Decisions and Action Points

## Name

Summarize Meeting into Decisions and Action Points

## Purpose

Transforms raw meeting notes or recordings into a structured record of decisions made, commitments accepted, and follow-up work required. Provides clarity on what was decided versus what was discussed, clarifying accountability and preventing post-meeting confusion or scope creep.

## Inputs

### Required

- Meeting notes (transcript, recording transcript, or written notes)
- Meeting context (participants, stated objectives, date)
- Stakeholder who will receive the summary

### Optional

- Prior decisions or commitments referenced during meeting
- Decision-making framework or approval authority used
- Known constraints or precedent

## Referenced Skills

- Define and Enforce a Quality Bar

## Process Constraints

- Must distinguish between decisions (commitments to act or change) and discussions (exploration without commitment)
- Must assign explicit ownership to each action (person/team, not "someone")
- Must not interpret or infer decisions not explicitly stated; flag ambiguities
- Must use participant language when possible to avoid misrepresentation
- Timeline must be specified for each action (date, quarter, or event-based)

## Output Contract

The summary must contain:

1. **Meeting Header**: Date, participants, stated objectives
2. **Decisions Made** (prioritized by urgency/impact):
   - Decision statement (clear, outcome-focused)
   - Rationale briefly summarized
   - Approval authority if applicable

3. **Commitments and Action Items**:
   - Owner (person/team name, not generic)
   - Action description (verb + outcome)
   - Due date or trigger
   - Success criteria (how we'll know it's done)
   - Dependencies (what must happen first, if any)

4. **Parking Lot / Follow-ups**: Items discussed but not decided; flagged for later processing
5. **Open Questions or Ambiguities**: Decisions referenced but not clearly defined in the meeting; require clarification
6. **Next Meeting**: Date, participants, preliminary agenda (if applicable)

Format: Structured document, 300-800 words depending on meeting complexity.

## Quality Checks

Before delivery, validate:

- [ ] Every decision is stated as outcome, not discussion ("We will adopt X by Q2" not "We discussed X")
- [ ] Every action has an explicit owner (name or team role), not "someone" or "TBD"
- [ ] Timelines are specific (date, quarter, or event-based), not vague ("soon")
- [ ] Success criteria are measurable or verifiable
- [ ] Discussions that did not result in decisions are not listed as decisions
- [ ] Ambiguities or deferred decisions are clearly flagged as such
- [ ] Participant names/roles are accurate and attributions are traceable
- [ ] No interpretation or inference beyond what was explicitly stated (flag inferred items separately)
