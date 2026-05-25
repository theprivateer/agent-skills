# Claude Skills

A growing collection of custom skills for [Claude Code](https://claude.ai/code). Each skill is a reusable prompt definition that extends what Claude does in a project — triggered automatically by context or on demand via a slash command.

## What are skills?

Claude Code skills are Markdown files stored in `~/.claude/skills/<skill-name>/skill.md`. The file has a YAML frontmatter block that tells Claude when and how to trigger the skill, followed by the instruction set Claude follows when it runs. Claude Code loads all skills from that directory at startup and makes them available in every session.

## Skills

### `/code-comments`

Adds explanatory inline comments to code that capture the *why* behind decisions, not just the *what*. Code accumulates cognitive debt as it grows: the developer returning to a file six months later has lost the context that made the original choices obvious. Self-documenting names help, but they can't tell you why an alternative was rejected or why a workaround exists. This skill makes that reasoning explicit in the source.

Triggers automatically on coding tasks — writing new code, modifying existing code, fixing bugs, refactoring. Also invocable on demand: "add comments to this file", "annotate the code", "this is hard to follow, can you explain it inline".

Does not apply to generated code, migration files, config files, or Markdown.

### `/readme`

Sweeps the full codebase and rewrites `README.md` from what it actually finds, not from memory or stale notes. Also evaluates whether `CLAUDE.md` needs updating. Invoked on demand via `/readme`.

### `/writing-style`

Enforces Australian English, bans common AI-tell words and phrases, and keeps written output sounding like a real human wrote it. Applies to all prose: emails, Slack messages, blog posts, reports, documents, and anything else a human will read. Does not apply to code, variable names, or direct quotes.

Triggers on any request to write, edit, rewrite, or clean up prose. Also fires when cleaning up AI-generated text to remove robotic language.

Attribution: adapted from [AdenCJM/writing-style](https://github.com/AdenCJM/writing-style).

## Project structure

```
claude-skills/
├── code-comments/
│   └── skill.md       # /code-comments skill definition
├── readme/
│   └── skill.md       # /readme skill definition
├── writing-style/
│   └── skill.md       # /writing-style skill definition
├── LICENSE
└── README.md
```

## Installation

Clone this repository and copy the skill directories into your Claude Code skills folder:

```bash
git clone https://github.com/philstephens/claude-skills.git
cp -r claude-skills/code-comments ~/.claude/skills/
cp -r claude-skills/readme ~/.claude/skills/
cp -r claude-skills/writing-style ~/.claude/skills/
```

Claude Code picks up skills from `~/.claude/skills/` automatically at startup. No further configuration is needed.

To install a single skill, copy just that directory:

```bash
cp -r claude-skills/writing-style ~/.claude/skills/
```

## Adding a skill

Create a new directory under the repo root and add a `skill.md` file with this structure:

```markdown
---
name: my-skill
description: One-line description of what this skill does and when it triggers.
user-invocable: true   # omit if the skill triggers automatically, not via /command
---

# Skill title

Instruction set for Claude to follow when this skill runs.
```

Copy the directory to `~/.claude/skills/` and restart Claude Code.

## Contributing

Open a PR with a new skill directory or improvements to an existing one. Keep skill definitions accurate to what Claude actually does — don't describe aspirational behaviour.

## License

MIT — use, copy, modify, and distribute freely with attribution.
