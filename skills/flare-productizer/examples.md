# flare-productizer example reviews

These abbreviated fictional examples demonstrate reasoning and output shape. They are not claims about real deployments or substitutes for inspecting a repository and current official Flare sources.

## Example 1: XRP onboarding through FAssets and Smart Accounts

**Request**

> Use flare-productizer to review this repository and demo. The product helps XRP holders enter a yield position without setting up an EVM wallet. Do not make changes. End with the highest-priority next-task prompt.

**Current product, excerpt**

The intended flow appears to be:

```text
Connect XRPL wallet → derive or resolve the user's Flare Smart Account
→ send XRP through the FAssets minting path → receive FXRP
→ route FXRP into a vault → view the resulting position
```

- `verified`: the repository derives the user's Smart Account and contains the vault deposit call.
- `claimed`: one XRP payment completes minting and vault entry.
- `unavailable`: the supplied demo could not be opened, so the end-to-end interaction was not observed.

**Core-flow finding**

After wallet approval, the interface marks the action successful before the external payment, minting, and vault position are confirmed. The user cannot distinguish waiting from failure.

**Flare integration review**

- `Flare Smart Accounts — ESSENTIAL`: the product promise is that an XRPL user controls activity on Flare without first adopting a separate EVM account and gas workflow. Removing Smart Accounts breaks that promise.
- `FAssets / FXRP — ESSENTIAL`: XRP must become a usable asset on Flare before entering the vault. Removing FAssets removes the asset path.
- `FTSO — USEFUL BUT REPLACEABLE`: the feed supplies a USD estimate, but no product decision or transaction rule depends on it.

Each mechanism and the repository's use of it must still be checked against the current official skills and docs.

**Prototype → product gap**

`BIGGEST GAP`: the product cannot yet prove or communicate the real state of the user's asset movement. For a flow involving XRP, FXRP, and a vault position, premature success is a trust failure.

**Next task**

A `NEXT IMPLEMENTATION PROMPT` covering only the real pending, confirmed, failed, and recovery states for this flow. Do not bundle unrelated APY, visual redesign, or portfolio work into the same prompt.

---

## Example 2: Confidential Compute for a private decision

**Request**

> Productize this Flare Confidential Compute prototype. The stated user is an insurer running parametric payout logic over private policy data. Review the attached screenshots if accessible.

**Current product, excerpt**

- `verified`: the repository builds a TEE extension and routes a signed instruction to an onchain receiver.
- `claimed`: private policy inputs remain confidential through the deployed path.
- `inferred`: the intended trust model relies on attestation and a reproducible extension build.
- `unavailable`: screenshots were not inspectable in the current environment, so no UI conclusion is made.

**Flare integration review**

- `FCC — ESSENTIAL`: the claimed product requires private inputs plus evidence about which computation ran. Public smart-contract execution would expose the inputs or logic. The conclusion remains conditional until the deployed measurement and verification path are confirmed.
- `FDC — MEANINGFUL`, if the implementation actually uses an FDC-supported attestation to prove the triggering external event. If the repository only mentions FDC without requesting and verifying a proof, classify it as claimed or superficial rather than implemented.

**Prototype → product gap**

`BIGGEST GAP`: no verified link between the extension users trust, the attested code measurement, and the deployed execution path. Without that chain, “verifiable confidential execution” is a pitch rather than a demonstrated product property.

**Next task**

A `NEXT TECHNICAL SPIKE PROMPT` that traces the build artifact, measurement, attestation, registry/deployment record, and rejection behavior. Convert it to an implementation prompt only after the uncertainty is resolved.

---

## Example 3: a superficial Flare integration

**Request**

> Review my Flare hackathon project and tell me whether the Flare integration is meaningful.

**Current product, excerpt**

The repository is a notes application with wallet login. When a note is saved, it reads a FLR/USD feed and stores the value beside the note. Nothing displays the value later or uses it in a decision.

- `verified`: the feed call exists and its output is stored.
- `verified`: removing the stored value would not change the current note workflow.
- `unavailable`: no target-user research or usage evidence was supplied.

**Flare integration review**

`FTSO — SUPERFICIAL`: the call demonstrates that the developer can read a feed, but it does not create a user capability or change a product outcome. More feed usage is not automatically the answer.

**Prototype → product gap**

`BIGGEST GAP`: the project has no demonstrated user problem for which Flare is load-bearing. UX polish cannot resolve that product uncertainty.

**Priorities**

1. Decide whether the product should exist as a normal notes app or whether there is a real target-user problem that requires verifiable external data, onchain ownership, confidential computation, or another specific mechanism.
2. Do not choose a protocol first and invent a use case afterward.
3. Do not implement another Flare feature until evidence identifies the missing capability.

**Next task**

A `NEXT VALIDATION PROMPT`, not an implementation prompt: interview or test with the stated target user, document the job they cannot accomplish with existing tools, and determine whether any Flare property is necessary. If no necessary property emerges, remove the decorative integration rather than forcing one.
