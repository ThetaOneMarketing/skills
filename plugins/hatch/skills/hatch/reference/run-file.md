# Run-file template

Path: `./.hatch/<slug>.md` in the project root. Create `.hatch/` if missing.
Slug: a few lowercase words joined by hyphens, from the ask.

Write the header the moment the run starts. Append each stage's section the
moment that stage's gate closes. If the session is compacted, reopen this
file and continue from the last closed gate.

```markdown
# Hatch run — <slug>

**Started:** <ISO date>
**Depth:** quick | standard | deep
**Mode:** chain | <single verb>
**Ask (verbatim):** "<the user's words>"

## Brief
<from stage 1 — or "skipped: single-stage run">

## Wide set
<from stage 2>

## Shortlist
<from stage 2>

## Verdict
<from stage 3>

## Risk sheet
<from stage 4>

## Built
<from stage 5>

## Decisions
- <date> — <call> — <reason>

## Ship-gate
<from stage 6>

## Gates closed
- [ ] gate:framed
- [ ] gate:shortlisted
- [ ] gate:decided
- [ ] gate:risked
- [ ] gate:built
- [ ] gate:shipped
```

Tick each gate in the checklist as it closes. That checklist is what a
resumed session reads first.
