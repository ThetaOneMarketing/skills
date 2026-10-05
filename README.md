# LocalPro Skills

Two skills for Claude Code that make the agent think harder before it calls
something done.

| Skill | What it does |
|---|---|
| **hatch** | Turns a fuzzy problem or risky decision into a shipped, defended result. Six stages: frame, diverge, judge, red-team, build, ship-gate. Each stage also runs alone. |
| **raise-the-bar** | Before the agent reports "done", a fresh critic that sees only your original ask and the finished work says SHIP or NOT YET. The agent fixes the one biggest gap and loops, three rounds at most. |

## Install

**Option A: as a Claude Code plugin.** Run these inside Claude Code:

```
/plugin marketplace add ThetaOneMarketing/public-skills
/plugin install hatch@localpro-skills
/plugin install raise-the-bar@localpro-skills
```

Install one or both. Restart Claude Code if the skills do not show up.

**Option B: copy the folders.** Works anywhere that reads `~/.claude/skills`:

```
git clone https://github.com/ThetaOneMarketing/public-skills.git
mkdir -p ~/.claude/skills
cp -r public-skills/plugins/hatch/skills/hatch public-skills/plugins/raise-the-bar/skills/raise-the-bar ~/.claude/skills/
```

## Use

```
/hatch <your problem>                 full chain, standard depth
/hatch quick <problem>                ~4 subagents, no council
/hatch deep <problem>                 up to ~25 subagents, live verification
/hatch frame | diverge | judge | redteam | gate <input>
```

Plain words work too: "hatch this, quick", "just red-team it", "raise the
bar on this before you call it done".

raise-the-bar also fires on its own when the agent finishes a real
deliverable (a page, a report, a code change). It skips chat answers and
trivial edits.

## Cost

Both skills spend extra model calls on purpose. A standard hatch run is
about 20 subagent calls; raise-the-bar is 1 to 3. Use the single hatch verbs
when you do not need the whole chain.

## Credits

The methods behind these skills come from people who published them first.
See [ATTRIBUTION.md](ATTRIBUTION.md).

Made by Joey Farbstein, [LocalPro Solutions](https://localprosolutions.com).
