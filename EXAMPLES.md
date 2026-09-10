# intent — worked examples

## Example 1 — text mode

Subject: a long, rambling message from a project manager, pasted by the user.

> "...as discussed we still have the issue from last week and the client keeps asking, I
> think the dashboard numbers are off again but not sure if it's the same thing as the
> report thing, anyway can we get something before the Thursday call, doesn't have to be
> perfect, also the export they asked for last month..."

**Situation** — A project manager writing to the developer about a client complaint, ahead of a Thursday client call (stated).

**What they want**
1. Something showable about the dashboard numbers before Thursday (stated: "can we get something before the Thursday call").
2. A judgment on whether the dashboard issue and the "report thing" are the same defect (inferred from "not sure if it's the same thing"; they did not ask directly).
3. Status on last month's export request (stated, but no deadline given).

**Why** — The client keeps asking (stated). Which issue "from last week" is meant is not stated.

**Constraints** — Thursday; "doesn't have to be perfect" lowers the bar to a partial fix or an explanation.

**One line** — They need a credible answer about the dashboard numbers for Thursday, and secondarily want the two open issues triaged.

**Unclear** — "the issue from last week" and "the report thing" have no referent; the export request has no owner or date. Ask which issue before starting.

## Example 2 — change mode

Subject: a bug ticket that changed how a dashboard classifies "strong" and "weak" concepts.

### Evidence gathered

| Rung | Finding |
|---|---|
| 1 Ticket | Parent feature ticket says "apply the same classification rule as the existing feature; exclude items equal to the threshold". Sub-ticket says the new dashboard used 60/80 thresholds and no learning-history check. |
| 2 Commits | Original implementation commit: "aligned with plan v1.1 (2026-08-12)". Fix commit: "restore legacy 0.5 rule and history gate". |
| 3 Diff | Threshold constants 60/80 → 50/50; strict `>` on the strong side; new CTE joining the learning-page table; individual-view tie-break flipped to KC ID descending. |
| 4 Docs | API spec §7.1 claimed "plan v1.1 retired the 0.5 rule". The planning repository has no v1.1 decision about thresholds. |
| 5 Data | dev DB: 29% of score rows fell in the retired middle zone; 104 of 362 course/unit/lecture combinations had scores but zero learning history. |
| 6 Conversation | User: "the policy to keep is the existing KC logic". |

### Output

**Before** — The legacy dashboard classified a concept as strong above 0.5 and weak below 0.5, and only for units the student had actually studied (stated: legacy mapper queries).

**Problem** — The new dashboard shipped with 60/80 thresholds and no history check, citing a planning decision that does not exist (stated: spec §7.1; planning repo search). The same student therefore got different strengths in the two screens, and units never studied showed analysis built from pre-generated engine scores (inferred from rung 5: 29% of rows changed class; 29% of unit combinations had no history).

**What the change achieves**
1. Old and new screens give the same answer for the same student (stated: parent ticket).
2. "No learning history" becomes a true statement about the unit, not about score availability (stated: sub-ticket).
3. Individual and class views break ties the same way (inferred from diff; not requested in the ticket, agreed in conversation).
4. The spec no longer records a non-existent decision as the source of truth (stated: doc diff).

**One line** — The new feature had not changed policy; it had mis-implemented the existing one, and this change restores the documented rule.

**Unknown / conflicting** — Who introduced the "plan v1.1" claim and why is not recorded. The planning summary the user pasted says "0.5 or above" while the ticket says "exclude equal"; the ticket was followed.
