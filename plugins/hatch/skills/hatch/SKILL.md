---
name: hatch
description: Turn a fuzzy problem, half-baked idea, or risky decision into a shipped, defended result. Six stages, each also a standalone verb — FRAME (interview in rounds until the problem is locked), DIVERGE (parallel isolated minds generate past the obvious), JUDGE (five advisors, blind peer review, one verdict), RED-TEAM (find the assumption that kills it), BUILD, SHIP-GATE (fresh critic, binary verdict, loop on the one biggest gap). Use on /hatch, "hatch this", "hatch <problem>", or whenever the user brings an open-ended design, architecture, product, naming, API, or strategy question where the obvious answer is expensive if wrong. Also answers single-stage asks — "diverge on X", "judge these options", "red-team this plan", "gate this before I ship". Skip for lookups, syntax, one-right-answer questions, and bugs with a known root cause.
---

# Hatch

One model asked once gives the first answer a competent person would give in
thirty seconds. Correct, safe, forgettable. Hatch makes the model do what a
good team does instead: lock the question, generate wide in isolation, argue
it out, hunt for the thing that kills it, build, and then let a stranger judge
the result before anyone says "done".

Six stages. Each one is useful alone. Chained, they turn "I have a vague
idea" into "here is the thing, here is why, here is what could still break".

## The pipeline

| # | Stage | Consumes | Produces | Gate id | Subagents | Human? |
|---|---|---|---|---|---|---|
| 1 | **Frame** | raw ask | **The Brief** — problem in one sentence, constraints, definition of done, out-of-scope, open judgment calls | `gate:framed` | 0 | yes, answers rounds |
| 2 | **Diverge** | Brief | **The Wide Set** — 12–42 ideas, clustered by angle, scored, traps flagged; **Shortlist** of 3 | `gate:shortlisted` | 3–10 | no |
| 3 | **Judge** | Shortlist | **The Verdict** — where advisors agree, where they clash, blind spots caught in review, one pick, one first step | `gate:decided` | 11 | yes, go / no-go |
| 4 | **Red-team** | Verdict + plan | **The Risk Sheet** — most dangerous assumption, ranked findings, three questions that cut risk most | `gate:risked` | 0–1 | no |
| 5 | **Build** | Brief + Verdict + Risk Sheet | the deliverable + decisions log | `gate:built` | as needed | no |
| 6 | **Ship-gate** | frozen ask + deliverable | **SHIP** or **done-with-gaps** | `gate:shipped` | 1–3 | no |

Read only the stage file you are about to run. Never load all six.

| Stage | File |
|---|---|
| Frame | `stages/01-frame.md` |
| Diverge | `stages/02-diverge.md` |
| Judge | `stages/03-judge.md` |
| Red-team | `stages/04-redteam.md` |
| Build | `stages/05-build.md` |
| Ship-gate | `stages/06-ship-gate.md` |
| Cognitive frames for Diverge | `reference/frames.md` |
| The five advisors for Judge | `reference/advisors.md` |
| Every subagent prompt, verbatim | `reference/prompts.md` |
| Run-file template | `reference/run-file.md` |
| An illustrative sample of each stage's output | `examples/cli-config-store.md` |

## How to invoke

Full chain, default depth:

    /hatch <problem>
    hatch this: <problem>

Depth dial (prefix or say it in words):

    /hatch quick <problem>      ~4 subagents, one round of questions, no council
    /hatch <problem>            standard, ~20 subagents
    /hatch deep <problem>       up to ~25 subagents, verification in red-team, all gates

Single stage (the verbs). Each starts from whatever the user hands it:

    /hatch frame <fuzzy ask>          → The Brief only
    /hatch diverge <problem>          → The Wide Set + Shortlist only
    /hatch judge <2–4 options>        → The Verdict only
    /hatch redteam <plan | file>      → The Risk Sheet only
    /hatch gate <deliverable | path>  → SHIP / NOT YET only

Flags, in words or as `--auto`:

- **auto** — skip the human go/no-go after Judge. Frame still asks its
  questions unless the ask is already unambiguous.
- **from <stage>** — start mid-chain. "hatch from judge: option A vs B."

Natural language works. "Hatch this quick", "just red-team it", "gate this
before I ship" all map to the above.

## Pre-flight

Before spending a single subagent, check three things. If any is a no, answer
the question directly and stop. Offer Hatch in one sentence at most.

1. **Open-ended?** Would two senior people give different viable answers?
   If there is one canonical answer, abort.
2. **Expensive if wrong?** Architecture, public API surface, naming a real
   product, a schema, a positioning choice, a plan about to be committed to.
   A throwaway script is not this.
