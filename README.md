# 🔨 Skill Forge

Research-driven skill creation for Claude. Discovers, analyzes, and learns from existing skills before building new ones — so you build better, not blind.

## 🤔 The Problem

Today, creating a Claude skill is either "build from scratch" or "install someone else's as-is." There's no middle ground. Skill Forge fills that gap by adding a **research and synthesis layer** before creation — finding what's already out there, analyzing what works and what doesn't, flagging security concerns, and using those insights to create something genuinely better.

## ⚡ What It Does

When you say something like _"I want to build a skill for code review"_, Skill Forge:

1. 🔍 **Discovers** existing skills across GitHub, skill directories, and plugin marketplaces
2. 🔬 **Analyzes** each one — structure, techniques, security posture, effectiveness
3. 📊 **Synthesizes** findings into a clear briefing: what works, what doesn't, where the gaps are
4. 🛠️ **Creates** a new skill informed by that research, or hands off to your preferred creation workflow

## 📦 Installation

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

### ✅ Verify Installation

Start a new Claude Code session and try: _"What skills exist for code review?"_ — Skill Forge should activate and begin asking about your intent before searching.

### Claude.ai

Copy the contents of `SKILL.md` into your project's custom instructions or system prompt. Discovery will use web search instead of CLI tools, but the analysis and creation phases work the same way.

## 🚀 Usage

Skill Forge triggers when you want to research the landscape before building. Example prompts:

- _"I want to build a skill for X. What's already out there?"_
- _"Help me make a better version of this skill"_
- _"What skills exist for [domain]? How do they compare?"_
- _"Audit this skill for security concerns"_

### Skill Forge vs. Skill Creator

|            | Skill Forge                                   | Skill Creator                                 |
| ---------- | --------------------------------------------- | --------------------------------------------- |
| **When**   | Before you build — research phase             | After you know what to build — creation phase |
| **Does**   | Discovers, analyzes, synthesizes              | Drafts, tests, iterates                       |
| **Output** | Ecosystem briefing + informed recommendations | Polished SKILL.md                             |

They complement each other. Use Skill Forge first, then hand off to Skill Creator for polishing.

## 📋 What You Get

After running Skill Forge, you'll have:

- 📄 A **discovery report** cataloging relevant existing skills (`references/discovery-report.md`)
- 🔒 A **security audit** of each discovered skill
- 📊 A **comparative synthesis** highlighting patterns, anti-patterns, and gaps
- 🎯 An **informed starting point** for your new skill — built on research, not guesswork

## 🗂️ Project Structure

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

## 🤝 Contributing

Contributions welcome! Some areas where help would be valuable:

- 🌐 **Discovery sources** — Know of skill directories or registries not covered? Open an issue or PR.
- 🛡️ **Security patterns** — Additional security checks for the audit checklist.
- 📈 **Analysis heuristics** — Better ways to evaluate skill quality.

## 📄 License

MIT
