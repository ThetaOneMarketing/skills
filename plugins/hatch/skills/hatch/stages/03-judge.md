# Stage 3 — Judge

**Consumes:** the Shortlist (or two to four options the user hands in).
**Produces:** The Verdict.
**Gate:** `gate:decided`.
**Subagents:** 5 advisors + 5 reviewers + 1 chair = 11. Skipped at quick depth.

## Why

Ask one model which option is best and you get one perspective with no way
to tell whether it is the strong one. Five advisors who think from
deliberately clashing angles, who then review each other without knowing who
wrote what, produce something a single pass cannot: the places where
independent minds converge, and the places where they genuinely split.

## Step 1 — frame the question, neutrally

Write one framed question all five advisors will receive. It contains:

- the decision, in one sentence
- the options, each in two or three lines (use the deepened sketches)
- the constraints and definition of done from the Brief
- what is at stake if the choice is wrong

Add any workspace context that changes the answer — a `CLAUDE.md`, an
existing architecture doc, a past decision record. Spend a minute, not ten.
Do not add your own lean. The chair decides, not the framer.

## Step 2 — convene, in parallel

Spawn all five advisors from `reference/advisors.md` in one message. Each gets
its identity, the framed question, and the advisor instruction from
`reference/prompts.md`. The instruction tells them: do not balance, do not
hedge, represent your angle as hard as you honestly can, the others cover
the rest. 150–300 words each.

## Step 3 — blind review, in parallel

Collect the five responses. Relabel them A–E in a **shuffled** order. Record
the mapping in the run file and nowhere the reviewers can see.

Spawn five reviewers in one message, **one per seat**. The Skeptic reviews
as the Skeptic, the Reframer as the Reframer, and so on — each reviewer gets
its own advisor paragraph from `reference/advisors.md`, exactly as in Step 2.
That is what makes five reviews different from one review run five times.
Each sees the framed question and all five anonymous responses, and answers
exactly three things:

1. Which response is strongest, and why.
2. Which response has the biggest blind spot, and what it is.
3. What all five missed.

Under 200 words each. A reviewer will usually be reading its own earlier
response among the five without knowing which one it is. That is intended.
The letters stay hidden so it cannot favour itself.

## Step 4 — the chair

One subagent, strongest model available. It receives the framed question,
the five responses with names restored, and the five reviews. It writes The
Verdict in this exact shape:

```
## Verdict

### Where the advisors agree
(points that two or more reached independently — high-confidence signals)

### Where they clash
(genuine disagreements; both sides, and why reasonable people split here)

### Caught in review
(things no single advisor saw that the blind review surfaced)

### The pick
(one option; the reasoning; the chair may side with the minority if its
reasoning is stronger, and must say so when it does)

### The first step
(one concrete action; not a list)
```

## Step 5 — present and pause

Render The Verdict in chat. Write it to the run file. Then stop and ask one
question: **build the pick, build a different option, or send it back to
Frame?** Wait for the answer unless the run is `auto`, in which case proceed
with the pick and log that no human confirmed it.

A `judge`-only run ends here.
