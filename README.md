# Agent Skills

A growing collection of reusable skills for AI coding agents. The repository follows the open [Agent Skills specification](https://agentskills.io/specification), so each skill can be used by any compatible agent rather than being tied to one model or client.

## What are skills?

An Agent Skill is a directory containing a `SKILL.md` file. Its YAML frontmatter gives the skill a name and tells an agent when to load it. The Markdown body contains the instructions the agent follows. A skill can also include scripts, references, and assets when the workflow needs them.

Skill discovery and installation paths vary between clients. The skill contents in this repository avoid client-specific commands, tools, and configuration unless a requirement is explicitly labelled.

## Skills

### `code-comments`

Adds explanatory inline comments to code that capture the *why* behind decisions, not just the *what*. Code accumulates cognitive debt as it grows: the developer returning to a file six months later has lost the context that made the original choices obvious. Self-documenting names help, but they can't tell you why an alternative was rejected or why a workaround exists. This skill makes that reasoning explicit in the source.

Applies to coding tasks such as writing new code, modifying existing code, fixing bugs, and refactoring. It also handles direct requests such as "add comments to this file", "annotate the code", or "explain this inline".

Does not apply to generated code, migration files, config files, or Markdown.

### `readme`

Sweeps the full codebase and rewrites `README.md` from what it actually finds, not from memory or stale notes. It also evaluates whether `AGENTS.md` or existing client-specific agent instructions need updating. Use it by asking the agent to create, update, or review a project's README.

### `writing-style`

Enforces Australian English, bans common AI-tell words and phrases, and keeps written output sounding like a real human wrote it. Applies to all prose: emails, Slack messages, blog posts, reports, documents, and anything else a human will read. Does not apply to code, variable names, or direct quotes.

Triggers on any request to write, edit, rewrite, or clean up prose. Also fires when cleaning up AI-generated text to remove robotic language.

Attribution: adapted from [AdenCJM/writing-style](https://github.com/AdenCJM/writing-style).

## Project structure

```
agent-skills/
├── code-comments/
│   └── SKILL.md       # Inline code-comment guidance
├── readme/
│   └── SKILL.md       # README maintenance workflow
├── writing-style/
│   └── SKILL.md       # Australian English writing rules
├── LICENSE
└── README.md
```

## Installation

Clone the repository, then copy or link the skill directories into a location your agent scans. You can choose a different local checkout name without changing the skills:

```bash
git clone https://github.com/theprivateer/claude-skills.git agent-skills
cd agent-skills
```

For clients that support the shared `.agents/skills` convention, install all three skills at project level with:

```bash
mkdir -p /path/to/project/.agents/skills
cp -R code-comments readme writing-style /path/to/project/.agents/skills/
```

Other clients use their own discovery directories. For example, Codex uses `${CODEX_HOME:-$HOME/.codex}/skills`, while Claude Code uses `$HOME/.claude/skills`. Check the client's documentation if it does not scan `.agents/skills`.

To install one skill for Codex, for example:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R writing-style "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Restart the client or start a new session if it does not detect newly installed skills immediately.

## Adding a skill

Create a directory under the repository root and add an exact-case `SKILL.md` file:

```markdown
---
name: my-skill
description: One-line description of what this skill does and when it triggers.
---

# Skill title

Instructions for an agent to follow when this skill runs.
```

Keep the directory name identical to the frontmatter `name`. Use lowercase letters, numbers, and hyphens. Put all trigger guidance in `description`, keep the body focused on execution, and use imperative language.

Avoid assumptions about a particular client:

- Refer to the "agent" rather than a model or product name.
- Do not depend on slash commands, proprietary frontmatter, fixed tool names, or one client's configuration paths.
- Prefer `AGENTS.md` for neutral project guidance. Preserve existing client-specific instruction files when a task requires it, but do not create them by default.
- Declare real environment or client requirements with the specification's optional `compatibility` field.

Validate each skill against the [Agent Skills specification](https://agentskills.io/specification) before submitting it.

## Contributing

Open a PR with a new skill directory or improvements to an existing one. Keep instructions portable, concise, and accurate to what compatible agents can actually do. Do not describe aspirational behaviour as current capability.

## License

MIT. You may use, copy, modify, and distribute the skills with attribution.
