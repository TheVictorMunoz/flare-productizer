---
name: flare-productizer
description: "Use when reviewing an existing Flare prototype, hackathon build, MVP, or early application to determine what is actually built, who it serves, whether the core flow works, whether Flare is load-bearing, and what single next task would most improve the product. Inspects repository and accessible demo evidence, separates verified behavior from inference and claims, identifies the prototype-to-product bottleneck, proposes stage-appropriate validation, and ends with a copy-ready next-task prompt. Review-only: never edits or implements the reviewed project unless the user later gives a separate implementation instruction."
---

# Flare Productizer

## Purpose and boundary

Review an existing Flare project as a product, not as a feature list or hackathon scorecard. Reconstruct what exists, identify the target user's desired outcome, test whether the current experience can deliver that outcome, verify what Flare contributes, and find the single highest-leverage next step.

This skill is **review-only**. Inspecting files, history, tests, deployments, and accessible demos is allowed. Editing, refactoring, committing, deploying, signing, or broadcasting is not. A generated prompt is a handoff; it is not permission to execute the task it describes.

## First-principles model

A **prototype** demonstrates that a capability might work. A **product** lets a specific user reach a useful outcome repeatedly, understand what is happening, recover from failures, and trust the result.

Productization closes the gap between those states. Review the causal chain:

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
Single highest-leverage next task
```

Do not recommend features merely because they sound valuable. Find the current bottleneck. If the user cannot understand the promise, more backend code is not automatically the answer. If the critical transaction path is mocked or unreliable, visual polish is not the answer. If the Flare integration can disappear without changing the outcome, more Flare terminology is not the answer.

A complete review answers:

1. What exists now?
2. Which user is it for, and what outcome do they need?
3. Can the current flow deliver that outcome?
4. What role does Flare play in causing the outcome?
5. What evidence supports the product claims?
6. What one next task removes the biggest blocker?

## When to use

Use for requests such as:

- review or productize a Flare prototype, MVP, hackathon project, or early app;
- inspect a repository and demo before showing it to users;
- determine whether a Flare integration is meaningful;
- identify the biggest prototype-to-product gap;
- review a Summer Signal project;
- produce the highest-priority next-task prompt without changing the repository.

Use a different workflow when the user primarily wants:

- implementation of a Flare feature: use the relevant official Flare skill;
- a general security audit, smart-contract audit, or code-style review;
- market sizing or a full go-to-market strategy without an existing product to inspect;
- transaction signing, deployment, or fund movement.

## Non-negotiable constraints

- **No automatic edits.** Do not modify the reviewed project during this review.
- **No invented observation.** Never describe a demo, video, screenshot, route, test result, deployment, or user behavior that was not actually inspected.
- **No invented Flare facts.** Verify protocol behavior, addresses, deployments, and maturity against current official sources.
- **No invented traction.** A repository cannot prove adoption. Usage, retention, revenue, demand, or product-market fit require supplied product evidence.
- **No giant backlog.** Maximum three must-fix items and three high-impact next items.
- **No gamified score.** Do not emit arbitrary `8/10` category scores.
- **No style-only review.** Ignore naming, formatting, and taste-level code issues unless they affect the user outcome, reliability, trust, security, or ability to ship.

## Evidence model

Use four evidence states:

| State | Meaning |
|---|---|
| `verified` | Observed directly in code, tests, runtime behavior, repository history, deployment evidence, or a demo actually inspected. |
| `inferred` | Reasoned from structure or context but not directly confirmed. |
| `claimed` | Asserted by README, pitch, comments, or user description without independent confirmation. |
| `unavailable` | The relevant source or artifact could not be inspected. |

Attach the evidence state to each material finding. Do not clutter every sentence with a tag; make the evidence basis unambiguous where it changes the conclusion.

Treat sources in this order:

1. Running behavior, source code, tests, configuration, deployment artifacts, and repository history.
2. Accessible demo, screenshots, or video.
3. Project README, docs, pitch, and user-supplied claims.
4. Explicit assumptions, labeled as assumptions.

For a URL, file, or repository supplied by the user, inspect that source directly before relying on summaries or memory.

## Flare source priority

For Flare technical claims, use:

1. the relevant installed official Flare AI Skill;
2. the current [Flare Developer Hub](https://dev.flare.network/) (`.md` pages and `llms.txt` are agent-readable);
3. current [`flare-foundation`](https://github.com/flare-foundation) repositories, docs, code, and READMEs;
4. clearly labeled secondary material only when necessary.

Choose official skills by the mechanism being reviewed:

| Project mechanism | Official skill |
|---|---|
| FTSO price or data feeds | `flare-ftso` |
| FDC attestations or external event/state verification | `flare-fdc` |
| FAssets minting, redemption, FXRP, FBTC, FDOGE, agents, or collateral | `flare-fassets` |
| XRPL-controlled actions on Flare or Flare Smart Accounts | `flare-smart-accounts` |
| Confidential Compute, TEEs, attestation, or reproducible extension builds | `flare-fcc` |
| Networks, RPCs, tooling, contract registry, or general architecture | `flare-general` |

A project may require several. If a relevant skill is unavailable, continue with official docs and repositories. Mention the missing skill only when that limitation materially weakens the conclusion; do not derail the review with installation advice.

`reference.md` contains source links and the next-task prompt template.

## Review workflow

Run the following phases in order. Do not recommend changes until the current product and core flow are reconstructed.

### 1. Establish the review context

Inspect the repository before asking questions. Infer the product name, implementation stack, likely user, current stage, and Flare mechanisms where possible.

Useful inputs include the current repository, target user, product description, live deployment, demo URL, screenshots, recording, transcript, architecture diagram, known blockers, network/deployment information, and program track. Work with partial information.

Ask only questions whose answers could change the review. Prefer a few targeted questions over a questionnaire.

**Completion criterion:** enough context exists to state the likely user, desired outcome, and evidence available for inspection. Unknowns are explicitly recorded.

### 2. Inspect the repository and observable product

Inspect only surfaces that affect product behavior:

- README and product claims;
- frontend routes, components, wallet flows, state management, and user-facing copy;
- backend/API behavior;
- smart contracts and onchain integration points;
- package manifests, network/config files, deployment artifacts, and environment examples;
- critical-path tests;
- mocks, feature flags, hard-coded values, TODOs, and fallback behavior;
- repository history when chronology or new-work claims matter.

If a demo URL is reachable, use it. If media is inspectable, inspect it. If the environment cannot access an artifact, label it `unavailable` and request one practical fallback, such as screenshots or a short walkthrough. Never infer visual or runtime behavior from a filename alone.

Produce a concise `CURRENT PRODUCT` reconstruction:

- target user and job to be done;
- current promise;
- implemented primary flow;
- Flare components involved;
- what is verified;
- what is inferred, claimed, mocked, incomplete, or unavailable.

**Completion criterion:** every important product claim has an evidence basis, and the review can distinguish functioning behavior from pitch or scaffolding.

### 3. Reconstruct the core user flow

Identify the single most important user outcome. Express the current path as a short sequence, for example:

```text
Connect XRPL wallet → resolve Flare Smart Account → choose position
→ send XRP/receive FXRP → wait for confirmation → view resulting position
```

Then map each stage:

| Stage | Question |
|---|---|
| Entry | Does the intended user understand what this is and what to do first? |
| Input/authorization | Do they understand what they are approving or sending? |
| Pending | Does the product explain that work is still in progress? |
| Success | Is the completed outcome clear and verifiable? |
| Failure | Is the reason understandable? |
| Recovery | Can the user retry, resume, or get help without starting blindly? |

Do not expose blockchain terminology unless the user needs it to make a safe decision. Do not call normal FAssets or Smart Accounts flows inherently complex; evaluate the actual interface and path shown by this product.

**Completion criterion:** the review states whether a first-time target user could complete the core outcome without outside instructions, and names the exact step where failure or confusion is most likely.

### 4. Assess product readiness

Assess four dimensions.

#### Problem and user clarity

Determine whether there is an identifiable user, job to be done, painful current alternative, and understandable promise. Separate:

- product hypothesis;
- evidence that the problem exists;
- evidence that the current solution works;
- traction evidence, if any was actually supplied.

#### Core product experience

Evaluate onboarding, action clarity, feedback, cognitive load, failure recovery, transaction lifecycle, technical friction, and whether the user receives the promised outcome.

#### Technical product readiness

Surface only issues that affect the critical path or trust, including:

- mocked or hard-coded behavior presented as real;
- missing transaction lifecycle states;
- fragile wallet/network state;
- missing retries or recovery;
- inconsistent frontend/backend/onchain state;
- insecure secret/config handling;
- non-reproducible setup or deployment assumptions;
- missing critical-path tests;
- production paths that are visibly incomplete.

#### Demo quality

Judge the user-facing demonstration independently from code quality. A sophisticated repository can still fail to communicate or complete a useful outcome.

**Completion criterion:** findings identify causal blockers, not vague requests to “improve UX,” “add polish,” or “write more docs.”

### 5. Verify the Flare integration

For each important Flare primitive:

1. Identify where it appears in code or runtime behavior.
2. Verify the mechanism against official sources.
3. State the user capability it enables.
4. Apply the counterfactual: if this primitive were removed, what capability or outcome would disappear?
5. Classify it and explain the classification.
6. Flag incorrect, incomplete, unnecessary, or superficial use.
7. Recommend a stronger Flare-native approach only when evidence supports it.

Classifications:

| Classification | Test |
|---|---|
| `ESSENTIAL` | The core user capability materially depends on this primitive. |
| `MEANINGFUL` | It materially improves the product, though another architecture could exist. |
| `USEFUL BUT REPLACEABLE` | It adds value but is not central to the outcome or differentiation. |
| `SUPERFICIAL` | Removing it would make little meaningful difference to the user. |

Mechanism probes:

- **FTSO:** Which display, decision, pricing, risk rule, or automation depends on the feed?
- **FDC:** Which external event or state is proved, and what action becomes safe because of that proof?
- **FAssets:** Which native-asset capability becomes available on Flare, and where do minting, ownership, use, or redemption occur?
- **Smart Accounts:** Which XRPL user action controls a Flare account, and what wallet or gas friction is removed?
- **FCC:** Which computation or data must remain confidential or be verifiably executed, and why would public contract execution be insufficient?

Do not manufacture a Flare justification. A `SUPERFICIAL` conclusion is valid.

**Completion criterion:** every material Flare integration has a source-backed mechanism, user consequence, counterfactual, and classification.

### 6. State the prototype-to-product gap

Create a section titled exactly `PROTOTYPE → PRODUCT GAP`:

- `CURRENT STATE`: what is demonstrably built;
- `PRODUCT STATE`: what must be true for the target user to receive the promised outcome reliably;
- `BIGGEST GAP`: the single bottleneck between those states;
- `WHY IT MATTERS`: the consequence for the user.

Choose one biggest gap. Other findings can remain important, but do not hide the bottleneck behind a list.

**Completion criterion:** a reader can explain in one sentence why the current build is still a prototype and what must change first.

### 7. Prioritize and define validation

Return:

- `MUST FIX BEFORE MORE USERS`: maximum three items;
- `HIGH-IMPACT NEXT`: maximum three items;
- `LATER`: only important work that should not distract the team now.

For each item include:

- issue;
- user consequence;
- evidence;
- recommended change;
- likely repository area, if identifiable;
- cheapest useful validation.

Validation must match the stage. Examples include observing five target users without instructions, measuring core-flow completion, checking comprehension before the main action, testing failure recovery, or verifying that a user can explain what Flare enabled after a short demo.

Do not assume every recommendation requires code.

**Completion criterion:** the top item directly addresses the `BIGGEST GAP`, and each proposed validation could change a product decision.

### 8. Generate one next-task prompt and stop

End with one self-contained copy-ready prompt for the highest-priority task.

Use the title that matches the work:

- `NEXT IMPLEMENTATION PROMPT` when the next task is a code or configuration change;
- `NEXT TECHNICAL SPIKE PROMPT` when the next task is to resolve technical uncertainty;
- `NEXT VALIDATION PROMPT` when the next task is user/product evidence rather than implementation.

The prompt must contain:

- objective;
- user/product reason;
- relevant verified and inferred context;
- files, components, evidence, or participants to inspect;
- requirements or procedure;
- constraints;
- relevant error and edge states;
- tests or validation criteria;
- definition of done;
- explicit instruction not to modify unrelated functionality when code is involved.

Do not generate an implementation prompt merely to satisfy the format if the actual bottleneck is validation or positioning.

**Stop after the prompt.** Do not begin the task unless the user gives a separate explicit instruction to execute it.

## Default output structure

```text
# Flare Productizer Review
## 1. Current Product
## 2. Core User Flow
## 3. What Already Works
## 4. Product and UX Findings
## 5. Technical Product Readiness
## 6. Flare Integration Review
## 7. Prototype → Product Gap
## 8. Prioritized Improvements
### Must Fix Before More Users
### High-Impact Next
### Later
## 9. Validation Plan
## 10. Summer Signal Readiness    # only in Summer Signal mode
## 11. Next Task Prompt
```

Keep the review concise enough to drive a decision. Evidence and causal explanation matter more than section length.

## Summer Signal mode

Activate only when the user explicitly requests Summer Signal review. Apply `summer-signal.md` after the normal product review. Verify current program and track definitions from official sources. Separate pre-existing work from new work only when repository history or supplied evidence supports that distinction.

Do not let program scoring language leak into ordinary reviews.

## Common pitfalls

1. **Starting with recommendations.** Reconstruct the product and user flow first.
2. **Trusting the README.** Treat documentation as `claimed` until code, runtime behavior, or other evidence confirms it.
3. **Mistaking technical sophistication for product quality.** Judge whether the user reaches and understands the outcome.
4. **Counting Flare integrations.** Use mechanism and counterfactual value instead.
5. **Forcing code as the next step.** Use a validation or spike prompt when that is the actual bottleneck.
6. **Calling missing polish the biggest gap.** Name the underlying failure: unclear promise, broken flow, missing state, unreliable result, or absent evidence.
7. **Pretending to inspect inaccessible media.** Mark it unavailable and request one useful fallback.
8. **Producing a backlog.** Preserve focus by keeping the stated limits.
9. **Executing the handoff prompt.** Stop after generating it.

## Completion checklist

- [ ] Target user, job to be done, and promised outcome are stated.
- [ ] Current product behavior is reconstructed from direct evidence.
- [ ] Verified, inferred, claimed, and unavailable information remain distinct.
- [ ] Core flow includes pending, success, failure, and recovery behavior.
- [ ] Product findings identify causal blockers rather than vague polish requests.
- [ ] Technical findings affect the critical path, trust, security, or ability to ship.
- [ ] Every material Flare integration has a current official source and counterfactual classification.
- [ ] Traction is not inferred from code or pitch.
- [ ] One `BIGGEST GAP` is selected.
- [ ] Priorities stay within the maximum limits.
- [ ] Validation is proportional to stage and capable of changing a decision.
- [ ] The final prompt type matches the actual next task.
- [ ] No repository files were changed during the review.
- [ ] The agent stopped after producing the prompt.

## Additional resources

- `reference.md`: official source order and next-task prompt templates.
- `examples.md`: abbreviated example reviews.
- `summer-signal.md`: optional program-specific lens.
