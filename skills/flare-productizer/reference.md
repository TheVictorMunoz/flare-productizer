# flare-productizer reference

Use this file for source selection and final prompt construction. The review method itself lives in `SKILL.md`.

## Official Flare sources

Resolve Flare technical claims from current primary sources. Do not copy an address, deployment state, supported asset, or protocol behavior from an old review.

### 1. Official Flare AI Skills

Repository: <https://github.com/flare-foundation/flare-ai-skills>

| Skill | Use it to verify |
|---|---|
| `flare-general` | Networks, RPCs, explorers, faucets, chain IDs, contract discovery, tooling, and general architecture |
| `flare-ftso` | FTSO feeds, feed identifiers, onchain/offchain consumption, and delegation |
| `flare-fdc` | Attestation types, proof acquisition, DA-layer interaction, and onchain verification |
| `flare-fassets` | FAssets/FXRP minting and redemption, agents, collateral, vault paths, and contract integration |
| `flare-smart-accounts` | XRPL-controlled Flare accounts, proof-based instructions, direct-minting flows, executors, and account behavior |
| `flare-fcc` | Confidential Compute, TEE extensions, registries, attestation, instruction routing, and reproducible builds |

An installed skill is structured reference material, not proof that the reviewed project implemented the mechanism correctly. Compare the project against the source.

### 2. Flare Developer Hub

- Documentation root: <https://dev.flare.network/>
- Agent-readable index: <https://dev.flare.network/llms.txt>
- Network getting started: <https://dev.flare.network/network/getting-started>
- FTSO getting started: <https://dev.flare.network/ftso/getting-started>
- FDC getting started: <https://dev.flare.network/fdc/getting-started>
- FAssets developer guides: <https://dev.flare.network/fassets/developer-guides>
- Smart Accounts guides: <https://dev.flare.network/smart-accounts/guides>
- Confidential Compute getting started: <https://dev.flare.network/fcc/guides/getting-started>

Developer Hub pages can normally be requested as Markdown by appending `.md`. If direct retrieval fails, inspect the canonical page through an available browser or use the official repository source. Do not silently fall back to memory.

### 3. Official GitHub repositories

Organization: <https://github.com/flare-foundation>

Use current code, release information, and repository documentation when implementation details matter. Confirm that a repository and branch are still current before treating them as authoritative.

### 4. Secondary material

Use secondary sources only when primary sources do not answer the question. Label them and do not let them override current official code or documentation.

## Mechanism-to-skill selection

Choose by what causes the user outcome:

- a feed drives a display, rule, valuation, risk decision, or automation: `flare-ftso`;
- an external transaction, event, or state must be proved: `flare-fdc`;
- XRP or another non-smart-contract asset is minted, used on Flare, or redeemed: `flare-fassets`;
- an XRPL address controls actions on Flare or avoids a separate EVM wallet/gas path: `flare-smart-accounts`;
- private data or computation needs confidentiality plus verifiable execution: `flare-fcc`;
- network, tooling, registry, or general architecture questions: `flare-general`.

A mechanism can require more than one skill. For example, a direct FXRP-to-vault flow may require both `flare-fassets` and `flare-smart-accounts`; a product using an FTSO price to manage an FXRP position may also require `flare-ftso`.

## Evidence labels

Use these labels where the distinction matters:

- `verified`: observed in code, tests, runtime behavior, deployment evidence, repository history, or an inspected demo;
- `inferred`: a reasoned interpretation not directly confirmed;
- `claimed`: stated by project documentation, pitch, comments, or the user without independent confirmation;
- `unavailable`: evidence could not be inspected.

Repository code can verify implementation but cannot by itself verify adoption, retention, revenue, customer demand, or product-market fit.

## Next-task prompt template

Select the title that matches the bottleneck:

- `NEXT IMPLEMENTATION PROMPT` for a code/configuration change;
- `NEXT TECHNICAL SPIKE PROMPT` for resolving technical uncertainty;
- `NEXT VALIDATION PROMPT` for user or product evidence.

