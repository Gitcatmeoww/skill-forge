---
name: skill-forge
description: Research and analyze existing Claude skills before building new ones. Triggers when users want to create, compare, or audit skills — not for installing or managing existing ones.
---

# Skill Forge

Build better skills by learning from what already exists — without blindly copying.

Skill Forge sits upstream of skill creation. Before writing a single line of SKILL.md, it researches the ecosystem, analyzes what's out there, identifies patterns that work (and ones that don't), flags security concerns, and then helps create something genuinely better — informed by existing work but original in design.

## When to use Skill Forge vs. Skill Creator

- **Skill Forge** → "I want to build a skill for X. What's already out there? How can I do it better?"
- **Skill Creator** → "I have a clear idea, just help me write and iterate on it."

They complement each other. Skill Forge handles the research and synthesis phase, then hands off to Skill Creator's eval/iterate loop for polishing.

---

## The Forge Workflow

### Phase 1: Understand the Intent

Before searching, get clarity on what the user actually needs. You need to understand:

1. What problem does this skill solve? (not "what should it do" — the underlying problem)
2. Who is it for? (personal use, team, open-source community)
3. What environment? (Claude Code, Claude.ai, Cowork, cross-platform)
4. Any must-have features or constraints?

If the user's initial message already covers some of these, don't re-ask — acknowledge what you know and only ask about gaps. Keep this conversational.

**Calibrate depth:** Based on the user's request, decide the workflow depth:
- **Quick scan** — User wants a fast overview ("what's out there for X?"). Search broadly, produce a brief summary of findings, skip the full comparative matrix, and go straight to synthesis. Phases 3-4 are lightweight.
- **Full forge** — User wants thorough research before building ("I'm building a production skill for X, help me do it right"). Run the complete workflow with deep analysis on each discovered skill.

### Phase 2: Ecosystem Discovery

Search for existing skills that overlap with the user's intent. Cast a wide net.

**Where to search (and how):**

1. **GitHub** — The primary source. Use `gh search repos` or web search to find repos with `SKILL.md` files, or repos tagged with `claude-skill`, `agent-skill`, etc.
   ```
   Search queries to try:
   - "[domain] SKILL.md" (e.g., "code-review SKILL.md")
   - "[domain] claude skill"
   - "[domain] agent skill"
   - "awesome-claude-skills [domain]"
   ```
   Once you find repos, fetch their `SKILL.md` files directly via raw GitHub URLs.

2. **Skill directories** — Use web search to check aggregators like SkillsMP (skillsmp.com) and LobeHub (lobehub.com/skills) for existing skills in the domain. Fetch and read any promising results.

3. **Plugin marketplaces** — Check if there are Claude Code plugins covering the same domain (e.g., `phuryn/pm-skills` for product management).

**How many to find:** Aim for 3-8 relevant skills. More than that and the analysis becomes noise. If you find fewer than 3, broaden the search terms. If you find more than 8, filter by relevance and quality signals (stars, recency, documentation quality).

**If you find almost nothing:** Some domains are genuinely novel. If after broadening you still find fewer than 2 relevant skills, shift to analyzing adjacent domains — skills that solve related problems or use techniques that could transfer. Note to the user that this is a greenfield opportunity and skip ahead to Phase 5 with recommendations drawn from adjacent-domain patterns rather than direct comparisons.

**What to capture for each skill:**

```
For each discovered skill, record:
- Name and source URL
- Brief description (what it claims to do)
- Star count / popularity signals
- Last updated date
- SKILL.md structure (frontmatter, sections, length)
- Whether it bundles scripts, references, or assets
- Key patterns and techniques used
- Any obvious gaps or issues
```

Save findings to `references/discovery-report.md` using the template in `references/discovery-template.md`. See `references/example-discovery-report.md` for an example of the expected depth and quality.

### Phase 3: Deep Analysis

This is where Skill Forge earns its name. Don't just list what you found — analyze it.

**Triage first:** Do a quick surface scan of each skill. If a skill is clearly simple (no scripts, no API calls, short SKILL.md), note it as low-complexity and keep the analysis brief. Focus the deep analysis on the more substantial skills — ones with bundled scripts, external integrations, or novel techniques.

For each discovered skill, evaluate across these dimensions:

#### 3a. Structural Analysis
- How is the SKILL.md organized? Is it clear and scannable?
- Does it follow progressive disclosure (metadata → instructions → bundled resources)?
- Is it too long? Too short? The sweet spot is under 500 lines for SKILL.md.
- Does the description trigger well? (Would Claude actually invoke this when it should?)

#### 3b. Technique Analysis
- What patterns does the skill use? (Templates, scripts, examples, decision trees, etc.)
- Does it explain the "why" behind instructions, or just bark orders?
- How does it handle edge cases?
- Does it leverage bundled scripts for deterministic/repetitive tasks?

#### 3c. Security Audit

This is critical and often overlooked in community skills. Run the full audit using the checklist in `references/security-checklist.md`.

Flag any issues clearly. Use severity levels:
- **Critical** — Could compromise system security (e.g., `curl [url] | bash` patterns, unvalidated eval/exec)
- **Warning** — Potentially risky, depends on context (e.g., broad filesystem access, optional API calls)
- **Info** — Not a risk, but worth noting (e.g., installs known packages, writes to temp dirs)

