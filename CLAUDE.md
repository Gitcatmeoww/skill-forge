# Skill Forge — Project Guide

This is a Claude skill project. The main deliverable is `SKILL.md` — the instructions Claude follows when the skill is activated.

## Project Structure

- `SKILL.md` — The skill itself. All instruction logic lives here.
- `references/discovery-template.md` — Blank template for discovery reports.
- `references/example-discovery-report.md` — Filled-out example showing expected output quality.
- `references/security-checklist.md` — Security audit checklist used during Phase 3 (forge path).

## Testing Changes

1. Symlink the project into your skills directory: `ln -s "$(pwd)" ~/.claude/skills/skill-forge`
2. Start a new Claude Code session.
3. Try a trigger phrase from the list below to verify the skill activates.
4. Walk through at least Phases 1-2 to verify discovery works, then test both the find path (Phase 3 find) and forge path (Phase 3 forge) at least once.

## Trigger Test Cases

Changes to the frontmatter `description` in SKILL.md should be validated against these cases.

**Should trigger (find path):**
- "Find me a skill for linting"
- "Is there a skill for database migrations?"
- "What skills exist for code review?"
- "Recommend a skill for testing"

**Should trigger (forge path):**
- "I want to build a skill for code review"
- "Help me make a better version of this deployment skill"
- "Are there any good skills for database migrations? How do they compare?"
- "Audit this skill for security issues"

**Should NOT trigger:**
- "How do I write a for loop?" (too general — not skill-related)
- "How do I manage my installed skills?" (skill management, not discovery/creation)
- "Write me a SKILL.md for a linter" (skill-creator territory — user already knows what to build)
- "Remove the deploy skill from my project" (skill management)
- "List my current skills" (skill management)
