# intent — reference

## Text mode

Use when the subject is prose: a message, spec, email, meeting note, clause, log excerpt, thread.

1. Read the whole text once. Mark every sentence that asks for something, sets a condition,
   or gives a reason. Everything else is context.
2. Identify the parties: author, addressee, and whose interest each sentence serves.
3. Reconstruct the intent: what they want (asks), why (reasons, quoted), under what
   constraints (deadlines, limits, dependencies), and what happens if not done.
4. Flag ambiguity explicitly: pronouns without referents, undefined terms, two asks that
   conflict, and tone that suggests an unstated ask (urgency, escalation, blame).
5. Label claims the same way as change mode: `stated` (quote), `inferred` (from which
   sentence and why), `unknown`.

Text-mode skeleton:

```
**Situation** — <who is writing to whom, about what, stated>
**What they want** — 1. <ask> (quote) 2. <ask> (quote)
**Why** — <reason, quoted or "not stated">
**Constraints** — <deadline, scope, conditions>
**One line** — <the intent in one sentence>
**Unclear** — <ambiguities and contradictions>, or "none found"
```

## Change mode

## Evidence ladder

Climb from the most authoritative source to the least. Cite the rung for every claim.

| Rung | Source | What it answers | How to read it |
|---|---|---|---|
| 1 | Ticket, spec, planning doc (source of truth) | Who asked, what outcome they wanted, acceptance criteria | Quote the sentence. Note the ticket status and date. |
| 2 | Commit and PR messages | The author's stated reason at the time | `git log --format='%h %ad %an%n%b' --date=short <range>` |
| 3 | Code diff | What actually changed and where the behavior boundary moved | `git diff <base>..<head> -- <paths>`; name the branch or condition that flipped |
| 4 | Docs, comments, ADRs | The rationale someone wrote down later | Check the date; docs drift from code |
| 5 | Data measurements | How often the problem occurs, who is affected | Read-only `SELECT`, log counts, test runs. Record host and date. |
| 6 | Conversation history | What the user said in this session | Quote; do not paraphrase sentiment into a stronger claim |

Rules for the ladder:

- A lower rung may not override a higher one. If code (3) contradicts the ticket (1),
  report the contradiction; do not decide which is "right" unless the user asks.
- A citation to a decision ("per plan v1.1", "as agreed") is not evidence until you find
  the decision itself. Missing decisions are the most common root of a wrong intent.
- Revert history matters. `git log --follow` on the file and search the ticket key in
  all branches before claiming what production has.

## Claim labels

| Label | Meaning | Required form |
|---|---|---|
| stated | Someone wrote this down | Quote or cite `source:line` |
| inferred | You concluded it from evidence | "inferred from <rung>: <why>" |
| unknown | No source found | Say "not stated" — never fill the gap |

## Output skeleton

```
**Before** — <one or two sentences, stated>
**Problem** — <what broke or was missing>. <measured impact, with host and date if from data>
**What the change achieves**
1. <outcome> (stated: <source>)
2. <outcome> (inferred from <source>)
**One line** — <intent in a single sentence>
**Unknown / conflicting** — <gaps and doc-vs-truth conflicts>, or "none found"
```

## Pitfalls

- Reading the diff and narrating it back is not intent. Every bullet must answer "so what".
- "Improvement" and "refactor" in a commit title are placeholders, not reasons. Dig one rung higher.
- Do not assume the newest document is the correct one. Check which one the ticket points to.
- Measured numbers go in a table or on their own line, never buried in prose.
- When the user is thinking out loud, deliver the assessment and stop. Do not start fixing.

## Subject resolution

| Input | Resolve to |
|---|---|
| empty | The work performed or discussed in the current session |
| `PROJ-123` | Ticket first (rung 1), then commits mentioning the key |
| `a1b2c3d`, `A..B` | Commit range; ticket keys found in messages become rung 1 |
| path | `git log --follow` on the path, most recent ticket-linked change first |
| free text | Search tickets, commit messages, and docs for the phrase; confirm the match with the user if more than one fits |