3. **Open phrasing?** "Quick", "standard", "just", "the normal way" mean the
   user wants a direct answer, not a search.

If the user typed `/hatch` or said "hatch", skip pre-flight. They opted in.

## The ten laws

These are why the chain works. Break one and the stage collapses into a
single wider thought wearing a costume.

1. **Generators never judge. Judges never generate.** Two separate passes,
   two separate prompts. The critic strangles the generator if they share a
   context.
2. **Parallel branches are blind to each other.** Spawn them in one message,
   give none of them another's output. Branches that see each other anchor
   and the spread collapses.
3. **Reviewers review anonymous work, each from its own seat.** Shuffle the
   letters. A reviewer who knows the author defers to the style instead of
   judging the content. A reviewer with no seat is a clone; five clones give
   one review five times.
4. **The critic is a stranger.** New subagent every round. It sees the frozen
   ask and the actual output. Never the conversation, never the effort.
5. **Facts are the agent's job. Decisions are the human's.** Never ask the
   user something you could look up. Never decide something only they can.
6. **Every stage writes to the run file before the next stage starts.**
   Context gets compacted. Files survive.
7. **One biggest gap per round.** A list of ten dilutes into cosmetic edits.
8. **Gate verdicts are binary.** SHIP or NOT YET. Scores drift upward.
9. **An unverified "it works" is a gap.** If the ask needed a live check, the
   critic treats the missing check as the biggest gap by default.
10. **Stop when the space is mapped.** When new ideas repeat the shape of old
    ones, stop diverging. Do not pad to a number.

## The run file

Every run writes `./.hatch/<slug>.md` in the project root (create the folder
if missing). Template in `reference/run-file.md`. Append each stage's output
under its heading the moment the stage completes. If the session is
compacted, reopen the file and continue from the last completed gate.

Keep `.hatch/` in the repo if the team wants a decisions log. Gitignore it if
not. Either is fine. Losing it mid-run is not.

## Depth, in numbers

| Depth | Frame | Diverge | Judge | Red-team | Ship-gate | Subagents |
|---|---|---|---|---|---|---|
| quick | 1 round, ≤3 questions, or none if clear | 3 frames × 4 ideas, pick inline | skipped, orchestrator picks | inline, 3 lines | 1 round | ~4 |
| standard | ≤3 rounds | 5 × 6, deepen top 3 | 5 advisors + 5 reviews + chair | 1 fresh auditor | ≤3 rounds | ~20 (23 max) |
| deep | until frontier empty | 7 × 6, deepen top 3 | same | 1 auditor + live verification | ≤3 rounds | up to ~25 |

Model routing, when the harness allows it: cheapest capable model for
generators, advisors, and reviewers (they produce volume), strongest model
for the chair, the red-team auditor, and the critic (they produce judgment). In
Claude Code, that is the Agent tool's `model` field.

## Output shape of a full run

Render in this order. The structure is the product; do not collapse it.

1. **Brief** — the locked problem, three to six lines.
2. **Wide set** — clusters labeled by angle, ideas as short phrases, score
   chips like `[N7 V8 F9]`. Traps listed separately with their one-line reason.
3. **Verdict** — agrees / clashes / caught in review / the pick / first step.
4. **Risk sheet** — most dangerous assumption on top, then ranked findings,
   then the three questions.
5. **Deliverable** — what was built, where it is.
6. **Ship-gate** — SHIP, or done-with-gaps with the gaps listed plainly.
7. **Decisions log** — every judgment call made without the user, one line
   each.

Single-stage runs render only their own section.

## Anti-patterns

- **Decorated convergence.** Ten variants of one idea is not width.
- **Weird with no landing.** Thirty absurdities and no shortlist is as useless
  as one safe answer. Always converge, always commit.
- **The chair as vote counter.** If four advisors say go and the fifth has
  the strongest reasoning, the chair sides with the fifth and says why.
- **Self-judging inline.** "Let me review my own work… looks good" is the
  failure Ship-gate exists to kill.
- **Scope creep in the gate.** Round three judges the same ask as round one.
- **Hiding a NOT YET.** Exiting on the round cap means listing the gaps, not
  rounding up to done.
- **Asking the user for facts.** Look it up. Ask them only for decisions.

## Cost honesty

A standard run is roughly twenty subagent calls, twenty-three at the cap
(5 generators + 3 deepeners + 5 advisors + 5 reviewers + 1 chair + 1 auditor
+ up to 3 critics). Deep adds two generators, so twenty-five at the cap. Frame
may add read-only lookups on top. That is five to ten times a single answer. Use it where the obvious answer being wrong costs a day or
more. For everything else, the single verbs exist for a reason.
