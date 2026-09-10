# intent

An agent skill that explains **why** a change, feature, fix, or decision exists — not just what it does.
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

1. Before
2. Problem, with measured impact when cheap to get
3. What the change achieves, each item cited
4. One-line summary
5. Unknown / conflicting

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | Instructions the agent loads |
| `REFERENCE.md` | Evidence ladder, claim labels, output skeleton, pitfalls |
| `EXAMPLES.md` | A worked example |

## Principles

- Read-only. Never edits, commits, or posts.
- No invented motives. "Not stated" is a valid answer.
- A cited decision is not evidence until the decision itself is found.

MIT License.
