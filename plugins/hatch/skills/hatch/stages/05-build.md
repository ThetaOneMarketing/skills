# Stage 5 — Build

**Consumes:** The Brief, The Verdict, The Risk Sheet.
**Produces:** the deliverable, and a decisions log.
**Gate:** `gate:built`.
**Subagents:** whatever the work needs. This stage is ordinary engineering.

## Order of work

1. **Kill or prove the most dangerous assumption first.** Before writing
   anything that depends on it, run the check the Risk Sheet named. If the
   assumption dies, stop, write what happened to the run file, and go back to
   Judge with the pick removed. Do not build around a corpse.

2. **Build to the Brief's definition of done.** Not to your interpretation of
   it. Reread the Brief before starting and again before calling it built.

3. **Take the first step the chair named.** It was chosen for a reason.
   Start there.

4. **Log every judgment call.** Anything you decided without the user goes in
   the run file under "Decisions", one line each, as you make it. Not at the
   end.

5. **Verify what you claim.** If the deliverable runs, run it. If it renders,
   open it. If it has tests, execute them and keep the output. The Ship-gate
   critic will treat any unverified claim as the biggest gap. Save yourself
   the round.

## Write to the run file

```
## Built
**What:** <one line>
**Where:** <paths, URLs>
**Verified by:** <what you ran or opened, and the result>

## Decisions
- <date> — <the call, the reason>
- …
```

Then hand the frozen ask and the deliverable to Ship-gate.
