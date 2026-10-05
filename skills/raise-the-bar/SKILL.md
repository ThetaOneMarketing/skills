---
name: raise-the-bar
description: Autonomous self-critique loop. When a substantial deliverable (file, page, report, code change, campaign asset, doc) is finished and about to be reported done, run this first - spawn a fresh-context harsh critic that sees only the original ask and the finished output, get a binary SHIP / NOT YET verdict plus the single biggest gap, fix it, and loop (max 3 rounds) until it ships. Also triggers on "raise the bar", "self-gauntlet", "judge your own work", "critique yourself", "check your own work before calling it done".
---

# Raise the Bar

You just finished and it feels great. That feeling is the trigger, not the exit. Before you report done, make the work survive a judge who doesn't care how hard you tried.

Adapted from the gauntlet-loop pattern (Matt Shumer). Same core: fresh-context critic, blind of effort, binary pick, loop on the single biggest gap. Simplified: no external bar to fetch — the bar is the original ask read literally, plus what a top professional in this domain would flag.

## The loop

1. **Freeze the ask.** Copy the user's original request verbatim. This is the contract. The critic judges against it — not against your interpretation of it.
2. **Spawn a fresh critic.** A subagent with clean context. It gets ONLY two things: the frozen ask, and the finished output itself (the actual file, page, diff, or answer). Never the conversation, never your process, never how hard you tried.
3. **The critic's job** (put this in its prompt):
   - Read the ask literally. List anything asked for but missing, and anything claimed but not verified.
   - Judge as the intended audience — the person who will actually use this. Harsh. Praise is not useful.
   - Verdict: **SHIP** or **NOT YET**. Binary. No scores out of 10 — scores drift upward every round.
   - If NOT YET: name the single biggest gap. One. A second only if it is nearly tied.
4. **Fix the biggest gap.** Just that one. Not a laundry list — a laundry list dilutes into cosmetic edits.
5. **Loop.** Back to step 2 with a NEW fresh critic. The old critic never re-judges; it gets attached to its own advice.

## Exit

- Critic says SHIP → report done, plus 1–2 lines on what the loop actually changed.
- 3 rounds and still NOT YET → stop. Report done-with-gaps and list the unfixed gaps plainly. Never hide them, never silently keep burning rounds.
- The user interrupting always wins.

## Rules

- **Critic sees output only.** If the output is a page, it opens the page. If it's code, it reads the diff and runs what's runnable. Judging a description of the work is hallucinated approval.
- **Unverified claims are automatic gaps.** If the ask needed a live check (screenshot, read-back query, a form that submits), an unverified "it works" is the biggest gap by default — before any taste critique.
- **Judge the ask, not your ambitions.** The critic cannot invent new scope. Ideas beyond the ask go in the final report as suggestions, never get built inside the loop.
- **Fresh means fresh.** New subagent each round. No shared memory with the builder or the previous critic.
- **Skip it entirely for:** prose answers to questions, opinions, trivial edits, anything the user asked to see raw or fast. This loop is for deliverables, not conversation.

## Critic prompt template

Adapt wording each time; keep the verdict binary.

```
You are a harsh reviewer with no stake in this work. Here is the original request, verbatim: [ASK]. Here is the finished output: [OUTPUT / path / URL].

Open the actual output. Judge as [the intended audience]. Check the request line by line: what was asked but not delivered, what is claimed but not verified.

Verdict: SHIP or NOT YET. If NOT YET, name the single biggest gap — the one fix that most changes whether this is accepted. Do not list more than two. Do not score. Praise is not useful.
```

## What breaks it

- **Self-judging inline.** "Let me review my own work... looks good" is the failure this skill exists to kill. The critic must be a separate fresh context.
- **Scores.** 7/10 becomes 8/10 becomes ship. Binary only.
- **Fixing everything the critic said.** Fix the biggest gap, re-judge. Small gaps often dissolve after the big one is fixed.
- **Letting the loop expand scope.** Round 3 should be judging the same ask as round 1.
- **Hiding a NOT YET.** If it exits on the round cap, the gaps go in the report. Rounding up to "done" defeats the whole skill.
