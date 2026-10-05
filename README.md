# Skills

Free skills for people who build with AI. I use these exact files every day
to run a marketing agency on AI agents. More at
[joeyfarbstein.com/skills](https://joeyfarbstein.com/skills).

| Skill | What it does | Lives in |
|---|---|---|
| **raise-the-bar** | The AI can't grade its own work. Before it says "done", a fresh critic that sees only your ask and the finished work says SHIP or NOT YET, and names the one thing to fix. Three rounds at most. | this repo |
| **hatch** | Turns a fuzzy idea or risky decision into a clear, defended result. Six stages: frame, diverge, judge, red-team, build, ship-gate. | [ThetaOneMarketing/hatch](https://github.com/ThetaOneMarketing/hatch) |

## Install raise-the-bar

Copy one file into your project:

```
npx skills@latest add ThetaOneMarketing/skills --skill=raise-the-bar
```

Or install it as a Claude Code plugin that updates when I ship:

```
claude plugin marketplace add ThetaOneMarketing/skills && claude plugin install raise-the-bar@joey
```

Then finish something and type **raise the bar**.

## Install hatch

```
claude plugin marketplace add ThetaOneMarketing/hatch && claude plugin install hatch@hatch
```

Then type `/hatch:hatch <your problem>`. If you already added this
marketplace, `claude plugin install hatch@joey` gets you the same skill.

## License

MIT. Credits are in [ATTRIBUTION.md](ATTRIBUTION.md).

Made by [Joey Farbstein](https://joeyfarbstein.com).
