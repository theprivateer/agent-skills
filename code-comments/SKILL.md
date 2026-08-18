---
name: code-comments
description: Add and maintain explanatory inline comments that capture non-obvious reasoning, constraints, tradeoffs, and complex behaviour, within strict length ceilings — long-form rationale is relocated to project documentation rather than written at the call site. Use when writing or modifying code, fixing bugs, refactoring, implementing features, scaffolding projects, or when asked to annotate, review, or thin out the comments in existing code. Apply only to source code; skip self-evident code, generated code, migrations, simple configuration, and prose. In review mode, describe observed behaviour without inventing rationale, and condense or remove bloated comments as readily as adding missing ones. If other instructions conflict about comments, ask the user how to proceed.
---

# Inline code comments for sustained context

## Overview

Code accumulates *cognitive debt* as it grows: the developer who returns to a file six months later — or the reviewer seeing it for the first time — has lost the context that made the original choices feel obvious. Self-documenting code names things well, but it cannot tell you why an alternative was rejected, why a workaround exists, or why a piece of logic looks more complicated than it "should". This skill keeps that reasoning available.

Available, but not necessarily *in the file*. A comment is read far more often than it is written — by reviewers, by maintainers, and by coding agents that load the whole file into a context window on every task. Length is therefore a running cost, paid on every future read. Short, high-value comments at the call site; the long-form argument in a document the reader can choose to open.

Apply this skill in two modes:

1. **Authoring mode** — when writing or modifying code. Comments are added as the code is produced, capturing decisions in real time.
2. **Review mode** — when invoked on existing code. Comments are added, condensed, relocated, or removed retroactively, sticking strictly to factual descriptions of behaviour rather than inventing rationale.

## When to comment

Comment at two kinds of locations:

**Decision points** — wherever a meaningful choice was made between viable alternatives. This includes architectural choices, library or pattern selections, performance tradeoffs, deliberate deviations from convention, and any decision that resulted from a user instruction or constraint discussed in the prompt.

**Complex control points** — sections where a reasonably experienced mid-level developer would have to stop and reverse-engineer what is happening. Non-obvious loop conditions, intricate state transitions, recursive calls with non-trivial base cases, off-by-one boundaries that exist for a real reason, bitwise tricks, regex with non-trivial semantics, async/concurrency coordination, and anything where the surface reading does not match the actual behaviour.

Do **not** comment:

- Code that is genuinely self-evident from variable and function names
- Trivial getters, setters, simple delegators, or one-line wrappers
- Standard idioms that any developer in the target language would recognise
- Restatements of what the next line literally says ("increment the counter" above `i += 1`)

When in doubt, leave it out. A comment that restates the code, or explains something the reader would work out in ten seconds, costs attention on every future read and earns nothing. Spend the budget on the two or three genuinely non-obvious things in the file.

## What to write

Use **free-form prose**, not tagged or templated comments. Write the comment as if explaining to a colleague who is reading the code for the first time and asking "wait, why this way?".

A useful comment typically answers one or more of:

- Why this approach over the alternative the reader is probably imagining
- What constraint, requirement, or earlier decision forced this shape
- What the non-obvious behaviour is, and why the obvious reading is wrong
- What will break if this is changed, or what assumption is being relied on

Place comments immediately above the code they describe, or inline at end-of-line for very short clarifications. Match the comment syntax of the language being used.

### Length budget

These are ceilings, not targets. Most comments should come in well under them.

| Location | Ceiling |
| --- | --- |
| Inline / end-of-line | 1 line |
| Above a statement or block | 2 lines |
| On a function, method, or member | 3 lines |
| On a class, module, or file | 6 lines |

The ceiling covers the whole comment, including tags, annotations, and doc-comment scaffolding. A constant holding a tuned value gets one line for what it means and at most two for why it is that number — not a paragraph on the domain it came from.

If the explanation will not fit, that is a signal about *where it belongs*, not a reason to exceed the ceiling.

## Where reasoning lives

Not all reasoning belongs at the call site. There are three homes, and each fact gets exactly one:

**The comment** — the constraint a reader needs *right here* to avoid breaking the code: what will break if this changes, which non-local thing this is coupled to, why the obvious reading is wrong. Fits the length budget above.

**A linked document** — the full argument: threat models, why an alternative was rejected, how a tuned constant was derived, domain background, migration history. If the project has a `docs/` directory, an ADR folder, a wiki convention, or design notes, it goes there. Leave a one-line pointer in the code — the language's cross-reference syntax if it has one (`@see docs/…`), otherwise a plain path in a comment.

**Nowhere** — anything a competent developer in this language reads straight off the code.

