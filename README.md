# Claude Skills

A (growing) collection of custom skills I have created and use with Claude.

### /code-comments

Code accumulates *cognitive debt* as it grows: the developer who returns to a file six months later — or the reviewer seeing it for the first time — has lost the context that made the original choices feel obvious. Self-documenting code names things well, but it cannot tell you why an alternative was rejected, why a workaround exists, or why a piece of logic looks more complicated than it "should". This skill makes that reasoning explicit in the source itself.

### /readme

Keep the project's `README.md` accurate and complete by deriving its content from the actual codebase rather than memory or stale notes. A thorough file sweep is mandatory — never rely solely on the current context window. The skill also updates `CLAUDE.md` when the sweep uncovers information that would help future Claude sessions work effectively in the project.

### /writing-style

A Claude Code skill that enforces Australian English, bans common AI-tell words, and keeps written output sounding like a real human wrote it.

All credit goes to https://github.com/AdenCJM/writing-style - cloned here for ease of portability.
