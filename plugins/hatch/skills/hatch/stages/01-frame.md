# Stage 1 — Frame

**Consumes:** the raw ask.
**Produces:** The Brief.
**Gate:** `gate:framed`.
**Subagents:** none. This stage is a conversation.

## Why

Most bad outcomes are not bad answers. They are good answers to a question
nobody locked down. Frame exists so every later stage argues about the same
thing.

## The method: a design tree, worked in rounds

Treat the problem as a tree. The root is the ask. Every decision under it
branches into the decisions that depend on it. You cannot sensibly ask "which
database" before "does this need to persist at all".

The **frontier** is the set of questions whose prerequisites are already
settled. Ask the whole frontier in one round. Number each question. Give your
recommended answer under each. Then stop and wait.

Each answer reshapes the tree. Settled questions push the frontier outward.
Recompute it. Ask the next round. A question that depends on another question
still open in this round belongs to the next round, not this one.

Format every question exactly like this:

```
Q1 — <short title>
<the question; give choices when choices exist>
→ Recommended: <your pick and a one-line reason>
```

## Rules

- **Facts are yours to find.** If a question needs a fact from the
  environment (does the repo already have X, what does the API return, what
  does the current page look like), look it up or dispatch a read-only
  subagent. Do not block the round on it: ask the rest of the frontier now,
  and hold only the questions downstream of the lookup.
- **Decisions are the user's.** Anything that is a judgment call about what
  they want, put to them. Never silently assume.
- **Recommend every time.** A question with no recommended answer is lazy.
  The user should be able to reply "all recommended" and move on.
- **Round caps by depth.** Quick: one round, three questions max, or zero
  rounds if the ask is already unambiguous (write the Brief, show it, ask for
  a one-word confirm). Standard: up to three rounds. Deep: until the frontier
  is empty.
- **Done means empty.** The stage ends when there is nothing left on the
  frontier, or the round cap hits. If the cap hits with questions still open,
  list them in the Brief under "open judgment calls". Later stages treat those
  as unknowns, not as assumptions.

## The Brief

Write it to the run file the moment the last round closes. This exact shape:

```
## Brief

**Problem (one sentence):** …
**Who it is for:** …
**Constraints:** (bullets — time, money, stack, compatibility, people)
**Definition of done:** (bullets — observable, checkable)
**Out of scope:** (bullets)
**Open judgment calls:** (bullets, or "none")
**Locked on:** <date>, after <n> rounds
```

Ask for a one-line confirm before proceeding to Diverge unless the user said
"auto". "Looks right" is enough. If they edit it, the edit wins.
