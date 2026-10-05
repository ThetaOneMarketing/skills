# Stage 2 — Diverge

**Consumes:** The Brief.
**Produces:** The Wide Set and the Shortlist.
**Gate:** `gate:shortlisted`.
**Subagents:** quick 3, standard 5 + 3, deep 7 + 3.

## Why

The first three answers are the ones any competent person would give. They
are usually right and never interesting. The useful ideas live past number
three, and a single context cannot get there because it keeps anchoring on
what it already wrote. Isolation is the trick. Different minds, no shared
notes, then one honest sort.

## Two passes. Never mixed.

### Pass A — generate (critic off)

1. Pick frames from `reference/frames.md`. Quick: 3. Standard: 5. Deep: 7.
   When the problem is code-shaped, lean on frames tagged `code` and
   `design`. Always include at least one tagged `wild`. Rotate picks across
   runs so the same problem does not produce the same set twice.

2. Spawn one subagent per frame, **all in the same message, in parallel**.
   Each one receives only: the Brief, the frame's vantage prompt, and the
   generator instruction from `reference/prompts.md`. Nothing else. Not the
   other frames, not your opinions, not prior runs.

3. The generator instruction forbids evaluation, forbids hedging, and bans
   the first three obvious answers. It returns a JSON array of ideas, each a
   phrase or a sentence plus a one-line rationale. Quick: 4 ideas each.
   Standard and deep: 6.

4. If a branch comes back with fewer ideas or with evaluation in it, discard
   the evaluation and keep the ideas. Do not re-run to pad.

### Pass B — sort (critic on)

Do this yourself, inline, after every branch has returned.

1. **Score** each idea on three axes, 0–10:
   - *Novelty* — distance from the default answer.
   - *Viability* — could it actually ship, with this team, this stack, this
     budget (from the Brief).
   - *Fit* — does it solve the problem as stated in the Brief, not a
     neighbouring problem.
   Weighted total = novelty × 0.35 + viability × 0.40 + fit × 0.25.

2. **Flag traps.** An idea that scores well but hides a cost — false economy,
   will not scale, premature abstraction, depends on a thing that does not
   exist — gets a trap flag and a one-line reason. Traps stay visible but
   cannot make the Shortlist.

3. **Cluster** by underlying angle, not by surface words. Three to six
   clusters. Label each by the move it makes: "remove the server", "make it a
   queue", "let the user do it", "change what is being measured".

4. **Shortlist** the top three by weighted total, traps excluded. Mark with a
   star the highest-scoring idea that is *not* in the most obvious cluster.
   That star is the point of the whole stage.

5. **Deepen** (standard and deep only). One subagent per shortlisted idea, in
   parallel, using the deepener instruction. Each returns a four-to-eight
   sentence sketch of how it works, the load-bearing risk, the first concrete
   step, and three to five child ideas.

## Stop rule

When branches start returning ideas that are reshapes of ideas already in the
set, the space is mapped. Do not add frames to hit a count.

## Write to the run file

```
## Wide set
### <cluster label>
- <idea> [N7 V8 F9]
- …
### Traps
- <idea> — <one-line reason>

## Shortlist
1. ★ <idea> — <why it is on the list>
2. <idea> — …
3. <idea> — …

### Deepened
#### <idea 1>
Sketch: …
Load-bearing risk: …
First step: …
Children: …
```

In a `diverge`-only run, render this and stop. In the chain, hand the
Shortlist (with the deepened sketches) to Judge.
