---
name: readme
description: Create or update a project's README.md from a full repository sweep. Use when the user asks to write, refresh, rebuild, or review a README. Document verified features, structure, usage, local development, testing, building, and deployment. Also evaluate AGENTS.md and any existing client-specific agent instruction files when repository guidance needs to stay in sync.
---

# README updater

## Purpose

Keep the project's `README.md` accurate and complete by deriving its content from the actual codebase rather than memory or stale notes. Complete a thorough file sweep and never rely solely on the current context window. Keep agent instruction files in sync when the sweep uncovers repository guidance that would help future agents work effectively in the project.

## Conflicts with other instructions

If any active skill, project convention, or system instruction says **not** to write documentation — stop immediately, surface the conflict to the user, and ask for explicit permission before continuing. Never silently suppress or silently override another instruction.

---

## Process

Follow these steps in order. Do not skip steps or reorder them.

### Step 1 — Full codebase sweep

Before writing a single line, explore the entire repository. Use a combination of directory listing, file reads, and pattern searches to answer the following questions:

**Project identity**
- What does this project do? (top-level description)
- What problem does it solve, and for whom?
- Is there a license? What kind?

**Tech stack and architecture**
- What language(s), frameworks, and runtimes are in use?
- What is the directory structure and what does each top-level directory contain?
- Are there notable sub-packages, plugins, or modules?
- What external dependencies exist (package manifests, lock files)?

**Features**
- What capabilities does the project expose to its users or consumers?
- Are there CLI commands, API endpoints, library exports, or UI entry points?
- Are there configuration options, flags, or environment variables?

**Local development setup**
- How does a new developer install dependencies?
- Are there environment variable requirements (`.env.example`, documented vars)?
- Are there any required external services, databases, or tools?
- What command starts the development server or local environment?

**Testing**
- What test runner or framework is used?
- Where do tests live?
- What command runs the full test suite?
- Are there separate unit, integration, or end-to-end test commands?

**Deployment and build**
- Is there a build step? What does it produce?
- Is there CI/CD configuration? What does it do?
- How is the project released or deployed?

**Ambiguity**
- Note any areas where the code is unclear, contradictory, or missing context.

Do not write the README yet.

---

### Step 2 — Resolve ambiguities

After the sweep, list every item that is unclear or missing. Ask the user to clarify **all of them in a single message** — do not ask one question at a time, and do not guess. Wait for the user's answers before proceeding.

If nothing is ambiguous, skip this step and proceed directly to Step 3.

---

### Step 3 — Draft the README

Write a complete `README.md`. Use GitHub-flavored Markdown. Structure the file as follows (omit any section for which the project genuinely has no content — do not add placeholder text):

```
# <Project Name>

<One-paragraph description: what it is, what problem it solves, who it is for.>

## Features

<Bullet list of key capabilities. Be specific — name commands, endpoints, exports, or options by name.>

## Project structure

<Short annotated directory tree. One line per entry explaining what each top-level directory or file contains.>

## Getting started

### Prerequisites

<List of required tools, runtimes, or accounts, with version constraints if known.>

### Installation

<Step-by-step numbered instructions to clone, install dependencies, and configure the environment.>

### Environment variables

<Table or list of every env var the project reads, with a description and whether it is required or optional.>

### Running locally

<Commands to start the dev server or run the project locally.>

## Usage

<How to use the project once running. Include examples, commands, or code snippets. This section should be thorough — it is the most-read part of any README.>

## Testing

<How to run tests. Include the specific command(s). If there are multiple test suites or layers, explain each.>

## Building

<If there is a build step, explain what it does and how to invoke it.>

## Deployment

<High-level instructions or pointers to deploy the project, if applicable.>

## Contributing

<Brief contribution guide: branching strategy, PR expectations, code style, or a pointer to CONTRIBUTING.md if one exists.>

## License

<License name and a one-line summary of what it permits.>
```

---

### Step 4 — Write the README

Write the file to `README.md` at the project root. If a `README.md` already exists, **replace it entirely** — do not append. Confirm to the user that the file has been written and summarise the sections included.

---

### Step 5 — Evaluate agent instructions

Review the sweep findings and the newly written README. Consider whether any of the following belong in project-level agent instructions:

- Conventions that are non-obvious from reading the code (naming rules, architectural decisions, forbidden patterns)
- Commands agents should know to run tests, start the development server, or build the project
- Areas of the codebase that are sensitive or require extra care
- Known workarounds, gotchas, or technical debt that would affect how agents should approach tasks

Prefer `AGENTS.md` for agent-neutral guidance. If it does not exist and there is meaningful content to add, create it. If it already exists, update only the sections that need to change and preserve content that remains accurate.

Also inspect any existing client-specific agent instruction files. Update them only when their existing content needs to stay in sync, and preserve their client-specific structure and scope. Do not create a vendor-specific instruction file unless the user asks for one.

If nothing from the sweep warrants an instruction-file change, skip this step and say so. Tell the user which instruction files were added or changed, if any.

---

## Quality constraints

- **Accuracy over completeness.** Never invent features, commands, or configuration that do not exist in the codebase. If something is missing, omit the section or note it as "not yet documented."
- **No filler.** Do not include sections with only placeholder text such as "coming soon" or "see the source." Omit the section entirely.
- **Concrete examples.** Where usage instructions exist, show actual commands or code snippets derived from the real project — not generic templates.
- **Keep it current.** The README should reflect the state of the codebase at the time of the sweep, not aspirational future state.
- **Do not overwrite uncommitted work.** If `README.md` or an agent instruction file has uncommitted changes, warn the user and ask whether to overwrite before writing.
