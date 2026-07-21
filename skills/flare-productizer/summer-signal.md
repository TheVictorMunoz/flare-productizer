# flare-productizer Summer Signal mode

This is an optional program-specific lens. Activate it only when the user explicitly asks for a Summer Signal review.

Summer Signal rules, tracks, dates, and judging language can change. Do not treat this file as the source of truth for the current program. Before applying the lens:

1. inspect the current official program brief, submission page, rules, or user-supplied judging criteria;
2. cite the source used;
3. mark any unavailable criterion rather than reconstructing it from memory.

If no current official brief is accessible, complete the normal product review and clearly state that program-specific alignment could not be verified.

## Add one section: `Summer Signal Readiness`

Keep the normal product review intact. Add only findings that are supported by the current program criteria.

At minimum, assess these product-level questions when they are relevant to the supplied brief:

- Does the project solve a recognizable user problem or mainly demonstrate technology?
- Is the Flare integration essential, meaningful, replaceable, or superficial under the mechanism test in `SKILL.md`?
- Does the critical path work as shown?
- What evidence distinguishes work completed during the program from older work?
- Can an outsider understand the product, current state, and next credible milestone?

Do not convert these questions into arbitrary 1–10 scores unless the official current program explicitly requires numeric scoring.

## New-work ledger

When the project predates the program, separate:

| Category | Meaning |
|---|---|
| `pre-existing` | Evidence shows the capability existed before the program window. |
| `newly built` | Evidence shows it was created during the program window. |
| `ported` | Existing work was moved from another stack or chain with limited functional change. |
| `integrated or improved` | Existing work was materially extended or connected to Flare during the program. |
| `undetermined` | Available history cannot support the distinction. |

Use repository history, release/deployment timestamps, dependency additions, first appearance of Flare-specific code, changelogs, and supplied demo evidence. A commit timestamp proves when a commit appeared; it does not always prove when the work was created. State that limitation where relevant.

Never guess the ledger to make a submission look stronger.

## Track alignment

Use the exact track names and definitions from the current official program brief. For each candidate track:

1. quote or summarize the criterion accurately;
2. identify the repository or product evidence that matches it;
3. state the mismatch or missing evidence;
4. conclude `aligned`, `partially aligned`, or `unclear`.

Do not force-fit the project. If the current track definitions are unavailable, do not publish old track names as though they are current.

## Tone and output

Keep the same evidence standards as the main review. The purpose is to help the builder show real product value and real work, not to simulate a judge or inflate the submission.

The final next-task prompt must still target the product's biggest bottleneck. Program presentation work should not outrank a broken or unproven core user outcome merely because a deadline exists.
