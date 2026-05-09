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
## From Vibe Coding to Agentic Engineering

::image::

![](./images/cover.webp)


---
layout: statement
---

# Prompt Engineering
## It's 2023

<div v-click style="position: relative; display: inline-block;">

![](./images/prompt-engineering-job.png)

<div v-click="2" style="
  position: absolute;
  top: 92px; left: -15px;
  width: 140px; height: 50px;
  border: 6px solid #E78200;
  border-radius: 50%;
  transform: rotate(-4deg);
  pointer-events: none;
"></div>

</div>

::image::

![](./images/prompt-engineering.jpg)


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

Let's burn some tokens

---
layout: default-aside
---

# Case Study: Four on a row
## Modernization of a pet project

<v-clicks depth="2">

- Something I made back in school
  - VB.NET, .NET 2.0, WinForms
- Let's modernize this baby
  - C#, .NET 10, Avalonia
- A demo of **obra/superpowers**
  - brainstorm → spec → plan → subagents → execute
- I already prepared the spec/plan
  - Let's fire it up

</v-clicks>

::image::

![](./images/case-study.jpg)

<!--
AI is especially good in a few things ouf of the box:  
ex: Prototyping, Onboarding, Modernization, Refactoring, Replacing obsolete dependencies

**Short demo of spec/plan**

We're just letting it run:
- These frameworks make it especially easy to get started with AI Driven Development
- We'll integrate the results with other things we'll talk about during this course
-->


---
layout: section
---

# The Maturity Ladder

::subtitle::

Where are you now?

---
layout: default
---

# Levels of Agentic Engineering

<MaturityLadder
  :items="[
    'Manual Caveman Coding',
    'Tab Complete & Chat Copy/Paste',
    'Agent IDE',
    'Context Engineering',
    'Compounding Engineering',
    'MCP & Skills',
    'Harness Engineering',
    'Background Agents',
    'Autonomous Agent Teams',
  ]",
  :highlight="[3, 6]"
/>

<!--
**Agent IDE**: Heavy use of Plan Mode  
**Compounding Engineering**: Improve the next sessions (Kieran Klaassen)  
**Harness Engineering**: Automated feedback loops  
**Background Agents**: Where we're at in 2026. Plan Mode is dying, if everything before is in place,
you no longer need to babysit the plan.  
**Autonomous Agent Teams**: The active frontier (ex: Claude's experimental Agent Teams)

**Sources**:
- https://bassimeledath.com/blog/levels-of-agentic-engineering
- Competing taxonomies:
  - Simon Willison's 5 levels: https://simonwillison.net/2026/Jan/28/the-five-levels/
  - Steve Yegge's 8 levels: https://www.augmentcode.com/guides/steve-yegge-8-levels-ai-assisted-development
-->


---
layout: quote
---

# Context Engineering
## The lever that moves you up the ladder

::image::

![](./images/context-engineering.jpg)


---
layout: default-aside
---

# The Prompt
## More than the conversation

<PromptPrism
  :items="[
    'System Prompt',
    'CLAUDE.md',
    'MCP tool schemas',
    'Skills index',
    'Memory',
    'System Reminders',
    'Conversation [7..n]',
  ]"
/>

::image::

![](./images/the-prompt.jpg)

<!--
Before we can start with Context Engineering,
we need to go back to Prompt Engineering.

**WHAT is the prompt?**
-->


---
layout: default-aside
---

# Why does this matter
## Lost in the Middle

<v-clicks depth="2">

- Context Windows have grown considerably
- But the middle is the "dumb zone"
- Instructions in the middle get followed less reliably
- **Implications**:
  - Big `CLAUDE.md` files are harmful
  - MCP Tools you're not using for the task at hand eat precious context

</v-clicks>

::image::

![](./images/lost-in-the-middle.jpg)

<!--
Lost in the middle: https://arxiv.org/abs/2307.03172
-->


---
layout: statement
---

# The System Prompt
## What's up with all the goblins


<div class="goblin-quote">
  You are an unapologetically nerdy, playful and wise AI mentor to a human.
  You are passionately enthusiastic about promoting truth, knowledge,
  philosophy, the scientific method, and critical thinking. [...] You must
  undercut pretension through playful use of language. The world is complex
  and strange, and its strangeness must be acknowledged, analyzed, and
  enjoyed. Tackle weighty subjects without falling into the trap of
  self-seriousness. [...]
</div>

<style scoped>
  .goblin-quote {
    font-style: italic;
    font-size: 1.1rem;
    line-height: 1.55;
    color: #555;
    margin-top: 1.25rem;
    max-width: 52rem;
  }
</style>

::image::

![](./images/the-system-prompt.jpg)

<!--
Injected as the very first thing into the context.

**Goblins**:  
The "Nerdy" personality of ChatGPT-5.1 started mentioning Goblins more often  
To show that it really is a black box...  
https://openai.com/index/where-the-goblins-came-from/

So OpenAI explicitly added to Codex:  
> Never talk about goblins, gremlins, raccoons, trolls, ogres, pigeons, or other animals or creatures unless it is absolutely and unambiguously relevant to the user's query.


**Hard Rules**:  
Circumvent these with jailbreaking...

OpenAI Codex base_instructions:  
https://simonwillison.net/2026/Apr/28/openai-codex/
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
