# POV: building an AI-powered presentation

link: https://0xrusowsky.github.io/shadow-relay-demo

> [!TIP]
> - **Invest in the first prompt.** Give the goal, the audience, reference links or images, and real source material (READMEs, incident notes).
> - **Then iterate in one-liners.** Move this, rename that, softer, slower, drop a word.
> - **Ask for the copy, edit it yourself, paste it back.** It's the fastest way to get the wording exactly right.

## How the animations were made

### 1. STF animation (ended up being discarded for the sake of time)

```md
Make a short animation about how STF works on Tempo (a blockchain for global payments).

Your objective is to teach the very high-level foundations of blockchain execution (state/execution/STF). The idea is to make it like a gif that can be looped.

Refer to this style. We prefer the dark version.
https://tempo.xyz/developers/ · https://tempo.xyz/developers/blog · https://tempo.xyz/solutions/cross-border-payments/
```

Follow-ups:
- "drop the corner labels, make transitions slightly slower, make sure the inputs don't overlap the header, and write `execution binary` in the central box"
- "rather than `BLOCK B · ORDERED TXS` let's simply write `INPUT`"
- "as soon as txs are processed, drop INPUT"
- "make the frame headers slightly bigger and bolder; drop the `.` at the end of sentences"
- "gimme the copy of each frame, so that I can edit it" *(then pasted my own edited copy)*
- "render `S`, `B`, `S′` and `STF` in a code font"
- "in the consensus frame, change 'should' for 'MUST'"
- "don't show the execution binary box until we explain it in frame 3"

### 2. Gruyère cheese defense animation

```md
Create another animation with the same style to showcase the gruyère cheese defense analogy: how adding layers improves security. Our current layers are:
- unit/e2e/integration tests
- code reviews (manual + agentic)
- re-execution tests, to catch STF-breaking changes on historical loads
- external security audits
- devnet, to catch bugs in the new hardfork (with synthetic loads)
- [we are now adding shadow forks], to catch STF-breaking changes on historical loads + bugs in the new hardfork (with real/live loads)
- testnet (staging) > mainnet (prod)
```

Follow-ups:
- "make testnet and mainnet like the other shapes, since devnet/shadow/testnet/mainnet are actual deployments"
- "drop the `P(escape)` formula, change 'new code' for 'new binary', use line breaks instead of `·` in the footers"
- "I want to better flag the deployments, as they are another set of tests. Box them with a dotted line"
- "gimme the copy of each frame" *(then pasted my edits)*
- "the shadow replayer footer should only say `hardfork / live loads` so it compares easily with devnet; drop the NEW badge"
- "frame 3 should drop STF and simply say 'a user-breaking change'"

### 3. Shadow replay animation

```md
Create another animation with the same style to showcase how our shadow replayer works.
Ref: @shadow-fork-replayer/crates/node/src/shadow_replay/README.md
```

Follow-ups:
- "let's emphasize more that it is a follower node"
- "move the validator set to the bottom, taking 2/3 of the width. Have them generate new blocks on the canonical chain, so the flow validator set > chain > follower is clear. Then the validator set disappears and the follower box takes the full width"
- "to be technically accurate, shouldn't one validator propose the block, and the others vote?"
- "drop the thin data lines, keep only the ball animation"
- "after the state is plugged into the executors, show an `S` in their corner"
- "after each tx executes, update S to S1, S2, S3"
- "for the shadow, show S1′, S2′, S3′ and showcase that we override them with the canonical S1, S2, S3"
- "only show the lines from the parent state to the executors, then hide them"

```md
Modify the shadow replay animation to be tailored to this real example, which is what motivated its creation:

The partner incident was a T11 compatibility regression on September 10, 2026, at 14:00 UTC: strict ABI decoding fixed a DoS vulnerability but also rejected harmless trailing calldata that integrations previously used. Bridge’s padded supply-check calls blocked USDB minting/burning; Relay’s transfers with appended order metadata reverted; Privy was reported affected through Relay, although direct Privy-originated failures were not established.

Relay mainnet examples each have receipt status 0 and a 100-byte transfer(address,uint256) input: 68 standard bytes plus a 32-byte suffix.

Bridge’s reported failure was in RPC supply checks, so that report provides no failed onchain transaction to list. Partners temporarily stripped trailing bytes; the T12 fix restored trailing-byte compatibility while retaining the DoS protections.
```

- "update the compare copy to: `expected`: reject malformed ABI / `finding`: transfer with 32 bytes of trailing metadata"

### 4. T11 incident animation (the "why")

```md
Since the audience is GTM, I think it's best to have an initial slide explaining how we patched the ABI bug where we could get DoSed, but broke a user-facing flow.
```

Follow-ups:
- "this slide is not up to the quality standards of the other ones" *(attached screenshot; it was rebuilt as an animation)*
- "minimize the text in each frame; use unified wording: accepted/rejected"
- "unify the fix and side-effect frames; drop T11/T12 from the frame titles, the node box already shows them"
- *(pasted the final 3-frame copy: Before / The fix / After)*

## How the presentation was finally assembled

```md
Create a new html file which is a presentation that leverages the individual animations. Use these images as reference for the title and chapter slides: *(attached template slide pictures)*

Title: "Hardening our development pipeline with the shadow replayer". Subtitle: "Internal demo".

GOAL: an internal demo for the GTM team to learn about the security practices of engineering's release pipeline. In this case we are demoing the shadow replayer. Guide them through these topics with the animations we created:
1. the T11 incident
2. the classic gruyère cheese security analogy
3. the shadow replayer
```

Follow-ups:
- "skip the agenda and all the explanatory text slides"
- "rename the cheese chapter to 'Security is a game of layers'"
- "add a final slide with this image, and overlay this QR code on it" *(attached)*
- "drop the numbers from the chapter slides"
