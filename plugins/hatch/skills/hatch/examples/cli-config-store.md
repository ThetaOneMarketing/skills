# Sample output — where should the CLI keep its config?

**This is a written sample, not a recorded run.** It shows the shape and
tone of each stage's output so you know what to expect before your first
real run. Nothing below was executed; treat the numbers and file names as
placeholders. A real run writes its own record to `./.hatch/<slug>.md`.

**Ask (verbatim):** "hatch this: our CLI needs to remember the user's API
key and a few prefs between runs. Where should that live?"

---

## Brief

**Problem (one sentence):** Persist one secret and ~5 non-secret preferences
for a cross-platform CLI, with zero setup for the user.
**Who it is for:** Developers on macOS, Linux, Windows; some in CI.
**Constraints:** Node 20+; no native compile step; must work headless in CI.
**Definition of done:** first run prompts once, later runs never prompt; the
secret is not in plain text on disk; `--config` override works in CI.
**Out of scope:** multi-profile support; syncing config between machines.
**Open judgment calls:** none.
**Locked on:** 2026-09-22, after 2 rounds (Q: is CI a real target? → yes.
Q: is plain-text acceptable if file is 0600? → no, user wants better.)

---

## Wide set

### "Use what the OS already has"
- OS keychain via a pure-JS bridge, file fallback in CI [N5 V7 F9]
- Store only a refresh handle; fetch the real key on demand [N7 V6 F8]
- Piggyback on the git credential helper [N8 V5 F7]

### "Make the secret not a secret"
- Short-lived device-code tokens, re-auth silently [N8 V6 F8]
- Encrypt at rest with a key derived from machine identity [N6 V7 F8]

### "Remove the storage entirely"
- Env var only; the CLI prints the export line once [N4 V9 F6]
- Config lives in the project's `.npmrc`-style file, secret via 1Password CLI [N7 V4 F6]

### Traps
- Encrypt with a hard-coded app key — looks safe, is not; a trap.
- SQLite for five prefs — premature; a trap.

## Shortlist
1. ★ Short-lived device-code tokens, silent re-auth — the secret stops being
   worth stealing.
2. OS keychain with pure-JS bridge, plain file fallback under CI env flag.
3. Machine-identity-derived encryption of a JSON file.

---

## Verdict

### Where the advisors agree
- A plain JSON file with the raw key is the wrong answer even at 0600.
- CI needs an explicit, documented override; do not detect it by magic.

### Where they clash
- Skeptic: device-code flow adds a server dependency the CLI did not have.
  Maximalist: that dependency is the feature — it enables revocation and
  team seats later.

### Caught in review
- Nobody addressed what happens when the keychain is locked on a headless
  Mac. Reviewer C flagged it; three advisors had assumed interactive use.

### The pick
Option 1, device-code tokens with silent refresh, **but** ship Option 2's
keychain storage for the refresh token, with the CI file fallback. The chair
sides with the Skeptic's concern by keeping the server dependency behind a
feature flag for the first release.

### The first step
Write the token refresh function and its test before touching storage.

---

## Risk sheet

**Verdict:** fix first.

### Most dangerous assumption
The auth server supports a device-code grant today — critical, asserted.
Anchor: "silent re-auth via device-code" (Shortlist #1)
Why it leads: the whole pick rests on it, and you find out on launch day.

### Findings, ranked
1. [unstated assumption] Keychain access works without a UI session.
   Anchor: "OS keychain … fallback under CI" — major, asserted.
   If false: every headless Mac run hangs on a system prompt.
2. [claim-vs-demo gap] "Zero setup" vs. the CI override that must be set.
   Anchor: Brief, constraints — minor, contradicted.

### Three questions that cut the most risk
1. Does the auth server expose a device-code endpoint? (kills #MDA)
2. Does the keychain bridge return an error or block when locked? (#1)
3. Is "zero setup" meant for humans only? (#2)

### Judgment calls no source can settle
- Whether to require the server dependency at all for v1.

---

## Built
**What:** `src/auth/token.ts` (refresh + test), `src/config/store.ts`
(keychain with `HATCH_CI=1` file fallback).
**Where:** branch `feat/config-store`.
**Verified by:** <what you ran or opened, and the result — a real run
records the actual command and output here>

## Decisions
- 2026-09-22 — feature-flag the device-code path; default off for v1 —
  per the chair, Skeptic's dependency concern.

---

## Ship-gate
**Frozen ask:** "our CLI needs to remember the user's API key and a few
prefs between runs. Where should that live?"
**Round 1:** NOT YET — gap: <the critic's single biggest gap> → <fix>
**Round 2:** SHIP.
**Result:** SHIP. <one line on what the gate changed>
