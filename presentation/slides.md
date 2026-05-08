---
theme: ./theme
title: AI Driven Development
subTitle: From Vibe Coding to Agentic Engineering
transition: fade
session-time: 90min
track: AI
type: Theoretical
first: 2026-05-11
---

# AI Driven Development
# From Vibe Coding to Agentic Engineering

::image::

![](./images/cover.png)

---
layout: section
---

# Prompt Engineering

::subtitle::

A short eulogy

---
layout: default
---

# Prompt Engineering — RIP

<v-clicks depth="2">

- 2023: "Prompt Engineer" — six-figure job titles
- 2024: "Just write better prompts" courses everywhere
- 2025: dried up — the prompts moved into the system prompt, the skills, the agent loop
- 2026: the frontier is no longer one magical sentence

</v-clicks>

<!--
- Pause after the 2026 line — let it land
- Don't doom; this is the setup for the ladder
- Anecdote: prompt-engineering bootcamps from 2023 — where are they now?
-->


---
layout: agenda
textSize: sm
items:
  - The Maturity Ladder
  - Building Blocks (system prompts, CLAUDE.md, memory, skills)
  - Superpowers — the fast on-ramp
  - Context Engineering in practice
  - Guardrails — TDD & hooks
  - Code Reviews — the 2026 problem
  - Gallery of Leverage
  - Monday morning
---

---
layout: section
---

# Before we start

::subtitle::

Another Claude is already working

---
layout: default
---

# A second Claude is running right now

<v-clicks depth="2">

- Different window, modernizing a real project
- Running with **Superpowers**: brainstorm → spec → plan → subagents → execute
- We don't watch it. It works while we talk.
- We come back at the end and see what it did

</v-clicks>

<!--
- Name the actual project before showing the window
- Commit explicitly: "we will not peek for the next 90 minutes"
- This is an autonomy demo that doesn't eat session time
-->


---
layout: section
---

# The Maturity Ladder

::subtitle::

Where are you climbing from?

---
layout: default
---

# Levels of Agentic Engineering

<v-clicks depth="2">

- **L0** Manual coding — no AI
- **L1** Autocomplete — Copilot, Cursor tab
- **L2** Chat & copy-paste — ChatGPT in another window
- **L3** AI-enabled IDE — Cursor Composer, multi-file edits
- **L4** Single-agent loops — Claude Code with CLAUDE.md
- **L5** Custom workflows — slash commands, hooks, skills
- **L6** Multi-agent teams — sub-agents, parallel work
- **L7** Background agents — autonomous work, you review the result
- **L8** Self-improving systems — the agent updates its own playbook

</v-clicks>

<!--
- Source: bassimeledath.com/blog/levels-of-agentic-engineering
- Competing taxonomies: Simon Willison's 5 levels, Steve Yegge's 8 for Gas Town
- Numbers don't matter — the trajectory does
- Don't try to teach the whole ladder; just show the shape
-->


---
layout: default
---

# Where most of you are right now

<v-clicks>

- L1–L3 if the bootcamp didn't fully stick
- L3–L4 if it did
- The room is mixed — that's fine
- We skip L0–L2: those rungs are not the destination
- **Today's target rung: L4–L5** — the daily loop, on your own terms

</v-clicks>

<!--
- Bootcamp grads who shipped: probably L4 already
- Tie back to the bootcamp without name-checking individuals
- Show of hands optional, mostly for engagement
- Don't spend more than 90s here
-->


---
layout: quote
---

# Context engineering is the lever that moves you up the ladder.

<!--
- Thesis sentence — say it slow
- Pause for it
- The session = mechanics for pulling this lever
-->


---
layout: section
---

# Building Blocks

::subtitle::

System prompts · CLAUDE.md · Memory · Skills

---
layout: default
---

# System prompts

<v-clicks depth="2">

- The prompt the model never sees you write
- Sets identity, allowed tools, hard rules
- Claude Code stitches: system prompt + CLAUDE.md + your input
- **Demo**: add `Don't talk about goblins` to the system prompt
  - Watch every conversation respect it without ever being asked
  - Same mechanism = "Always run tests after edits", "Never commit without asking"

</v-clicks>

<!--
- Goblins example is silly on purpose — makes the mechanism vivid
- ChatGPT's "personality" comes entirely from system prompt
- Analogy: the constitution before the laws
- This is what jailbreaking attacks try to override
-->


