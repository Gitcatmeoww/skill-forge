---
name: skill-forge
description: Discovers, evaluates, and installs existing agent skills, or researches the ecosystem to build better new ones. Triggers when users want to find, compare, audit, or create skills.
---

# Skill Forge

Find the right skill — or build a better one.

Skill Forge helps you discover existing skills in the open agent skills ecosystem, evaluate their quality, and install them. When no existing skill fits, it shifts into research mode — analyzing what's out there, identifying patterns and gaps, and helping you create something genuinely better.

## When to use Skill Forge

- **Find path** → "Find a skill for X", "What skills exist for X?", "Is there a skill for code review?"
- **Forge path** → "I want to build a skill for X. What's already out there?", "Help me make a better version of this skill", "Audit this skill for security issues"

They're two sides of the same coin. Finding often leads to forging when nothing quite fits.

---

## Phase 1: Understand Intent

Before searching, get clarity on what the user needs and which path to take.

### Gather context

1. What problem does this skill solve? (the underlying problem, not "what should it do")
2. Who is it for? (personal use, team, open-source community)
3. What environment? (Claude Code, Claude.ai, Cowork, cross-platform)
4. Any must-have features or constraints?

If the user's initial message already covers some of these, don't re-ask — acknowledge what you know and only ask about gaps. Keep this conversational.

### Route: Find or Forge?

Based on the user's intent, pick a path:

- **Find path** — User wants to discover and use an existing skill. They said things like "find", "is there", "what exists", "recommend". Goal: find the best existing skill, verify quality, install it.
- **Forge path** — User wants to build a new skill informed by research. They said things like "build", "create", "make a better version", "audit". Goal: research the landscape, then create something better.

If ambiguous, default to the find path. You can always escalate to forge if nothing good exists or the user wants more.

---

## Phase 2: Find Skills

Search for existing skills that match the user's intent. This phase is shared by both paths.

### Primary: Skills CLI

The Skills CLI (`npx skills`) is the package manager for the open agent skills ecosystem.

