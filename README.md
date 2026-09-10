# intent

An agent skill that explains what a pasted text or a code change **wants and why** — not just what it says or does.
It climbs an evidence ladder (ticket → commits → diff → docs → data → conversation), labels every
claim as stated / inferred / unknown, and reports conflicts between documents and the source of truth.

Works with Claude Code, Codex, Gemini CLI, Cursor, Kiro, OpenCode, and any harness that reads `SKILL.md`.

## Install

```bash
# Claude Code (global)
git clone https://github.com/cskwork/intent ~/.claude/skills/intent

# or a shared skills root symlinked into several agents
git clone https://github.com/cskwork/intent ~/.agents/sources/cskwork/intent
ln -s ~/.agents/sources/cskwork/intent ~/.agents/skills/intent
```

## Use

```
/intent                 # intent of the current session's work
/intent PROJ-123        # a ticket
/intent HEAD~3..HEAD    # a commit range
/intent path/to/file    # a file or feature area
```

Or just ask: "why was this changed?", "what's the point of this PR?", "이 수정의 의도가 뭐야".

## Output

Two skeletons, one per mode (text / change): see `SKILL.md`.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | Instructions the agent loads |
| `REFERENCE.md` | Text-mode procedure, evidence ladder, pitfalls, subject resolution |
| `EXAMPLES.md` | Worked examples |

## Principles

- Read-only. Never edits, commits, or posts.
- No invented motives. "Not stated" is a valid answer.

MIT License.
