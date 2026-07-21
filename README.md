# flare-productizer

`flare-productizer` is an Agent Skill for reviewing an existing Flare prototype, hackathon build, MVP, or early application. It reconstructs what has actually been built, identifies the main gap between the prototype and a usable product, evaluates whether Flare is doing meaningful work, and returns the highest-priority next task as a copy-ready prompt.

It reviews. It does not modify the project being reviewed.

## Start here: what an Agent Skill is

An Agent Skill is a folder of instructions and reference files that an AI coding agent can load when a task matches the skill. It is not a standalone application, hosted service, model, or executable.

After you install this repository, your agent reads `skills/flare-productizer/SKILL.md` and follows its review process. The result still depends on what the agent can access: the repository, its available tools, any demo you provide, and current Flare documentation.

In practical terms:

```text
Your repo + optional demo + product context
                    ↓
     AI agent using flare-productizer
                    ↓
Evidence-based product review + one next-task prompt
```

## The productization idea, from first principles

A prototype proves that some capability can work. A product lets a specific user reach a useful outcome repeatedly, understand what is happening, recover when something fails, and trust the result.

Productization is the work between those states. The skill therefore reviews the causal chain rather than counting features:

```text
User and job to be done
        ↓
Core user flow
        ↓
Actual system behavior and failure states
        ↓
What Flare uniquely enables
        ↓
Evidence that the outcome works
        ↓
Single highest-leverage next step
```

The review asks four basic questions:

1. What exists now, based on code and observable behavior rather than the pitch?
2. What outcome is the target user trying to reach?
3. What currently blocks that outcome from being understandable, dependable, or useful?
4. What is the smallest next task that removes the biggest blocker?

## What the skill produces

A normal review includes:

- a reconstruction of the current product, separated into verified, inferred, and merely claimed behavior;
- the core user flow and its success, pending, failure, and recovery states;
- product and UX findings tied to evidence;
- technical readiness issues that affect the critical user path, not style-level code comments;
- a Flare integration review based on current official sources;
- a `PROTOTYPE → PRODUCT GAP` with one clearly stated biggest blocker;
- at most three must-fix items and three high-impact next improvements;
- cheap validation appropriate to the product's stage;
- one `NEXT IMPLEMENTATION PROMPT` for the highest-priority next task.

The last prompt may describe code, a technical spike, or a validation task. The skill must not force more implementation when the evidence says the next bottleneck is product understanding or user validation.

## How it evaluates Flare

The skill does not reward a project for mentioning many Flare protocols. For each important integration, it asks a counterfactual question:

> If this Flare primitive disappeared, what user capability or product outcome would be lost?

It then classifies the integration:

| Classification | Meaning |
|---|---|
| `ESSENTIAL` | The core capability depends on it. |
| `MEANINGFUL` | It materially improves the product, although another architecture could exist. |
| `USEFUL BUT REPLACEABLE` | It adds value but is not central to the product. |
| `SUPERFICIAL` | Removing it would barely change the user experience or outcome. |