---
layout: default
---

# CLAUDE.md hierarchy

<v-clicks depth="2">

- `~/.claude/CLAUDE.md` — your global preferences (skill levels, communication style)
- `<project>/CLAUDE.md` — repo-wide conventions (committed)
- `<project>/CLAUDE.local.md` — your personal overrides (gitignored)
- `<subdir>/CLAUDE.md` — narrow rules for one area
- They **stack**, deeper wins
- Project CLAUDE.md overrides global. Subdir overrides project.

</v-clicks>

<!--
- Live demo: show your own ~/.claude/CLAUDE.md briefly
- .local is gitignored by Claude Code default
- Test "is this rule loaded?" → just ask Claude
- Anti-pattern: stuffing personal prefs into project CLAUDE.md (commits to repo)
-->

---
layout: default
---

# Memory & Compaction

<v-clicks depth="2">

- **Auto-memory**: `~/.claude/projects/<proj>/memory/` — Claude builds it as you talk
  - User memories, feedback, project facts, references
  - `/memory` to inspect, edit, prune
- **Compaction**: long sessions get summarized, old messages drop
  - "Context rot": accuracy decreases as token count grows
  - Compaction fights it — but you lose detail you can't get back
- Don't put load-bearing facts only in chat. Put them in memory or CLAUDE.md.

</v-clicks>

<!--
- Four memory types: user, feedback, project, reference
- Memory beats CLAUDE.md for transient/dated context
- Stale memory is worse than no memory — clean it
- /memory live to show the structure
-->

---
layout: default
---

# Skills

<v-clicks depth="2">

- A markdown file Claude loads on-demand when its description matches what you're doing
- **Frontmatter** declares when to use it:
  - `name`, `description` (the discovery hook), `when_to_use`
- **Local**: `<project>/.claude/skills/` — committed, team-shared
- **Global**: `~/.claude/skills/` — your personal toolkit across projects
- Examples that already ship with Superpowers: brainstorming, TDD, debugging, code-review, plan-writing
- Anatomy: instructions, examples, optional checklists, optional sub-references

</v-clicks>

<!--
- Show one Superpowers skill open
- Description = the discovery hook (what Claude searches against)
- Skills can reference other skills (composability)
- Skill authoring is itself a Superpowers skill (meta)
-->


---
layout: section
---

# Superpowers

::subtitle::

The fast on-ramp

---
layout: comparison
---

# Why Superpowers

<div class="cols">
<div class="col">

### Roll your own

- Total control
- Months to build a real workflow
- Every team writes the same skills from scratch
- You learn a lot — slowly

</div>
<div class="col">

### Superpowers

- Opinionated workflow OOTB
- Brainstorming, TDD, debugging, code review, plan-writing
- 121k+ stars, real usage
- Override what you don't like
- **Start working today**

</div>
</div>

<!--
- Adoption path: install → use → override → eventually replace what you've outgrown
- Jesse Vincent / @obra: "imposing rigorous methodology on existing model > letting better model run free"
- Don't sell — let them try it
-->


---
layout: default
---

# Peek at the background Claude

<v-clicks depth="2">

- It's been running for ~30 minutes
- What it produced so far:
  - A spec it wrote from our prompt
  - A plan with concrete steps
  - Subagent dispatches happening in parallel
- We don't read the work. Just notice the **shape**.
- Back to it at the end of the session.

</v-clicks>

<!--
- Things to show: worktree, spec.md, plan.md, subagent log
- Resist questions about WHAT the work is
- Goal is showing autonomy shape, not solving a problem live
-->


---
layout: section
---

# Context Engineering

::subtitle::

Lost in the middle

---
layout: default
---

# It's not prompt writing

<div class="full-width text-xl italic text-orange-400">

Memory budgeting · retrieval strategy · tool-output shaping ·
instruction layering · eviction policy

</div>

<v-clicks>

- Prompt = one sentence
- Context = the whole screenplay
- The discipline of curating **everything the model sees**

</v-clicks>

<!--
- Tobi Lutke: "the art of providing all the context for the task to be plausibly solvable"
- Screenplay vs sentence
- Pace this slide — it's the conceptual reset before the practical slides
-->

---
layout: default
---

# LLMs read the start and end. The middle is the dumb zone.

<v-clicks depth="2">

