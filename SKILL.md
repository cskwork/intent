---
name: intent
description: Explain the intent behind anything the user cannot readily parse — a long or confusing text, message, spec, contract clause, log, thread, or a code change, ticket, PR, or decision — by stating what the author wants, why, what they are asking for, and what stays unclear, with every claim tied to the text or cited evidence. Use when the user asks "what does this mean", "what do they want", "why was this done", "의도가 뭐야", "이게 무슨 말이야", "요약 말고 의도", "/intent", pastes a wall of text, or needs a rationale before replying, reviewing, approving, or reporting.
---

# intent

Turn "what it says" or "what changed" into "what they want and why", grounded in the
text or evidence and honest about gaps. Works on two kinds of subject:

- **Text** — a pasted message, spec, email, meeting note, contract clause, log, or thread.
- **Change** — a ticket, commit range, diff, file, feature, or the current session's work.

## Quick start

```
/intent <paste a long or confusing text>   # text mode
/intent                                    # change mode: work of the current session
/intent A20-1305 | HEAD~3..HEAD | src/x    # change mode: ticket, commits, path
```

Output (in the user's language):

1. **Background** — what situation the author or change starts from.
2. **Intent** — what they want and why. Text mode: the ask, the reason, the constraints.
   Change mode: the problem it fixes, with measured impact when available.
3. **What it achieves / what they need from you** — numbered, each tied to a source.
4. **One-line summary** — the intent in a single sentence.
5. **Unknown / conflicting** — motives you could not source, ambiguities, and places
   where the text or documents contradict themselves or the source of truth.

## Workflow

1. **Fix the subject and mode.** Pasted prose → text mode. Ticket key, commit, path,
   or nothing → change mode. If two readings lead to different explanations, ask one
   question; otherwise state the reading and proceed.
2. **Gather evidence** (see [REFERENCE.md](REFERENCE.md)).
   Text mode: the text itself, then who wrote it, to whom, when, and what they replied
   to. Change mode, in order: ticket / spec (source of truth) → commit and PR messages →
   code diff → docs and comments → data measurements → conversation history.
3. **Label every claim**: `stated` (quote or cite), `inferred` (say from what), or
   `unknown`. Never present an inferred motive as stated.
4. **Look for a policy conflict.** When a doc, comment, or commit cites a decision
   ("aligned with plan v1.1"), find that decision. If it does not exist, say so — that
   absence is usually the real intent story.
5. **Quantify the pain when cheap.** A count from a DB, log, or test run makes the
   problem concrete. Read-only queries only; never write.
6. **Write for the reader.** Lead with the outcome. Plain language first, identifiers
   below. Keep code out of prose; put queries and commands in fenced blocks.

## Rules

- Do not invent business motives. If the ticket says nothing, say "not stated".
- Distinguish "what it says / what the code does" from "why someone wanted it". Both belong, in order.
- Text mode is not a summary. Drop detail the reader does not need; keep every ask, deadline, and condition.
- When the change reverses an earlier one, explain the earlier intent too.
- Report out-of-scope problems found on the way as recommendations, not fixes.
- Read-only. This skill never edits files, commits, or posts.

See [EXAMPLES.md](EXAMPLES.md) for a worked example.