Technical claims are checked against installed official Flare AI Skills when available, then the [Flare Developer Hub](https://dev.flare.network/), then current [`flare-foundation`](https://github.com/flare-foundation) repositories. The skill does not hard-code contract addresses, deployment status, or protocol behavior.

## What it does not do

- It does not edit, refactor, deploy, or implement anything in the reviewed repository.
- It does not sign transactions, access wallets, or handle private keys.
- It does not treat README claims as proof that a feature works.
- It does not infer traction from code. Traction requires product evidence such as usage data, completed user tests, or supplied customer evidence.
- It does not perform a general lint or code-style review.
- It does not generate a giant backlog or gamified `8/10` scores.
- It does not pretend to have inspected a URL, video, or screenshot that the agent could not actually access.

## Install

### Quick install with `skills`

Run this from the project where you want to use the skill:

```bash
npx skills add https://github.com/TheVictorMunoz/flare-productizer --skill flare-productizer
```

The CLI finds the public repository, discovers `flare-productizer`, and asks which compatible agent(s) should receive it. This command has been tested against the published repository.

Useful variants:

```bash
# Install for your user account instead of one project
npx skills add https://github.com/TheVictorMunoz/flare-productizer \
  --skill flare-productizer --global

# Non-interactive install for named agents
npx skills add https://github.com/TheVictorMunoz/flare-productizer \
  --skill flare-productizer --agent claude-code cursor --yes
```

The installer supports Claude Code, Cursor, Codex, Hermes Agent, and other Agent-Skills-compatible tools. Review a skill before using it: an installed skill guides an agent that may have access to your files and tools.

### Manual install

Clone this repository, then copy the entire skill folder into the skills directory used by your agent. Keep `SKILL.md`, `reference.md`, `examples.md`, and `summer-signal.md` together.

```bash
git clone https://github.com/TheVictorMunoz/flare-productizer.git
cd flare-productizer

# Claude Code, project-scoped
mkdir -p /path/to/your-project/.claude/skills
cp -r skills/flare-productizer /path/to/your-project/.claude/skills/

# Cursor, project-scoped
mkdir -p /path/to/your-project/.cursor/skills
cp -r skills/flare-productizer /path/to/your-project/.cursor/skills/
```

For user-wide installation, copy the folder to `~/.claude/skills/` or `~/.cursor/skills/` instead.

## Optional Flare knowledge skills

`flare-productizer` works without additional skills because it can consult official Flare documentation and repositories. Installing the official [Flare AI Skills](https://github.com/flare-foundation/flare-ai-skills) gives the reviewing agent more structured protocol-specific context.

Install only the skills relevant to the project, for example:

```bash
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-general
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-ftso
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fdc
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fassets
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-smart-accounts
npx skills add https://github.com/flare-foundation/flare-ai-skills --skill flare-fcc
```

These are reference dependencies, not runtime dependencies. The reviewed application does not need to import them.

## Use

Ask the agent to use the skill by name and point it at the project. Explicitly state that you want a review rather than implementation.

### Minimal request

```text
Use flare-productizer to review this repository.

Reconstruct what is actually implemented, identify the biggest gap between the
current prototype and a usable product, and evaluate whether the Flare
integration is meaningful. Do not modify any files. End with the single
highest-priority next-task prompt.
```

### With product context and a demo

```text
Use flare-productizer to review this repository and the demo below.

Product: An XRP yield onboarding app
Target user: XRP holders who do not use EVM wallets
Demo: https://example.com

Evaluate the current user flow, technical readiness, and Flare integration.
Separate what you verify from what you infer or what the README only claims.
Do not modify the project. End with the next-task prompt.
```

### With screenshots or video

```text
Use flare-productizer to review this repository and the attached screenshots
(or ./demo.mp4). Reconstruct the real user flow. If you cannot inspect the
media in this environment, say so and ask for one usable fallback. Do not
invent observations and do not modify the project.
```

### Summer Signal mode

```text
Use flare-productizer in Summer Signal mode to review this repository and demo.
Separate pre-existing work from work completed during the program when the
repository history supports that distinction. Do not guess when it does not.
Do not modify the project. End with the next-task prompt.
```

Summer Signal mode adds program-specific review questions. It does not replace the normal product review and does not turn the output into a scorecard.

## Understanding the final prompt

`NEXT IMPLEMENTATION PROMPT` is a handoff, not an automatic action. It contains the objective, user reason, existing context, files or components to inspect, requirements, constraints, edge states, validation, and definition of done for one task.

The reviewing agent stops after writing it. You decide whether to give that prompt to a coding agent. No implementation happens merely because the prompt exists.

## Repository layout

```text
flare-productizer/
├── README.md
└── skills/
    └── flare-productizer/
        ├── SKILL.md          # behavior and review workflow
        ├── reference.md      # source order and next-task template
        ├── examples.md       # abbreviated example reviews
        └── summer-signal.md  # optional program-specific lens
```

## License

MIT