- "Lost in the middle" — [arxiv 2307.03172](https://arxiv.org/abs/2307.03172)
- Long contexts: instructions in the middle get followed less reliably
- **Implications**:
  - Put hard rules at the **top** of CLAUDE.md
  - Put recap at the **bottom** of long prompts
  - Long context ≠ used context

</v-clicks>

<!--
- Reference NOTE_ContextEngineering.png + NOTE_ContextEngineering2.png — copy into images/ before talk
- Why CLAUDE.md must be short: model literally reads less of it as it grows
- Arxiv paper tested retrieval; agents see similar effects anecdotally
- Compaction fights middle-rot but loses detail you can't get back
-->


---
layout: default
---

# AGENTS.md / CLAUDE.md as a 100-line TOC

<v-clicks depth="2">

- OpenAI's pattern: AGENTS.md ≈ 100 lines, **table of contents only**
- Detail lives in linked structured docs the agent loads on demand
- Documentation **freshness in CI** — not ad-hoc updates that go stale
- ETH Zurich (2026): bloated AGENTS.md files actively hurt agent performance
- Don't write what's inferable from the code. Fix the code first.

</v-clicks>

<!--
- OpenAI's own AGENTS.md is public — show it
- "Freshness in CI" = part of pre-commit / a CI check
- Test: if it's already in the README, skip it from CLAUDE.md
-->

---
layout: default
---

# CLIs &gt; MCPs (often)

<v-clicks depth="2">

- Every MCP tool **inflates the system prompt** with its schema
- 20 MCPs loaded → thousands of tokens before you've even prompted
- A CLI tool the agent runs via Bash: schema cost = 0
- **Rule of thumb**: load MCPs you actually need this session, not "just in case"
- DeepWiki MCP, Context7, Braintrust — load when relevant, unload otherwise

</v-clicks>

<!--
- Contrarian take — flag it
- Counter-examples: Playwright (browser), GitHub MCP earn their schema cost
- Load MCPs per-project, not globally
- A bash CLI tool the agent invokes = zero schema cost
-->

---
layout: default
---

# Skills-in-git: your team's playbook

<v-clicks depth="2">

- Every skill checked into the repo = institutional memory the agent reads
- "How we deploy", "how we write tests", "how we review PRs"
- New hire + AI: onboarded against the same skill files a senior reads
- Skills compound: every lesson learned becomes one more skill the next session inherits
- See: [engineering.block.xyz/blog/3-principles-for-designing-agent-skills](https://engineering.block.xyz/blog/3-principles-for-designing-agent-skills)

</v-clicks>

<!--
- Pull a real example from Itenium ("how we deploy to staging")
- Team aspect: everyone gets the same skill loadout
- Block's 3 principles: descriptive name, scoped trigger, layered detail
-->

---
layout: section
---

# Guardrails

::subtitle::

Tests are cheap. Make them load-bearing.

---
layout: default
---

# TDD with Claude — the way that actually works

<v-clicks depth="2">

- Bad: "write me feature X with tests"
- Good: red-green-refactor, **enforced**
  - Write the test first
  - Run it. Watch it fail. (skill checks the failure is the right shape)
  - Implement
  - Run it. Watch it pass.
  - Refactor under green
- Hooks can enforce: no implementation edits while no failing test exists
- Result: 85-95% coverage instead of 30-50%

</v-clicks>

<!--
- Why TDD works better WITH AI: forces it to slow down, otherwise it writes test+impl together (cheating)
- Hook can block impl edits while no failing test exists
- Superpowers' TDD skill enforces the cycle
- Common failure: agent writes a passing test for nothing
-->

---
layout: default
---

# Hooks &gt; instructions

<v-clicks depth="2">

- Instructions in CLAUDE.md: model can forget, skip, rationalize
- Hooks: deterministic shell commands the model **cannot bypass**
- Lifecycle events:
  - `PreToolUse` — block dangerous edits
  - `PostToolUse` — auto-format, run linter
  - `UserPromptSubmit` — inject context, validate scope
  - `SessionStart` — set up memory, kick off background tasks
- **Use hooks for anything you need every time.**

</v-clicks>

<!--
- Live demo: show a hook firing
- Model can't talk its way out of a hook
- Example hooks: pre-commit lint, post-edit format, session-start memory load
- One hook = one rule that holds forever
-->

---
layout: default
---

# Agent backpressure

<v-clicks depth="2">

- Give the agent a feedback loop it reads:
  - Type checker
  - Tests
  - Linter
  - Formatter
  - LSP diagnostics
  - Visual inspection (screenshots, Playwright)
- **Context-efficient output**: tools should return `✓` on success
  - Verbose success logs eat context for nothing
  - Save the words for the failures
- See: [latentpatterns.com/glossary/agent-backpressure](https://latentpatterns.com/glossary/agent-backpressure)

</v-clicks>

<!--
- Tools that work well: tsc --noEmit, vitest --reporter dot, eslint --quiet
- "Return ✓" non-obvious until you see verbose output eat 5k tokens
- Save the words for failures
-->

---
layout: default
---

# Compounding Engineering

<v-clicks depth="2">

- Every bug found = a skill or hook so it never happens again
- Every PR review comment = a CLAUDE.md rule
- Every "ugh, the agent always does X wrong" = a check that prevents X
- **The agent gets smarter at your codebase the longer you use it**
- This is the closest you'll get to L8 (self-improving) right now

</v-clicks>

<!--
- Trigger: every time you correct Claude, ask "skill or hook?"
- Block's principles for designing skills are the canonical reference
- Compound interest: one skill/week × 50 weeks = a different agent next year
-->

---
layout: section
---

# Code Reviews

::subtitle::

The 2026 problem

---
layout: quote
---

# Code must not be written by humans.<br/>Code must not be reviewed by humans.

<!--
- StrongDM Dark Factory provocation
- Read slow, pause for reaction
- Frame as a question, not an assertion
- Don't endorse, don't reject
-->


---
layout: default
---

# Reviews are the new bottleneck

<v-clicks depth="2">

- Faros AI data: PR review times **nearly doubled** with AI-generated code
- More code, same reviewers, same Mondays
- "Speed in one phase created drag in every other phase"
- The model that wrote it **shouldn't** review it (shared blind spots)
- See: [latent.space/p/reviews-dead](https://www.latent.space/p/reviews-dead)

</v-clicks>

<!--
- Reference NOTES_CodeReviews.png — copy into images/ before talk
- Faros: PR review times nearly doubled
- Ask the room: PR review time now vs last year?
- This is THE productivity tax of AI today
-->


---
layout: default
---

# Multi-agent review

<v-clicks depth="2">

- Different agent than the one that wrote
- Different model where possible (Claude wrote → GPT reviews, or vice versa)
- **Specialized review skills**: security, performance, test coverage, docs
- Agents leave **line-specific GitHub comments** like a human reviewer
- Human role: arbitrate disagreement, sign off on architecture
- Risk: review theater. Mitigation: track which review comments led to changes.

</v-clicks>

<!--
- Review is genuinely a different cognitive task than write
- Line-specific GH comments via `gh api`
- Superpowers ships a code-review skill
- Failure mode: bikeshedding (calibrate by % comments → changes)
- The agent that wrote shouldn't review (shared blind spots)
-->

---
layout: default
---

# Dark Factory

<v-clicks depth="2">

- StrongDM: a development pipeline where humans only set direction
- Agents write, agents review, agents merge
- Humans monitor metrics, not commits
- Today: works for narrow domains (boilerplate, migrations)
- 2027: who knows
- See: [simonwillison.net/2026/Feb/7/software-factory](https://simonwillison.net/2026/Feb/7/software-factory/)

</v-clicks>

<!--
- Mostly aspirational today
- Stripe Minions: 1000+ PRs/week. TELUS: 500k hours saved
- Hard questions: ownership, accountability, when something breaks
- Not 2026 reality for most teams
-->

---
layout: break
---

# ☕ Break

::timer::

<Timer minutes="10" />

::image::

![](./images/cover.png)

---
layout: section
---

# Gallery of Leverage

::subtitle::

Tests-as-oracle: the unifying pattern

---
layout: default
---

# Tests-as-oracle

<v-clicks depth="2">

- The pattern behind every demo coming up:
  1. **Cover** existing behavior with tests (Claude writes them)
  2. **Change** the implementation under green
  3. **Trust** green = behavior preserved
- Works for any "I want to swap X for Y" job
- The test suite becomes the contract, not the code

</v-clicks>

<!--
- Pattern predates AI but AI makes it cheap
- The "scary refactor" disappears
- Calibration matters: what % of behaviors do tests cover?
- If you don't trust your tests, you don't trust the swap
-->

---
layout: default
---

# 1. Onboarding-fast

<v-clicks depth="2">

- New repo, no idea where anything lives
- Ask Claude to produce a **map**: entry points, data flow, hot paths
- Ask it to find "the file you'd edit to do X"
- Ask it to **write CLAUDE.md** for the repo, then critique it
- 30 minutes vs 2 days

</v-clicks>

<!--
- Real story from your own onboarding (pick a repo)
- Best move: ask Claude to draw the call graph for ONE feature
- Then ask it to write CLAUDE.md, then critique what it wrote
- New hire + AI > new hire alone
-->

---
layout: default
---

# 2. Modernization — VierOpEenRij

<v-clicks depth="2">

- Old framework / old deps / old patterns
- Claude reads, plans the migration, executes in chunks
- **Tests are the safety net** — if green stays green, behavior preserved
- Run with Superpowers + sub-agents for big migrations
- The background Claude is doing this *right now*

</v-clicks>

<!--
- Tell the actual VierOpEenRij story: what was modernized, what it cost, what it saved
- Pitfall: don't trust without tests — even with AI
- The plan/spec/subagent loop visible in the background Claude is THIS pattern
-->

---
layout: default
---

# 3. Performance optimization

<v-clicks depth="2">

- Hot path is slow. You want a rewrite. You're scared to touch it.
- Workflow:
  1. Cover current behavior with tests (lock the contract)
  2. Write new implementation alongside the old
  3. Run tests against **both**
  4. Both green? Delete the old code.
- Same flow works for: refactoring legacy code, reducing complexity, simplifying APIs

</v-clicks>

<!--
- Classic example: a hot loop you wanted to rewrite for years
- Test suite IS the safety net
- Add benchmark assertions to the suite (perf as a test)
- "Both green, delete old" is the magic moment
-->

---
layout: default
---

# 4. Library swap — MomentJS

<v-clicks depth="2">

- MomentJS deprecated for years, still in production everywhere
- Workflow:
  1. Add tests for every place that uses moment (date formatting, parsing, math)
  2. Swap to date-fns / Luxon / native `Intl.DateTimeFormat`
  3. Tests pass → done
- Generalizes: any library swap (lodash, jQuery, axios, MomentJS, classnames)
- The hard part is now the test coverage — and Claude writes that

</v-clicks>

<!--
- Tell the MomentJS story (what you replaced it with)
- Generalizes because dates are well-tested by users
- Jest snapshot tests on date strings = trivial coverage
- Same pattern: lodash, jQuery, axios, classnames
-->

---
layout: section
---

# Wrap

::subtitle::

What did the background Claude do?

---
layout: default
---

# Back to our second Claude

<v-clicks depth="2">

- Open the worktree
- What did it produce?
- What's good?
- What's funny?
- Honest read: would you ship this?
- What did Superpowers add over raw Claude?

</v-clicks>

<!--
- ~5 min on this slide
- If it nuked something, SHOW THAT — credibility skyrockets
- Audience can smell sales mode
- "Would I ship this PR?" is the right question
-->


---
layout: default
---

# Monday morning — three changes

<v-clicks depth="2">

1. **Add a `tests` rule to your CLAUDE.md**
   - "After edits to `src/`, run `bun test` and report failures."
2. **Install Superpowers**
   - One command. You get brainstorming, TDD, debugging, code-review skills today.
3. **Write one skill**
   - The thing you do every day that the agent always gets wrong.
   - That's your first compound interest payment.

</v-clicks>

<!--
- Write these on a whiteboard or print as a handout
- Three concrete things — no "and many more"
- Make them small enough to do tonight
-->

---
layout: default
---

# Series ahead

| #  | Title                                | What's new                                                  |
| -- | ------------------------------------ | ----------------------------------------------------------- |
| S2 | Writing your team's playbook in skills | From *using* Superpowers to *authoring* skills for your repo |
| S3 | Modernization & autonomous loops     | Ralph Wiggum, sub-agents, real Dark Factory recipes         |
| S4 | MCP & extending Claude               | Practical MCP servers, when to build your own               |
| S5 | Frameworks comparison *(optional)*   | BMAD vs Superpowers vs SpecKit                              |

<!--
- Explicit invitation: "what topics do YOU want?"
- Series shape can shift based on attendance and interest
-->

---
layout: socials
---

---
layout: source
source: itenium-be/Bootcamp-AI-Presentation
---

---
layout: end
---
