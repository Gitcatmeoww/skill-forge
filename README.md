# Skill Forge

Find the right skill — or build a better one. Discovers, evaluates, and installs existing agent skills, or researches the ecosystem to create something genuinely better.

## Demo

<!-- TODO: Replace with actual recording -->
<!-- Record with: "Find me a skill for code review" (find path) then "I want to build a skill for code review" (forge path) -->
<!-- Recommended: asciinema or screen recording, ~30-60 seconds -->

![Skill Forge Demo](assets/demo.gif)

## The Problem

Today, finding the right agent skill means sifting through thousands of GitHub repos with no quality signals. And creating a skill means building from scratch with no awareness of what already exists. Skill Forge fills both gaps — it **finds and evaluates** existing skills with quality verification, and when nothing fits, it **researches and synthesizes** the landscape to help you build something better.

## What It Does

Skill Forge has two paths depending on your intent:

### Find Path — "What's out there?"

When you say something like _"Find me a skill for PR reviews"_, Skill Forge:

1. **Searches** the skills ecosystem via `npx skills find` and the skills.sh leaderboard
2. **Verifies quality** — install count, source reputation, GitHub stars, recency
3. **Presents** the best options with quality assessments
4. **Installs** your choice via `npx skills add`

### Forge Path — "Help me build something better"

When you say something like _"I want to build a skill for code review"_, Skill Forge:

1. **Discovers** existing skills across the ecosystem (CLI + GitHub fallback)
2. **Analyzes** each one — structure, techniques, security posture, effectiveness
3. **Synthesizes** findings into a clear briefing: what works, what doesn't, where the gaps are
4. **Creates** a new skill informed by that research, or hands off to your preferred creation workflow

## Installation

### Claude Code

Clone this repo and add it to your skills directory:

```bash
# Clone the repo
git clone https://github.com/Gitcatmeoww/skill-forge.git

# Option 1: Symlink into your global skills
ln -s "$(pwd)/skill-forge" ~/.claude/skills/skill-forge

# Option 2: Symlink into a project's skills
ln -s "$(pwd)/skill-forge" .claude/skills/skill-forge
```

### Verify Installation

Start a new Claude Code session and try:
- _"Find me a skill for code review"_ — should search the ecosystem and present options
- _"I want to build a skill for code review"_ — should begin the forge research workflow

### Claude.ai

Copy the contents of `SKILL.md` into your project's custom instructions or system prompt. Discovery will use web search and skills.sh browsing instead of CLI tools, but the analysis and creation phases work the same way.

## Usage

### Find path triggers

- _"Find me a skill for X"_
- _"Is there a skill for database migrations?"_
- _"What skills exist for [domain]?"_
- _"Recommend a skill for testing"_

### Forge path triggers

- _"I want to build a skill for X. What's already out there?"_
- _"Help me make a better version of this skill"_
- _"Audit this skill for security concerns"_
- _"What skills exist for [domain]? How do they compare?"_

### How the tools fit together

|            | Skill Forge (Find)           | Skill Forge (Forge)                           | Skill Creator                                 |
| ---------- | ---------------------------- | --------------------------------------------- | --------------------------------------------- |
| **When**   | You want to use a skill      | You want to build a skill                     | You know what to build                        |
| **Does**   | Searches, verifies, installs | Discovers, analyzes, synthesizes              | Drafts, tests, iterates                       |
| **Output** | Installed skill              | Ecosystem briefing + informed recommendations | Polished SKILL.md                             |

Find and Forge are two paths within Skill Forge. If nothing good exists on the find path, you can escalate to forge. After forging, hand off to Skill Creator for polishing.

## What You Get

### From the Find path
- Quality-verified skill recommendations with install counts and reputation signals
- One-command installation

### From the Forge path
- A **discovery report** cataloging relevant existing skills (`discovery-report.md`)
- A **security audit** of each discovered skill
- A **comparative synthesis** highlighting patterns, anti-patterns, and gaps
- An **informed starting point** for your new skill — built on research, not guesswork

## Project Structure

```
skill-forge/
├── SKILL.md                          # Main skill instructions
├── CLAUDE.md                         # Project guide for contributors
├── references/
│   ├── discovery-template.md         # Template for discovery reports
│   ├── example-discovery-report.md   # Filled-out example report
│   └── security-checklist.md         # Security audit checklist
├── LICENSE
└── README.md
```

## Contributing

Contributions welcome! Some areas where help would be valuable:

- **Discovery sources** — Better search strategies or new ecosystem sources not covered
- **Security patterns** — Additional security checks for the audit checklist
- **Analysis heuristics** — Better ways to evaluate skill quality
- **Quality signals** — New indicators for skill trustworthiness beyond stars and installs

## License

MIT