```text
<NEXT IMPLEMENTATION PROMPT | NEXT TECHNICAL SPIKE PROMPT | NEXT VALIDATION PROMPT>

Objective:
<one task and one expected outcome>

User / product reason:
<the user-facing friction, failure, or uncertainty this removes>

Relevant existing context:
- <verified behavior or evidence>
- <important inference, clearly labeled>
- <constraint imposed by the current product or Flare mechanism>

Inspect or use first:
- <files, components, routes, contracts, evidence, or participants>
- <official Flare source required for the task>

Requirements / procedure:
- <specific, checkable requirement or research step>
- <how success, pending, failure, and recovery should behave when relevant>

Constraints:
- <scope, framework, network, deployment, or product constraints>
- Do not modify unrelated functionality.  # include for code tasks

Tests / validation:
- <automated or manual check>
- <evidence that would confirm or reject the product assumption>

Definition of done:
- <observable acceptance criteria>

Stop after completing this task and report the evidence. Do not expand scope.
```

Do not fill a section with generic boilerplate. Remove sections that genuinely do not apply, while preserving objective, reason, evidence, procedure, validation, and definition of done.

## Worked implementation prompt

```text
NEXT IMPLEMENTATION PROMPT

Objective:
Make the deposit screen reflect the real transaction lifecycle from wallet
approval through final confirmation.

User / product reason:
A user can initiate a deposit but cannot tell whether it is pending, confirmed,
or failed. That uncertainty makes a real asset movement feel untrustworthy.

Relevant existing context:
- Verified: the deposit handler submits through the wallet and receives a hash.
- Verified: the hash is logged but not shown in the interface.
- Inferred: the existing notification component can represent the lifecycle.

Inspect or use first:
- deposit form and submit handler
- wallet transaction utility
- notification/state components
- network and explorer configuration

Requirements / procedure:
- show pending immediately after wallet approval;
- derive confirmation from the real transaction result;
- show the resulting position or balance after confirmation;
- preserve the transaction hash and link to the configured explorer;
- handle wallet rejection, network mismatch, RPC timeout, and revert;
- offer retry only when retrying is safe.

Constraints:
- preserve existing accounting and contract calls;
- do not mark a transaction successful before confirmation;
- do not modify unrelated functionality.

Tests / validation:
- automated tests for pending, confirmed, rejected, timeout, and reverted states;
- a first-time user can identify the transaction state without opening an explorer.

Definition of done:
- every submitted deposit reaches an accurate pending, confirmed, or failed UI state;
- the hash remains retrievable;
- existing deposit behavior and tests still pass.

Stop after completing this task and report the evidence. Do not expand scope.
```

## Worked validation prompt

```text
NEXT VALIDATION PROMPT

Objective:
Determine whether XRP holders understand the product promise and can predict
what will happen before they approve the first transaction.

User / product reason:
The current review cannot distinguish a weak interface from a promise that the
target user does not value or understand. Building more transaction UI before
resolving that uncertainty may waste work.

Relevant existing context:
- Verified: the landing page leads with protocol names before the user outcome.
- Claimed: the target user is an XRP holder who does not use EVM wallets.
- Unavailable: no user-test or completion evidence was supplied.

Inspect or use first:
- current landing page and onboarding flow
- five participants who match the stated target user

Requirements / procedure:
- show the product without explanation;
- ask each participant what they think it does and what they expect after the
  main action;
- let them attempt the core flow without coaching;
- record points of confusion and whether they can explain Flare's role afterward.

Constraints:
- do not teach the product before the task;
- do not recruit only blockchain developers;
- do not change the interface during the five-session run.

Tests / validation:
- record comprehension and completion for every participant;
- identify repeated confusion rather than isolated preferences.

Definition of done:
- evidence shows whether the promise, first action, and expected outcome are
  understood;
- the next product decision follows from the observed pattern.

Stop after completing this task and report the evidence. Do not expand scope.
```
