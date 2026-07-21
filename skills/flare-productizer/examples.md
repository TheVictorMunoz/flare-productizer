# flare-productizer — Example Review Scenarios

Illustrative, abbreviated outputs showing the expected shape and honesty. Real reviews follow the full structure in `SKILL.md`. Details below are fictional and for format guidance only.

---

## Example 1 — Flare Smart Accounts / XRP product

**Prompt:** "Use flare-productizer to review this repo and demo. Product: an XRP yield onboarding app. Target user: XRP holders unfamiliar with EVM. Demo: https://example.com. Evaluate the product and Flare integration. Don't make changes; give me the next implementation prompt."

**Product Reconstruction (excerpt):** Web app; connect XRPL wallet → resolve a Flare Smart Account → deposit XRP into an FXRP vault → view position. Vault deposit path *verified* in the contract calls; APY figure is *claimed* (hard-coded in the frontend, not read on-chain).

**Core User Flow finding:** After wallet approval there is no pending state — the UI jumps straight to "success" before confirmation. On RPC timeout the user sees nothing. Core-flow completion by a first-timer is doubtful.

**Flare Integration Review:**
- **Flare Smart Accounts — ESSENTIAL.** The entire value prop is "XRP holders act on Flare without touching EVM"; the XRPL-controlled account removes exactly that friction. Verified against `flare-smart-accounts`.
- **FAssets/FXRP — ESSENTIAL.** Deposits mint/route FXRP; without it there's no yield asset.
- **FTSO — USEFUL BUT REPLACEABLE.** Used only to display a USD price label; no decision depends on it.

**Biggest gap:** trustworthy transaction lifecycle. Users move real XRP but can't tell if it worked.

**Next Implementation Prompt:** (single task) implement pending/confirmed/failed states + real on-chain APY read for the deposit flow — full template filled per `reference.md`. Not executed.

---

## Example 2 — FCC / Confidential Compute product

**Prompt:** "Productize this Flare confidential-compute app. Target user: insurers wanting parametric payouts. Screenshots attached."

**Demo honesty note:** if the environment cannot open the attached screenshots, state that and ask for a short transcript or a supported-format image before assessing the UI.

**Product Reconstruction (excerpt):** A TEE extension computes a payout decision from a private policy and signs it; an on-chain `InstructionSender` routes the instruction. Extension build present; attestation/reproducible-build step *inferred*, not verified.

**Flare Integration Review:**
- **FCC — ESSENTIAL.** The payout logic and policy parameters must stay confidential and be verifiably executed; ordinary smart-contract execution would expose the policy. Verified against `flare-fcc`.
- **FDC — MEANINGFUL.** Weather/event data is attested to trigger payouts; strengthens trust but a single oracle could theoretically substitute.

**Technical readiness gap:** no evidence the deployed code hash matches an attested reproducible build — the core trust claim is unverified. That's the must-fix.

**Next Implementation Prompt:** add and document attestation verification (code-hash check) in the payout path, with a failing-attestation rejection state. Template filled; not executed.

---

## Example 3 — Generic Flare prototype with a superficial integration (the skill must be willing to say so)

**Prompt:** "Review my Flare hackathon project and tell me if the Flare integration is meaningful."

**Product Reconstruction (excerpt):** A to-do/notes app with wallet login. The only Flare touchpoint: on save, it reads an FTSO price feed and stores the current FLR/USD price alongside the note. Nothing in the product uses that value.

**Flare Integration Review:**
- **FTSO — SUPERFICIAL.** *Verified:* the feed read exists but its output is never used in any decision, display the user cares about, or product outcome. Removing the FTSO call would not change the user experience at all. This is technology showcased, not a product capability. Do **not** manufacture a justification — the honest finding is that this app has no meaningful Flare integration yet.

**Prototype → Product Gap — BIGGEST GAP:** there is no user problem for which Flare is the answer. Before more UX work, decide whether Flare is actually part of this product. Two honest paths: (a) drop the Flare integration and ship a normal notes app, or (b) find a real job-to-be-done where a Flare primitive is load-bearing (e.g. verifiable timestamping via FDC, or token-gated shared notes) and rebuild around it.

**Prioritization — Must Fix:** decide the product's actual reason to be on Flare; everything else is premature.

**Next Implementation Prompt:** framed as a *decision + spike*, not polish — e.g. "prototype one FDC-attested feature that a target user would actually pay attention to, behind a flag, without removing existing notes functionality" — template filled; not executed. If the user chooses path (a), the prompt instead removes the unused FTSO call cleanly.
