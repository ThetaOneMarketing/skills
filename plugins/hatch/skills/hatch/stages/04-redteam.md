# Stage 4 — Red-team

**Consumes:** The Verdict plus whatever plan or sketch describes the pick.
Or, standalone, any plan, spec, or argument the user hands in.
**Produces:** The Risk Sheet.
**Gate:** `gate:risked`.
**Subagents:** quick 0 (inline, three lines), standard 1, deep 1 with live
verification.

## Why

Plans read well whether or not they are true. A model is very good at making
a plan coherent, which is exactly the danger. This stage reads the plan cold,
lists what it assumes without saying, checks what can be checked, and puts
the most dangerous assumption on top so it gets killed or proven before a
week is spent on it.

## Posture

- **Skeptic, not collaborator.** No praise, no "solid overall", no softening.
  Output is findings, ranked.
- **Non-interactive.** Zero questions to the user during the pass. Ambiguity
  in the plan is itself a finding.
- **Anchored.** Every finding quotes or points at the exact line in the plan
  that carries the assumption. A finding you cannot anchor does not ship.
- **Read-only.** Never fix the plan. Never start building. Findings only.
- **Prior reasoning is suspect.** If the plan came from this session, the
  auditor gets a fresh context and re-derives from the plan text. Defending
  earlier output is the failure this stage exists to catch.

## Method

1. **Inventory the claims.** One pass through the plan. List every
   declarative claim and every implicit precondition ("this only works
   if…"). Tag each:
   - *stated and evidenced* — the plan says it and shows why
   - *stated but asserted* — the plan says it, shows nothing
   - *unstated* — required for the plan to work, never written down

   The unstated ones are the payload. Hunt especially for: tool or API
   behaviour assumed, user behaviour assumed, timelines that ignore
   dependencies, "we already have X", and numbers with no source.

2. **Verify what is reachable.** Standard depth: check anything checkable
   from the repo (does the function exist, does the test pass, does the
   config say what the plan says). Deep depth: also check live sources (fetch
   the page, call the endpoint, read the docs). A claim is *verified* only if
   the auditor pulled it from the raw source in this pass. Confidence in the
   plan's prose is not evidence.

3. **Find contradictions.** Places the plan disagrees with itself. Scope says
   one thing, the task list funds three. Timeline says two weeks, the
   dependency list implies six.

4. **Rate and rank.**
   - Load-bearing: **critical** (plan collapses if false) / **major** (a
     phase fails) / **minor** (cosmetic).
   - Evidence: **verified** / **supported** (plan cites evidence the auditor
     did not check) / **asserted** / **contradicted** (auditor checked; it is
     wrong).
   - Order: contradicted-critical, asserted-critical, contradicted-major,
     asserted-major, the rest.

## The Risk Sheet

```
## Risk sheet

**Verdict:** <one line — proceed / fix first / do not commit yet>

### Most dangerous assumption
<stated plainly> — critical, <evidence rating>.
Anchor: "<quote>" (<where>)
Why it leads: <what collapses, and why you would find out late>

### Findings, ranked
1. [<unstated assumption | missing evidence | contradiction | claim-vs-demo gap>]
   <the assumption, plainly>
   Anchor: "<quote>" (<where>)
   Load-bearing: … · Evidence: …
   If false: <concrete failure>
2. …

### Three questions that cut the most risk
1. <question — de-risks finding #n>
2. …
3. …

### Judgment calls no source can settle
<bullets — these go back to the human, via Frame if the run continues>
```

Every section is mandatory. If the plan genuinely survives, the findings list
may be short, but the most dangerous assumption still ships. Every plan has
one.

## Quick depth

No subagent. Three lines, inline: the most dangerous assumption, the one
check that would prove or kill it, and whether to run that check before
building. Then move on.

## Hand-off

In the chain, Build reads the Risk Sheet first and treats the most dangerous
assumption as its first task: prove it or kill it before building anything
that depends on it.
