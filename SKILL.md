---
name: intent
description: Explain the intent behind a change, feature, bug fix, or decision — what was true before, what problem it caused, what the change achieves, and what remains unknown — using cited evidence rather than guesses. Use when the user asks "why was this done", "what's the point of this change", "의도가 뭐야", "왜 고친 거야", "/intent", or needs a rationale for a ticket, commit, PR, diff, or feature before reviewing, approving, or reporting it.
---

# intent

Turn "what changed" into "why it changed", grounded in evidence and honest about gaps.
The deliverable is an explanation a non-developer stakeholder can follow, with the
technical sources listed underneath.

## Quick start

```
/intent                         # subject = the work of the current session
/intent A20-1305                # a ticket key
/intent HEAD~3..HEAD            # a commit range
/intent src/x/Foo.java          # a file or feature area
```

Output (in the user's language):

1. **Background** — what was true before, in one or two sentences.
2. **Problem** — what went wrong or was missing, with measured impact when available.
3. **What the change achieves** — outcomes, numbered, each tied to a source.
4. **One-line summary** — the intent in a single sentence.
5. **Unknown / conflicting** — motives you could not source, and places where
   documents disagree with the source of truth.

## Workflow

1. **Fix the subject.** Ticket, commit range, diff, file, feature, or the current
   session. If two readings lead to different explanations, ask one question; otherwise
   state the reading and proceed.
2. **Climb the evidence ladder** (see [REFERENCE.md](REFERENCE.md)). Stop early only
   when the higher rungs already answer the question.
   ticket / spec (source of truth) → commit and PR messages → code diff → docs and
   comments → data measurements → conversation history.
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
- Distinguish "what the code does" from "why someone wanted it". Both belong, in order.
- When the change reverses an earlier one, explain the earlier intent too.
- Report out-of-scope problems found on the way as recommendations, not fixes.
- Read-only. This skill never edits files, commits, or posts.

See [EXAMPLES.md](EXAMPLES.md) for a worked example.
