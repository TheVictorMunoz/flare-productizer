# flare-productizer

An Agent Skill that reviews an existing **Flare** prototype, hackathon build, MVP, or early app and turns it into a plan for a **stronger real product** — ending with a single copy-ready implementation prompt you can *choose* to run.

It behaves like a strong product engineer / technical product lead: direct, evidence-based, no marketing language, no overpraise, no invented traction.

## What it does

- Inspects the repository (and demo / video / screenshots when they can actually be inspected) to reconstruct **what already exists**.
- Reconstructs the **core user flow** and evaluates the real experience.
- Assesses the product across problem/user clarity, core experience, technical product readiness, and the user-facing demo.
- **Verifies the Flare integration** against official Flare sources and classifies it as ESSENTIAL / MEANINGFUL / USEFUL BUT REPLACEABLE / SUPERFICIAL — honestly.
- Identifies the **prototype → product gap** and the single biggest blocker.
- Produces a short, **opinionated** prioritized list (max 3 must-fix, max 3 high-impact next).
- Defines **cheap validation** proportional to the product stage.
- Ends with a **NEXT IMPLEMENTATION PROMPT** — self-contained and copy-ready.

## What it does NOT do

- **It does not automatically edit or implement anything** in the reviewed project. Implementation happens only if you separately paste the generated prompt into a coding agent and tell it to run.
- It is **not** a code-style/lint review — it ignores taste-level nits.
- It is **not** a hackathon scoreboard — no "8/10" gamified scores. Summer Signal scoring stays behind an explicit mode.
- It does **not** invent Flare protocol behavior, addresses, deployments, or maturity — those come from official Flare sources.

## Installation

This repo follows the Agent Skills layout used by [`flare-foundation/flare-ai-skills`](https://github.com/flare-foundation/flare-ai-skills). The skill lives at `skills/flare-productizer/`.

### Claude Code

**Project/user skills directory** — copy the skill folder into your skills directory:

```bash
# project-scoped
mkdir -p .claude/skills
cp -r skills/flare-productizer .claude/skills/

# or user-scoped (all projects)
mkdir -p ~/.claude/skills
cp -r skills/flare-productizer ~/.claude/skills/
```

Then invoke it in a session with `/flare-productizer` or by asking Claude to "use flare-productizer".

**Via `skills.sh` (if you publish this repo):**

```bash
npx skills add https://github.com/<your-org>/flare-productizer --skill flare-productizer
```

### Other Agent-Skills-compatible tools

- **Cursor:** `mkdir -p .cursor/skills && cp -r skills/flare-productizer .cursor/skills/`, then enable it in Settings → Rules for AI.
- **Any `skills.sh`-compatible agent:** point `npx skills add` at this repo, or copy `skills/flare-productizer/` into that tool's skills directory.

## Official Flare knowledge dependencies (recommended)

flare-productizer **delegates all Flare technical claims to official Flare sources** rather than hard-coding protocol details. For the strongest reviews, also install the official Flare AI Skills so it can consult them directly:

Repo: <https://github.com/flare-foundation/flare-ai-skills>

> **Verify the current install commands from that repo** — they can change. As of this writing:

```bash
# skills.sh (recommended)
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-general
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-ftso
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fdc
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fassets
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-smart-accounts
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fcc
```

```bash
# Claude Code plugin marketplace
/plugin marketplace add flare-foundation/flare-ai-skills
/plugin install flare-general@flare-ai-skills
/plugin install flare-ftso@flare-ai-skills
/plugin install flare-fdc@flare-ai-skills
/plugin install flare-fassets@flare-ai-skills
/plugin install flare-smart-accounts@flare-ai-skills
/plugin install flare-fcc@flare-ai-skills
```

When a relevant official skill isn't installed, flare-productizer falls back to the **Flare Developer Hub** (`https://dev.flare.network/` — append `.md` to any page; discover via `https://dev.flare.network/llms.txt`) and **flare-foundation GitHub** repos, and tells you the skill would strengthen the review.

**Source priority for Flare claims:** (1) installed official Flare AI Skill → (2) Flare Developer Hub → (3) flare-foundation GitHub → (4) clearly-labeled secondary info. Never invented.

## How to use it

Point it at a repo and (optionally) a demo, then ask for the review. It infers what it can from the code before asking you anything.

### Basic usage

```
Use flare-productizer to review this repository and give me the highest-priority next implementation prompt. Don't make changes.
```

### Repo + demo example

```
Use flare-productizer to review this repository.

Product: An XRP yield onboarding app.
Target user: XRP holders unfamiliar with EVM.
Demo: https://example.com

Evaluate the current product and Flare integration.
Do not make changes.
Give me the highest-priority next implementation prompt.
```

### Video / screenshot example

```
Use flare-productizer to review this repo.

Here's a demo recording: ./demo.mp4   (or attached screenshots)

Reconstruct the user flow and assess the product. If you can't actually
inspect the recording in this environment, tell me and say what fallback
you need. Do not implement anything.
```

> Honesty guarantee: if the environment can't open your URL / video / screenshots, the skill says so and asks for a fallback (screenshots, a supported-format recording, or a short transcript) instead of pretending to have reviewed them.

### Summer Signal mode example

```
Use flare-productizer in Summer Signal mode.

Review the current repo and this demo recording: [path or URL]

Determine what already works, what separates this prototype from a usable
product, whether the Flare integration is meaningful, and the three
highest-impact improvements. Distinguish pre-existing work from new work.

Do not implement anything. End with the next implementation prompt.
```

## How the Next Implementation Prompt works

Every review ends with a `NEXT IMPLEMENTATION PROMPT` — a self-contained, copy-ready prompt for the **single** highest-priority task (objective, user reason, context, files to inspect, requirements, constraints, required error/edge states, tests, definition of done, and "don't touch unrelated functionality").

The skill **stops there**. Nothing in the reviewed repo is modified. You decide whether to paste that prompt into Claude Code / Cursor / Codex / Hermes / another agent and tell it to implement. Only then does implementation happen.

## Repository layout

```
flare-productizer/
  README.md
  skills/
    flare-productizer/
      SKILL.md          # core skill instructions + workflow
      reference.md      # official Flare source list, skill selection, prompt template
      summer-signal.md  # optional Summer Signal lens
      examples.md       # worked review scenarios (incl. an honest "superficial" case)
```

## Notes on conventions

The official Flare skills use folders named `flare-<name>-skill/`. This skill uses `skills/flare-productizer/` so the invocation name is exactly `flare-productizer`. If you add it to a marketplace/plugin setup that expects the `-skill` suffix, rename the folder accordingly; the `name:` in `SKILL.md` frontmatter is what agents match on.
