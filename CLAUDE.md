# Skill Forge — Project Guide

This is a Claude skill project. The main deliverable is `SKILL.md` — the instructions Claude follows when the skill is activated.

## Project Structure

- `SKILL.md` — The skill itself. All instruction logic lives here.
- `references/discovery-template.md` — Blank template for discovery reports.
- `references/example-discovery-report.md` — Filled-out example showing expected output quality.
- `references/security-checklist.md` — Security audit checklist used during Phase 3.

## Testing Changes

1. Symlink the project into your skills directory: `ln -s "$(pwd)" ~/.claude/skills/skill-forge`
2. Start a new Claude Code session.
3. Try a trigger phrase from the list below to verify the skill activates.
4. Walk through at least Phases 1-3 to verify your changes work end-to-end.

## Trigger Test Cases

Changes to the frontmatter `description` in SKILL.md should be validated against these cases.

**Should trigger:**
- "I want to build a skill for code review"
- "What skills exist for linting?"
- "Help me make a better version of this deployment skill"
- "Are there any good skills for database migrations?"
- "Audit this skill for security issues"

**Should NOT trigger:**
- "Install the code-review skill"
- "How do I manage my installed skills?"
- "Write me a SKILL.md for a linter" (this is skill-creator territory — user already knows what to build)
- "Remove the deploy skill from my project"
- "List my current skills"
