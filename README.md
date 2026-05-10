AI Driven Development
=====================

## Demo App

Modernization run during the presentation.

- Path: C:\Users\woute\Dropbox\Personal\Programming\UnixCode\projects\VierOpEenRij
- Repository: [Laoujin/VierOpEenRij](https://github.com/Laoujin/VierOpEenRij)


## Presentation

```bash
cd presentation
bun install
bun run dev
```

Update the theme:
```bash
cd presentation/theme
git pull
```


## Actionable Items

### The three

1. **Install Superpowers** — clone [obra/superpowers](https://github.com/obra/superpowers) into `~/.claude/skills/`. You get brainstorming, TDD, debugging, code review and plan-writing skills today, no investment required.
2. **Audit your context** — open a session, run `/context` and `/mcp`, unload anything you don't need for the task. Set up `/statusline` so context % stays visible. Plan to `/clear` at 40%.
3. **Write one skill** — pick the thing the agent gets wrong every week. Codify it. That's your first compound-interest payment. Skill anatomy: `~/.claude/skills/<name>/SKILL.md` with frontmatter `name`, `description`. Description is what triggers loading — make it specific.


### Five-minute wins

- Move "no `Co-Authored-By`" to `~/.claude/settings.json` → `attribution.commit: ""` and `attribution.pr: ""`.
- Replace heavyweight MCPs with their CLI: `gh` instead of GitHub MCP, `psql` instead of Postgres MCP. A CLI invocation is zero schema cost; an MCP server adds tokens to every prompt whether you use it or not.
- Default to `/clear` over `/compact`. `/clear` plus a handover prompt keeps the next session sharp.


### Fifteen-minute wins

- Add a `PreToolUse` hook that blocks the mistake your agent actually keeps making — `git push --force` to main, edits to `.env`, dropping migrations. Hooks run deterministically; the model can't talk its way out.
- Add a `PostToolUse` hook that runs `tsc --noEmit` and `eslint --quiet` after every edit. Tool-output shaping: print `✓` on success, full output only on failure. The agent gets the squiggles you get.
- Trim `~/.claude/CLAUDE.md`. Delete anything inferable from the code. Big CLAUDE.md files are harmful — middle-rot eats your most-load-bearing instructions first.
- Add a top-level `MEMORY.md` index pointing at topic files (`debugging.md`, `patterns.md`, …). Only the index is loaded into every context; topic files load on demand.
- Set up code-review fan-out: a separate Claude session reviews diffs from your main session. The model that wrote the code is too invested to review it honestly — same context = shared blind spots.


### Going deeper

- [Block's 3 principles for agent skills](https://engineering.block.xyz/blog/3-principles-for-designing-agent-skills) — descriptive name, scoped trigger, layered detail.
- [Latent Space — *Reviews are dead*](https://www.latent.space/p/reviews-dead) — why one-shot review is a rubber stamp and how multi-agent fan-out catches more.
- [Martin Fowler — *Harness Engineering for Coding Agents*](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) — guides (feed-forward) vs sensors (feedback); the CAR framework.
- [Agent backpressure](https://latentpatterns.com/glossary/agent-backpressure) — LSP, types, tests, lint, screenshots; controlling the flood.
- Search Kieran Klaassen on compounding engineering — the lever the talk is built around.