When a comment runs past its ceiling, do not truncate the reasoning — relocate it. Write the long version in the document, then write the comment as the one sentence a maintainer needs, plus the pointer. The cost is then paid once by the person who wants the detail, rather than on every read by everyone who does not.

Two constraints:

- **Do not silently create a documentation structure.** If no home exists and the project has no docs convention, keep the comment at its ceiling and tell the user the fuller rationale has nowhere to go.
- **One home per fact.** If the reasoning is already in a document, the comment is a pointer, not a summary of it. Duplicating an explanation guarantees the two will drift, and the reader cannot tell which is current.

## Authoring mode

While producing new code or modifying existing code, add comments as you go. Scan for three categories of reasoning:

**Decisions made in the conversation.** If the user specified a constraint ("we need to support iOS 17.4+", "no third-party dependencies", "this has to be CloudKit-compatible later"), and that constraint shaped the code, note it at the relevant point. The user will not remember the conversation later; the code should.

**Decisions made internally.** When choosing between viable approaches without an explicit instruction — picking a data structure, an algorithm, an error-handling strategy, a concurrency model — briefly note what was chosen and why the alternative was rejected. Be specific about the alternative; "chose a dict over a list for O(1) lookup on the hot path" is useful, "chose the best option" is not.

**Workarounds and constraints.** Anything that exists because of a platform quirk, a library bug, a backward-compatibility requirement, or a deliberate deviation from idiomatic style. These are exactly the comments future maintainers will thank you for.

These three are things to *scan for*, not a template to fill. Most code needs none of them. A function where nothing surprising happened gets no comment at all — resist writing one per category just because the category exists. Where two or three apply to the same piece of code, they share the length budget; they do not each get a paragraph.

**Keep existing comments in sync.** When modifying code that already has an associated comment, update the comment in the same change so it accurately reflects the new behaviour or reasoning. A stale comment is worse than no comment — it actively misleads the next reader. If a change makes an existing comment fully obsolete (the decision it described no longer applies), remove it rather than leaving it stranded.

## Review mode

When the user asks to review or annotate an existing codebase, follow this sequence:

1. **Survey first, do not modify.** Read through the files in scope and identify the locations that warrant comments under the criteria above — and the existing comments that breach the length budget, restate the code, or duplicate a document.
2. **Report back before editing.** Summarise what was found and what would change — roughly how many comments added, condensed, relocated, or removed, in which files. Ask the user to confirm before making any changes.
3. **Stick to factual description.** In review mode, the reasoning behind the original code is unknown. Do not invent rationale. Describe *what* the code does and *how* it behaves, especially at complex control points, but do not speculate on *why* the original author chose this approach unless it is genuinely self-evident from context (e.g. an obvious performance optimisation, a documented platform workaround visible in surrounding code).
4. **Flag uncertainty.** Where the intent is genuinely unclear and a comment would require guessing, either skip it or flag it back to the user as a question rather than fabricating an explanation.
5. **Check density in both directions.** If comments already exceed roughly 25% of non-blank lines in the files under review, the problem is over-commenting rather than under-commenting. Say so, identify the longest blocks and the ones that restate the code, and offer to condense or relocate them — do not add more on top. Removing a redundant comment is as much a part of review as writing a missing one.
6. **Never delete reasoning that has nowhere else to live.** Condensing an over-long comment means moving the detail to a document and leaving a pointer, not dropping it. Only delete outright when the content is genuinely redundant — a restatement of the code, or something a document already says.

**Reviewing at scale.** A comment review over a large codebase means reading that codebase, which is itself expensive. Work in batches by directory or module, delegating to subagents where the tooling allows, and report per batch rather than holding the whole tree in one context.

## Scope

Apply this skill to real source files: the languages and file types that contain the project's actual logic. Skip:

- Generated code (anything marked auto-generated, build artifacts, code emitted from schemas or IDLs)
- Database migration files (typically single-purpose and short-lived)
- Configuration files (JSON, YAML, TOML, .env, and similar) unless the user explicitly asks
- Markdown, plain text, and other non-code documents

If unsure whether a file qualifies, ask.

## Conflict with other instructions

If another skill, project convention, or system instruction tells you not to write documentation or comments — or if a project's existing style strongly suggests comments are unwelcome — **stop and ask the user explicitly** whether inline comments should be added for this task. Do not silently override the other instruction, and do not silently suppress this skill. Surface the conflict and let the user decide.

A project convention about comment *form* is different and does not need escalating. "Prefer doc-comments to inline comments", a required tag style, a fixed placement — these tell you which shape to use, not how long to be. Match the form and keep the ceiling: steering a comment into the more ceremonious form is not licence to write a longer one.