#### 3d. Effectiveness Assessment
- Does this skill actually improve on Claude doing the task without a skill?
- Where does it add the most value?
- Where does it over-constrain or under-deliver?

### Phase 4: Comparative Synthesis

Now bring it all together. Create a comparison that highlights:

1. **Common patterns** — What do all the good skills in this space do similarly? These are likely important.
2. **Differentiators** — Where do they diverge? Why might one approach be better than another?
3. **Gaps** — What does NO existing skill handle well? This is where the user's new skill can add the most value.
4. **Anti-patterns** — What should the new skill actively avoid?

Present this as a concise briefing to the user. Don't dump raw notes — synthesize. A format like this works well:

```
## Ecosystem Briefing: [Domain] Skills

### What exists (X skills found)
Brief overview of the landscape.

### What works well
- Pattern A (used by skills X, Y) — why it works
- Pattern B (used by skill Z) — why it works

### What doesn't work
- Anti-pattern A — why it fails
- Anti-pattern B — why it fails

### Security concerns found
- Issue 1 in Skill X (severity)
- Issue 2 in Skill Y (severity)

### Opportunity for your skill
The gap your skill can fill, informed by the above.
```

### Phase 5: Informed Creation

Present the user with concrete recommendations for their new skill, grounded in your research:

1. **Patterns to adopt** — Which techniques from existing skills should inform the design, and why they work
2. **Anti-patterns to avoid** — What to actively steer away from, based on what you saw fail
3. **Gaps to fill** — The specific opportunities where the new skill can differentiate
4. **Security posture** — What security considerations apply, informed by issues found in Phase 3

Then help the user create the skill. The key principle: **learn from existing work, don't copy it.** Understand WHY patterns work, then implement the principles in an original way.

If the user has access to `skill-creator`, hand off to its eval/iterate loop for drafting, testing, and polishing. If not, draft the SKILL.md directly and iterate with the user in conversation.

---

## Example Walkthrough

Here's a condensed example of Skill Forge in action:

**User:** "I want to build a skill for database migration reviews"

**Phase 1 — Understand Intent:**
> "A few quick questions before I research:
> 1. What's the core problem — catching risky migrations before they hit prod, or generating migrations from schema changes?
> 2. Is this for a specific database (Postgres, MySQL) or database-agnostic?
> 3. Personal use or something you'd share with a team?"
>
> User answers — this sounds like a production team tool. **Full forge** it is.

**Phase 2 — Discovery:**
Search GitHub for "database migration SKILL.md", "sql review claude skill", "migration safety" — find 4 relevant skills. Fetch and read each SKILL.md.

**Phase 3 — Analysis:**
Evaluate each skill's structure, techniques, and security. Notice that 2 of 4 execute raw SQL without input validation (Critical), one has excellent pattern-matching for dangerous operations (ALTER TABLE, DROP) but no rollback guidance.

**Phase 4 — Briefing:**
> "Found 4 skills. The good ones use regex pattern-matching to flag dangerous operations and require explicit user confirmation. Common gap: none handle data migrations (backfills, transforms) — they only cover schema changes. Security concern: two skills pipe migration files directly to database CLIs without sanitization."

**Phase 5 — Informed Creation:**
Recommend the new skill adopt the pattern-matching approach, expand scope to cover data migrations, and add a mandatory dry-run step before any destructive operation. Hand off to skill-creator or draft directly.

---

## Environment-Specific Notes

### Claude Code
- Full power: can fetch repos, read SKILL.md files directly from GitHub raw URLs, run analysis scripts
- Use `gh search repos`, `WebSearch`, and `WebFetch` for discovery
- For efficiency, run discovery searches in parallel and use subagents for analyzing multiple skills simultaneously during Phase 3
- Can integrate with `skill-creator` for the eval loop after Phase 5

### Claude.ai
- Discovery relies on web search (can't clone repos or use CLI tools)
- Analysis is based on what's visible in search results and web-fetched pages
- Still highly valuable — the synthesis and informed creation phases work the same way

### Cowork
- Similar to Claude Code capabilities
- Can check built-in skills at `/mnt/skills/public/` and `/mnt/skills/examples/` before searching externally
- Can spawn subagents for parallel discovery across multiple sources

---

## Important Principles

**Don't be a skill aggregator.** The goal is NOT to find the "best existing skill" and tell the user to install it. The goal is to understand the landscape and create something better.

**Don't blindly copy.** If Skill X has a great pattern, understand WHY it works, then implement the principle in your own way. Direct copying of substantial portions of another skill's instructions is not what Skill Forge does.

**Security is not optional.** Every skill that touches the filesystem, runs scripts, or calls APIs should have its security posture evaluated. This is a genuine value-add over just installing random skills from GitHub.

**Respect the user's time.** Use the depth calibration from Phase 1. Don't default to a full forge when a quick scan would suffice.

**Explain your reasoning.** When you recommend a pattern or flag a concern, explain why. The user should understand the trade-offs and be able to make informed decisions.