**Step 1 — Check the leaderboard.** Before running a CLI search, check the [skills.sh leaderboard](https://skills.sh/) to see if a well-known skill already exists for the domain. The leaderboard ranks skills by total installs, surfacing the most popular and battle-tested options.

**Step 2 — Search by keyword.** Run targeted searches:

```bash
npx skills find [query]
```

Try multiple queries to cast a wide net:
- Domain-specific: `npx skills find react performance`
- Task-specific: `npx skills find pr review`
- Alternative terms: if "deploy" returns little, try "deployment" or "ci-cd"

**Step 3 — Verify quality.** Do not recommend a skill based solely on search results. Always check:

1. **Install count** — Prefer skills with 1K+ installs. Be cautious with anything under 100.
2. **Source reputation** — Official sources (`vercel-labs`, `anthropics`, `microsoft`) are more trustworthy than unknown authors.
3. **GitHub stars** — Check the source repository via `gh api repos/{owner}/{repo} --jq '{stars: .stargazers_count, updated: .updated_at, description: .description}'`. A skill from a repo with <100 stars warrants extra scrutiny.
4. **Recency** — When was it last updated? Stale skills may not work with current tooling.

### Fallback: GitHub search

If the CLI returns fewer than 3 relevant results, broaden with manual search:

1. **GitHub** — Use `gh search repos` with `--sort stars --json fullName,description,stargazersCount,updatedAt` to get structured data.
   ```
   Search queries to try:
   - "[domain] SKILL.md" (e.g., "code-review SKILL.md")
   - "[domain] claude skill"
   - "[domain] agent skill"
   - "awesome-claude-skills [domain]"
   ```
   Once you find repos, fetch their `SKILL.md` files via `raw.githubusercontent.com/{owner}/{repo}/{branch}/SKILL.md` — never fetch the GitHub HTML page.

2. **Web search** — Search for skill directories, aggregators, or blog posts covering skills in the domain.

### Quality gate

Filter out repos with <=10 stars. Stars are a reasonable proxy for quality and community validation. Don't waste tokens fetching and analyzing low-signal repos. Exceptions: very new repos (<2 weeks old) in a niche domain where few options exist.

### How many to find

Aim for 3-8 relevant skills after filtering. If fewer than 3 pass the quality gate, broaden search terms or lower the threshold. If more than 8, keep the top 8 by stars/installs.

**If you find almost nothing:** Some domains are genuinely novel. If after broadening you still find fewer than 2 relevant skills, shift to analyzing adjacent domains — skills that solve related problems or use techniques that could transfer. Note to the user that this is a greenfield opportunity.

### What to capture for each skill

```
For each discovered skill, record:
- Name and source URL
- Brief description (what it claims to do)
- Install count (from skills.sh, if available)
- Star count (numeric — use gh api or gh search repos; "moderate" or "low" are not acceptable)
- Source/author reputation tier (official, well-known, unknown)
- Last updated date
- SKILL.md structure (frontmatter, sections, length)
- Whether it bundles scripts, references, or assets
- Key patterns and techniques used
- Any obvious gaps or issues
```

For the forge path, save findings to `./discovery-report.md` in the user's current working directory, using the template structure from `references/discovery-template.md`. See `references/example-discovery-report.md` for expected depth. The report is a deliverable for the user's project — do not write it into the skill-forge skill folder.

### Fetching content efficiently

- **SKILL.md / README files:** Always use `raw.githubusercontent.com/{owner}/{repo}/{branch}/SKILL.md` — never fetch the GitHub HTML page.
- **Repo metadata:** Use `gh api repos/{owner}/{repo} --jq '{stars: .stargazers_count, updated: .updated_at, description: .description}'` — don't scrape HTML.
- **Blog posts / articles:** Prefer web search snippets for context. Only fetch if you need specific technical content. Avoid fetching pages >50KB unless essential.
- **Never fetch full GitHub repo HTML pages** — they're 200-700KB of navigation chrome with little useful content.

---

## Phase 3 — Find Path: Present & Install

If the user is on the find path, present the best options and help them install.

### Present options

For each recommended skill, show:

1. **Name** and brief description of what it does
2. **Install count** and source
3. **Quality assessment** — a quick summary of strengths and any concerns (not a full security audit, but flag anything obvious)
4. **Install command**

Example response format:

```
## Found: react-best-practices

React and Next.js performance optimization guidelines from Vercel Engineering.
185K installs | Source: vercel-labs/agent-skills

Strengths: Well-maintained, comprehensive, from the React/Next.js team.
Concerns: None — battle-tested skill from an official source.

Install:
npx skills add vercel-labs/agent-skills@react-best-practices -g -y
```

If multiple options exist, present the top 2-3 with a brief comparison so the user can choose.

### Offer to install

If the user wants to proceed:

```bash
npx skills add <owner/repo@skill> -g -y
```

The `-g` flag installs globally (user-level) and `-y` skips confirmation prompts.

### Offer to escalate

After presenting options, always offer:

> "Want me to do a deeper analysis of any of these? Or if none fit, I can research the space and help you build a custom one."

This is the bridge from find to forge.

---

## Phase 3 — Forge Path: Deep Analysis

If the user is on the forge path, this is where Skill Forge earns its name. Don't just list what you found — analyze it.

**Triage first:** Do a quick surface scan of each skill. If a skill is clearly simple (no scripts, no API calls, short SKILL.md), note it as low-complexity and keep the analysis brief. Focus the deep analysis on the more substantial skills — ones with bundled scripts, external integrations, or novel techniques.

For each discovered skill, evaluate across these dimensions:

### 3a. Structural Analysis
- How is the SKILL.md organized? Is it clear and scannable?
- Does it follow progressive disclosure (metadata -> instructions -> bundled resources)?
- Is it too long? Too short? The sweet spot is under 500 lines for SKILL.md.
- Does the description trigger well? (Would Claude actually invoke this when it should?)

### 3b. Technique Analysis
- What patterns does the skill use? (Templates, scripts, examples, decision trees, etc.)
- Does it explain the "why" behind instructions, or just bark orders?
- How does it handle edge cases?
- Does it leverage bundled scripts for deterministic/repetitive tasks?

### 3c. Security Audit

This is critical and often overlooked in community skills. Run the full audit using the checklist in `references/security-checklist.md`.

Flag any issues clearly. Use severity levels:
- **Critical** — Could compromise system security (e.g., `curl [url] | bash` patterns, unvalidated eval/exec)
- **Warning** — Potentially risky, depends on context (e.g., broad filesystem access, optional API calls)
- **Info** — Not a risk, but worth noting (e.g., installs known packages, writes to temp dirs)

### 3d. Effectiveness Assessment
- Does this skill actually improve on Claude doing the task without a skill?
- Where does it add the most value?
- Where does it over-constrain or under-deliver?

### 3e. Ecosystem Standing
- How does install count and popularity compare to alternatives?
- Is the source reputable and actively maintained?
- Does the community trust this skill? (stars, forks, issues activity)

---

## Phase 4 — Forge Path: Comparative Synthesis

Bring it all together. **Order skills by star count / install count (descending)** throughout — in the discovery report, comparative matrix, and briefing.

Create a comparison that highlights:

1. **Common patterns** — What do all the good skills in this space do similarly? These are likely important.
2. **Differentiators** — Where do they diverge? Why might one approach be better than another?
3. **Gaps** — What does NO existing skill handle well? This is where the user's new skill can add the most value.
4. **Anti-patterns** — What should the new skill actively avoid?

Present this as a concise briefing. Don't dump raw notes — synthesize:

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

---

## Phase 5 — Forge Path: Informed Creation

Present the user with concrete recommendations for their new skill, grounded in your research:

1. **Patterns to adopt** — Which techniques from existing skills should inform the design, and why they work
2. **Anti-patterns to avoid** — What to actively steer away from, based on what you saw fail
3. **Gaps to fill** — The specific opportunities where the new skill can differentiate
4. **Security posture** — What security considerations apply, informed by issues found in Phase 3

Then help the user create the skill. The key principle: **learn from existing work, don't copy it.** Understand WHY patterns work, then implement the principles in an original way.

If the user has access to `skill-creator`, hand off to its eval/iterate loop for drafting, testing, and polishing. If not, draft the SKILL.md directly and iterate with the user in conversation.

**Publishing:** If the user wants to share their skill with the ecosystem, they can scaffold it with:

```bash
npx skills init my-skill-name
```

This creates the standard skill structure ready for publishing.

---

## Example Walkthroughs

### Find path example

**User:** "Is there a skill for PR reviews?"

**Phase 1 — Route:** This is a find — the user wants to discover an existing skill.

**Phase 2 — Find:**
> Check skills.sh leaderboard for popular code review skills. Run `npx skills find pr review`. Find 3 relevant skills with strong install counts.

**Phase 3 — Present & Install:**
> "Found 3 skills for PR reviews:
>
> 1. **pr-reviewer** (45K installs, vercel-labs) — Automated PR review with best practices enforcement. Well-maintained, strong reputation.
> 2. **code-review-checklist** (12K installs, community) — Checklist-based review workflow. Simpler but focused.
> 3. **review-bot** (800 installs, unknown author) — AI-driven review suggestions. Lower install count, less vetted.
>
> I'd recommend #1 for production use. Want me to install it, or should I dig deeper into any of these?"

### Forge path example

**User:** "I want to build a skill for database migration reviews"

**Phase 1 — Route:**
> "A few quick questions before I research:
> 1. What's the core problem — catching risky migrations before they hit prod, or generating migrations from schema changes?
> 2. Is this for a specific database (Postgres, MySQL) or database-agnostic?
> 3. Personal use or something you'd share with a team?"
>
> User answers — this is a production team tool. **Forge path** it is.

**Phase 2 — Find:**
> Run `npx skills find database migration`. Check skills.sh. Fall back to GitHub search with "database migration SKILL.md". Find 4 relevant skills with varying quality.

**Phase 3 — Deep Analysis:**
> Evaluate each skill's structure, techniques, and security. Notice that 2 of 4 execute raw SQL without input validation (Critical), one has excellent pattern-matching for dangerous operations (ALTER TABLE, DROP) but no rollback guidance.

**Phase 4 — Briefing:**
> "Found 4 skills. The good ones use regex pattern-matching to flag dangerous operations and require explicit user confirmation. Common gap: none handle data migrations (backfills, transforms) — they only cover schema changes. Security concern: two skills pipe migration files directly to database CLIs without sanitization."

**Phase 5 — Informed Creation:**
> Recommend the new skill adopt the pattern-matching approach, expand scope to cover data migrations, and add a mandatory dry-run step before any destructive operation. Hand off to skill-creator or draft directly.

---

## Environment-Specific Notes

### Claude Code
- Full power: can run `npx skills find` and `npx skills add` directly, fetch repos, read SKILL.md files from GitHub
- **Phase 2 (Find):** Run `npx skills find`, `gh search repos`, `WebSearch`, and `WebFetch` directly — do NOT delegate initial discovery to subagents (they often fail on permissions, wasting tokens)
- **Phase 3 (Deep Analysis):** Use subagents here if analyzing 3+ skills in parallel — this is where agents add real value
- Use `raw.githubusercontent.com` for file content, `gh api` for metadata — avoid fetching full GitHub HTML pages
- Can integrate with `skill-creator` for the eval loop after Phase 5

### Claude.ai
- Cannot run CLI commands — `npx skills` is not available
- Discovery relies on web search and browsing [skills.sh](https://skills.sh/) directly
- Analysis is based on what's visible in search results and web-fetched pages
- Still highly valuable — the synthesis and informed creation phases work the same way

### Cowork
- Similar to Claude Code capabilities — can run `npx skills` CLI
- Can check built-in skills at `/mnt/skills/public/` and `/mnt/skills/examples/` before searching externally
- Can spawn subagents for parallel discovery across multiple sources

---

## Important Principles

**Verify before recommending.** Never recommend a skill based solely on search results or name recognition. Check install count, source reputation, and recency. A quick quality check prevents installing broken or malicious skills.

**Don't be a skill aggregator.** The goal of the forge path is NOT to find the "best existing skill" and tell the user to install it. The goal is to understand the landscape and create something better.

**Don't blindly copy.** If Skill X has a great pattern, understand WHY it works, then implement the principle in your own way. Direct copying of substantial portions of another skill's instructions is not what Skill Forge does.

**Security is not optional.** Every skill that touches the filesystem, runs scripts, or calls APIs should have its security posture evaluated. This is a genuine value-add over just installing random skills from GitHub.

**Respect the user's time.** Use the path routing from Phase 1. Don't default to a full forge when a quick find would suffice. The find path exists for a reason.

**Explain your reasoning.** When you recommend a pattern or flag a concern, explain why. The user should understand the trade-offs and be able to make informed decisions.
