# flare-productizer — Reference

Long source lists, skill selection, and the implementation-prompt template. Keep this out of `SKILL.md` so the core instructions stay followable.

## 1. Official Flare sources (source of truth)

Resolve every Flare technical claim in this order, and state which tier you used.

### Tier 1 — Installed official Flare AI Skills

Repo: `https://github.com/flare-foundation/flare-ai-skills`. Six skills, each with `SKILL.md` + `reference.md`:

| Skill | Covers |
|---|---|
| `flare-general` | networks (Mainnet/Coston2/Songbird/Coston), RPC endpoints, explorers, faucets, chain IDs, contract resolution via `FlareContractRegistry`, FSP, tooling, WNat |
| `flare-ftso` | FTSOv2 price/data feeds — consuming feeds on/off-chain, feed IDs, delegation |
| `flare-fdc` | Flare Data Connector — attestation types, verifier + DA-layer endpoints, `FdcVerification`, proof round-trips |
| `flare-fassets` | FXRP / FBTC / FDOGE — minting, redemption, vaults, agents, collateral |
| `flare-smart-accounts` | account abstraction for XRPL users — XRPL-controlled accounts acting on Flare |
| `flare-fcc` | Confidential Compute — TEE extensions, `TeeExtensionRegistry`/`TeeMachineRegistry`, `InstructionSender`, attestation, reproducible builds |

If the relevant skill is **not installed**, note it, fall back to Tier 2/3, and recommend installing it (see README for commands).

### Tier 2 — Flare Developer Hub

`https://dev.flare.network/`. **Append `.md` to any page** for agent-ready markdown. Discover the corpus via `https://dev.flare.network/llms.txt`.

Useful entry points (all support `.md`):

- Network getting started — `https://dev.flare.network/network/getting-started`
- FTSOv2 — `https://dev.flare.network/ftso/getting-started`
- FDC — `https://dev.flare.network/fdc/getting-started`
- FAssets developer guides — `https://dev.flare.network/fassets/developer-guides`
- Smart Accounts guides — `https://dev.flare.network/smart-accounts/guides`
- FCC getting started — `https://dev.flare.network/fcc/guides/getting-started`

### Tier 3 — flare-foundation GitHub

`https://github.com/flare-foundation` — current README/docs/code. Starters worth knowing: `flare-hardhat-starter`, `flare-foundry-starter`. Prefer the live repo over memory.

### Tier 4 — clearly-labeled secondary info

Only if strictly necessary, and always labeled as secondary + verified.

**Never** invent protocol behavior, contract addresses, deployment facts, supported functionality, or maturity status. When uncertain or possibly stale, say so and verify.

## 2. Which skill to consult (auto-select)

- price/market/risk/automation decisions driven by a feed → `flare-ftso`
- verifying an external chain/web event or state → `flare-fdc`
- wrapping/minting/redeeming XRP/BTC/DOGE, vaults, agents → `flare-fassets`
- XRPL wallet controlling actions on Flare, gasless/abstracted UX for XRPL users → `flare-smart-accounts`
- secrets/keys/strategy/computation needing confidentiality or verifiable execution in a TEE → `flare-fcc`
- networks, RPCs, contract registry, general architecture → `flare-general`

A project can need several (e.g. an FXRP yield app may touch `flare-smart-accounts` + `flare-fassets` + `flare-ftso`).

## 3. Evidence tagging

Tag every non-trivial claim:

- **verified** — observed directly in code or in the demo you actually inspected.
- **inferred** — reasoned from structure/naming/config, not directly confirmed.
- **claimed** — asserted by README/docs/pitch but not confirmed in code or demo.

Keep these distinct in the write-up. Traction and product-market fit require **verified** evidence, never **claimed**.

## 4. NEXT IMPLEMENTATION PROMPT — template

Fill this in for the single highest-priority task. Copy-ready; the user pastes it into their coding agent. **Do not execute it.**

```
NEXT IMPLEMENTATION PROMPT

Objective:
<one crisp sentence describing the single task>

User / product reason:
<why this matters from the user's perspective — the friction or failure it removes>

Relevant existing context:
<what already exists that the task builds on; verified vs inferred>

Inspect first:
- <likely files / components inferred from the repo>
- <relevant Flare integration points>

Implementation requirements:
- <specific, testable requirements>
- <required Flare-native behavior verified against official sources>

Required error / edge states:
- <pending / confirmed / failed lifecycle, retries, network checks, empty/timeout states>

Constraints:
- Do not modify unrelated functionality.
- <framework/version/deploy constraints observed in the repo>

Tests / validation criteria:
- <automated tests to add or update>
- <manual/user validation, proportional to stage>

Definition of done:
- <concrete acceptance criteria a reviewer can check>

Do not modify unrelated functionality.
```

### Worked example (style reference)

```
NEXT IMPLEMENTATION PROMPT

Objective:
Make the deposit flow communicate the full transaction lifecycle without a block explorer.

User / product reason:
Users can initiate a deposit but can't tell whether it is pending, confirmed, or
failed, so they don't trust that their XRP actually moved.

Relevant existing context:
Deposit is triggered in the frontend deposit component and submitted via the wallet;
the tx hash is currently logged to console only (verified).

Inspect first:
- the deposit form / submit handler component
- the wallet/tx submission utility
- any existing toast/notification module

Implementation requirements:
- explicit pending state shown immediately after wallet approval
- confirmed state showing the resulting position/balance
- actionable failure state with a plain-language reason
- retry path where safe to retry
- preserve and surface the transaction hash (with explorer link)

Required error / edge states:
- user rejects in wallet; RPC timeout; tx reverts; network mismatch

Constraints:
- Do not alter unrelated deposit accounting or contract calls.

Tests / validation criteria:
- unit tests for each lifecycle state transition
- a first-time user can complete a deposit and correctly identify success/failure
  without external instructions

Definition of done:
- all three states (pending/confirmed/failed) render from real tx status
- tx hash always retrievable from the UI
- no regressions in the existing deposit path

Do not modify unrelated functionality.
```
