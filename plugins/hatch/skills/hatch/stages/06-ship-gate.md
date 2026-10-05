# Stage 6 — Ship-gate

**Consumes:** the user's original ask, frozen verbatim, and the finished
deliverable.
**Produces:** SHIP, or done-with-gaps.
**Gate:** `gate:shipped`.
**Subagents:** one fresh critic per round, up to three rounds.

## Why

The moment the work feels finished is the moment judgment is worst. The
builder is attached. So the builder does not judge. A stranger does — one who
has never seen the conversation, never seen the effort, and only cares
whether the ask was met.

## The loop

1. **Freeze the ask.** Copy the user's original request, word for word, into
   the run file. This is the contract. Not your reading of it. Theirs.

2. **Spawn a fresh critic.** New subagent, clean context. It receives exactly
   two things: the frozen ask, and the deliverable itself — the file path, the
   URL, the diff. Never the run file. Never the conversation. Never a
   description of the work in place of the work.

3. **The critic's brief** (verbatim in `reference/prompts.md`):
   - Open the actual output. Read the file, load the page, run what runs.
   - Read the ask literally. List what was asked for but is missing, and what
     is claimed but not shown.
   - Judge as the person who will actually use this. Harsh. Praise is noise.
   - Verdict: **SHIP** or **NOT YET**. No scores.
   - If NOT YET: the **single** biggest gap. A second only if it is nearly
     tied. Never a list.

4. **Fix the one gap.** Just that one. Smaller gaps often dissolve once the
   big one is fixed, and a list of fixes turns into cosmetic shuffling.

5. **Loop with a new critic.** The previous critic never re-judges. It is
   attached to its own advice now.

## Exit

- **SHIP** → report done. Add one or two lines on what the loop actually
  changed, so the user sees the gate did work.
- **Three rounds, still NOT YET** → stop. Report done-with-gaps. List the
  unfixed gaps plainly at the top of the report, not buried. Never round up.
- **The user interrupts** → the user wins.

## Rules

- **Unverified claims are automatic gaps.** If the ask needed a live check —
  a screenshot, a query read-back, a form that submits — and it was not done,
  that is the biggest gap before any taste critique.
- **The critic judges the ask, not your ambitions.** It cannot invent scope.
  Ideas beyond the ask go in the final report as suggestions and never get
  built inside the loop.
- **Round three judges the same ask as round one.**
- **Skip this stage entirely** for prose answers, opinions, and trivial edits.
  It is for deliverables.

## Write to the run file

```
## Ship-gate
**Frozen ask:** "<verbatim>"
**Round 1:** <SHIP | NOT YET — gap: …> → <fix applied>
**Round 2:** …
**Result:** SHIP | done-with-gaps: <list>
```
