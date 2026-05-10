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
textSize: lg
items:
  - The Maturity Ladder
  - Context Engineering
  - Compounding Engineering
  - Harness Engineering
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

</v-clicks>

<div v-click class="full-width text-5xl italic mt-10">
<FireText>Let's fire it up</FireText>
</div>

::image::

![](./images/case-study.jpg)

<!--
AI is especially good in a few things ouf of the box:  
ex: Prototyping, Onboarding, Modernization, Refactoring, Replacing obsolete dependencies  
-> Just fire it up now, we'll cover details later!
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

# Ladders of AI Maturity

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
layout: statement
---

> The art of providing all the context for the task to be plausibly solvable by the LLM.
- Tobi Lutke (Shopify CEO)


---
layout: default
---

# Context Engineering
## Designing what the agent sees


<CompassDisciplines
  hub-label="Context Engineering"
  :items="[
    'Memory Budgeting',
    'Retrieval Strategy',
    'Tool-output Shaping',
    'Instruction Layering',
    'Eviction Policy',
  ]"
/>

<!--
**Memory Budgeting**: What have you spent before you have started  
**Retrieval Strategy**: How does info get pulled into context  
**Tool-output Shaping**: Tools inject their output into context; more in Harness Engineering section  
**Instruction Layering**: Where you place what matters  
**Eviction Policy**: What when the context has filled up  
-->



---
layout: statement
---

# LLMs are stateless
## The harness is where state lives

::image::

![](./images/llms-stateless.jpg)

<!--
The whole context is sent to the LLM at every turn.  
The first part is fixed, you only pay full price once, then it hits the cache
and you pay ~10% of the cost.  
Changing CLAUDE.md mid-session is thus expensive.
-->


---
layout: statement
---

# **Prompt**: one sentence
# **Context**: the whole screenplay

<br>

## The discipline of curating **everything the model sees**



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

**System Prompt**:  
You are an AI assistant + pages of practical operational guidance

**&lt;system-reminder&gt;**: Authoritative  
- bypass middle-rot, added close to the action
- SessionStart, gitStatus, file changes, claude hooks (PreToolUse, stuff in settings.json), skill invocation
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
- Big `CLAUDE.md` files are harmful (avoid `/init` bloat)
  - Don't add what's inferable from the code
  - If the code is ambiguous, fix the code instead
- <small>MCP Tools you're not using for the task at hand eat precious context</small>

</v-clicks>

<div v-click class="full-width text-3xl italic text-orange-400 mt-5">
The Context Window is your memory budget - use it wisely
</div>

::image::

![](./images/lost-in-the-middle.jpg)

<!--
- Lost in the middle: https://arxiv.org/abs/2307.03172
- ETH Zurich (2026): bloated AGENTS.md files actively hurt performance
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
layout: default-aside
---

# The System Prompt
## Injects an Extensive Bash Manual

<VClickTable
  :headers="['Category', 'Examples']"
  :rows="[
    ['Tool routing', 'Use Read, not cat · Use Edit, not sed'],
    ['Shell hygiene', 'Quote paths with spaces · don\'t <code>cd</code> unnecessarily'],
    ['Dangerous-command warnings', 'Don\'t <code>rm -rf /</code> · don\'t <code>git reset --hard</code> without reason · never skip pre-commit hooks'],
    ['Tool-specific recipes', 'How to make a commit · how to create a PR via <code>gh</code>'],
    ['Performance tips', '<code>find</code> from <code>.</code> not <code>/</code> · longest-alternative-first in regex'],
    ['Behavioral defaults', 'Run independent tools in parallel · don\'t sleep between commands · use TodoWrite for multi-step work'],
  ]"
  :firstVisible="1"
  size="sm"
/>


---
layout: default-aside
---

# CLAUDE.md Hierarchy

<v-clicks>

- `Program Files/ClaudeCode/CLAUDE.md`
  - **Org**: security policies, compliance requirements
- `~/.claude/CLAUDE.md`
  - **Personal Global**: skill levels, communication style
- `<project>/CLAUDE.md`
  - **Team**: architecture, coding standards, common workflows
- `<project>/CLAUDE.local.md`
  - **Personal Local**: sandbox URLs, preferred test data
- `<subdir>/CLAUDE.md`
  - **Team Folder**: narrow rules for one area
- Use `AGENTS.md` for cross-tool compatibility

</v-clicks>

::image::

![](./images/claude-mds.jpg)

<!--
My `~/.claude/CLAUDE.md` contained "Don't add 'Co-Authored' to git commit messages":  
Undeterministic + I was paying these tokens for every session, every project  
-> This is what "Claude Code PreToolUse hooks" are for!

That is what Claude told me, but then I thought:

-> It would always try to add it first, it would fail, then retry it
-> Wasting even more tokens! The right way is:
~/.claude/settings.json: { "attribution": { "commit": "", "pr": "" } }

**WHAT**:  
- Claude makes the same mistake a second time
- A code review catches something Claude should have known about the codebase
- You type the same correction or clarification into chat that you typed last session
- A new teammate would need the same context to be productive

**NOTES**:  
- HTML comment blocks are stripped out before injected into context (only for humans)
- Use "@FILE.md" to load additional files into a CLAUDE.md
- In `.claude/rules`, use frontmatter "paths" to conditionally load md files (ex: src/api/**/*.ts)
- CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1 claude --add-dir ../shared-config

See: https://code.claude.com/docs/en/memory
-->

---
layout: default-aside
---

# Memory & Compaction

<v-clicks depth="2">

- **Auto-Memory**: `~/.claude/projects/<proj>/memory/`
  - Claude builds it as the session goes on
    - Build commands, debugging insights, architecture notes, preferences, workflow habits
  - `/memory` to inspect & prune
    - Context Poisoning: Stale memory is worse than no memory
- **Compaction**:
  - _"Context rot"_: accuracy decreases as token count grows
  - Compaction fights it — but you lose detail you can't get back

</v-clicks>

<div v-click class="full-width text-2xl italic text-orange-400 mt-2">
Don't put load-bearing facts only in chat.
<br>Put them in memory or CLAUDE.md.
</div>

::image::

![](./images/compaction.jpg)


<!--
Memory beats CLAUDE.md for transient/dated context

"Land the plane": use `/new` instead of relying on lossy compactions  
Consider creating a handoff prompt to kick-start the new session.

MEMORY.md: only first 200 lines or 25k is actually injected into context  
Topic files like debugging.md or patterns.md are read on demand, as needed
-->

---
layout: default-aside
---

# Skills

<v-clicks>

- Loaded automatically or on-demand
- Only **frontmatter** for **discovery** is loaded in every context (name & description)
- **Project**: `<project>/.claude/skills/`
- **Personal**: `~/.claude/skills/`
- **Anatomy**: instructions, examples, optional checklists, sub-references

</v-clicks>

::image::

![](./images/skills.jpg)


<!--
- See `research/superpowers/brainstorming/SKILL.md`
- A folder for each skill, with only SKILL.md mandatory
- Skills can reference other skills (composability)
- Skill authoring is itself a Superpowers skill (meta)
-->


---
layout: default
---

# MCPs & CLIs

<v-clicks depth="2">

- Every MCP tool **inflates the prompt** with its schema
- Load what you need in the current session (or unload)
  - Audit `/mcp` periodically
- Consider using a CLI instead: `gh` vs `GitHub MCP`
  - A CLI tool the agent runs via Bash has zero schema cost
- Some MCPs are very powerful
  - Context7: Stale training data vs up-to-date open-source APIs
  - Playwright MCP: give your agent "eyes"

</v-clicks>

<div v-click class="full-width text-2xl italic text-orange-400 mt-8">
MCPs give the agent access to state or signals it can't otherwise reach
<br>Current docs, opaque internals, live UIs, designs, production errors.
</div>


<!--
**Rule of Thumb**:
- A single bash invocation? → CLI.  
- State between calls or structured output? → MCP.

Give your agent eyes:  
- Figma MCP: your designs
- Sentry MCP: production logs, ...
- Jira MCP: your tickets
- Postgres MCP: live data

Other:  
DeepWiki MCP: Explore an unknown open-source library
-->



---
layout: default
textSize: sm
---

# Eviction Policy

<v-clicks depth="2">

- `/statusline`: Keep constant track of your context window
  - Start thinking about a new session once you hit **40%**
- Avoid `/compact` (lossy), but "_land the plane_" and `/clear` (nuclear)
- Sub-agents avoid eviction by running in their own context window
- If you're unsure what's eating your `/context`:

</v-clicks>

<v-click>

![](./images/slash-context.png)

</v-click>


<!--
`~/.claude/settings.json`:  
```json
"statusLine": {
  "type": "command",
  "command": "bash statusline.sh"
},
```
-->

---
layout: default-aside
textSize: sm
---

# Retrieval Strategy
## How code gets pulled into context

<v-clicks depth="2">

- **Agentic grep vs Vector RAG**
  - Code mutates constantly, RAG index goes stale
  - grep & read gives the agent precise, current state
- **Tool ladder**:
  - `glob`/`find`: narrow scope before grep/read
  - `grep`: find symbols/strings, usually sufficient
  - `read`: only when the full file is needed
- **Delegate noisy read retrieval to a sub-agent**
  - Returns a summary, not 10k lines of search hits
- **RAG wins for**: stable docs, large external corpora

</v-clicks>

<div v-click class="full-width text-2xl italic text-orange-400 mt-3">
Claude Code (grep/read) <-> Cursor, Copilot (RAG)
</div>

::image::

![](./images/retrieval-strategy.jpg)

<!--
**RAG**: Retrieval-Augmented Generation
- Fast semantic-ish lookup BUT
- RAG needs a re-index pipeline + Top-k chunks lose structural context (the surrounding functions, the imports)
- DB: Pinecone, pgvector, Chroma, ...

**Tools Ladder**:
- `grep`: matching lines, pull the few lines around those lines into context
- `glob/find`: find matching files
- `read`: where the sub-agents with summary come in

**When RAG fits**:
- For example Context7 for docs
- MCP servers for Confluence, Wiki, ...
For Claude just a tool call, RAG is someone elses problem
-->


---
layout: default-aside
---

# Progressive Context Disclosure
## How config gets pulled into context

<v-clicks depth="2">

- The pattern: thin index → load detail on demand
- Same shape across three mechanisms:
  - **CLAUDE.md** ≈ 100 lines, table of contents
  - **Memory**: MEMORY.md index, files load when relevant
  - **Skills**: name + description upfront, body loads on invocation

</v-clicks>

::image::

![](./images/progressive-context-disclosure.jpg)



---
layout: default-aside
textSize: sm
---

# Context Engineering
## What's actionable

<v-clicks depth="2">

- **The pattern**: progressive disclosure — thin index, load on demand
- **Closer to the action = more reliable**
  - Chat (one-shots) → CLAUDE.md (top vs middle) → skill (loads when relevant) → harness (deterministic hooks)
- **Your conversation is context too**
  - Don't paste 200-line stack traces — extract the 5 lines that matter
  - Tell the agent to be terse in `CLAUDE.md`; verbose narration eats your own budget
- **Watch your MCPs**: every tool schema is rent, CLIs are free
- **At 40%, land the plane**: `/clear` beats `/compact`

</v-clicks>

<div v-click class="full-width text-2xl italic text-orange-400 mt-3">
The context window is your budget. Spend it like one.
</div>

::image::

![](./images/context-takeaways.jpg)

<!--
`CLAUDE.md`:  
Important instructions at the top, not buried in 500 lines of prose.

**MCPs**: A 20-tool MCP can cost 5k+ tokens of
schema before you've done any work. (`/mcp` and `/context`)
-->








---
layout: section
---

# Case Study: Four on a row

::subtitle::

What are these Superpowers doing...


---
layout: default
---

# Superpowers
## Or...

<div class="dense">

| ⭐    | Project        | What it is                                            |
| ----- | -------------- | ----------------------------------------------------- |
| 184k  | [Superpowers]  | Composable Claude Code skills workflow                |
| 73k   | [OpenHands]    | Autonomous runtime — sandboxed coding agent           |
| 61k   | [Cline]        | IDE assistant — open-source agent inside VS Code      |
| 46k   | [BMAD-METHOD]  | Multi-persona agile framework (PM, Dev, …)            |
| 44k   | [Aider]        | CLI — git-native AI pair programmer                   |
| 37k   | [agent-skills] | Addy Osmani's curated skills                          |

[Superpowers]: https://github.com/obra/superpowers
[OpenHands]: https://github.com/OpenHands/OpenHands
[Cline]: https://github.com/cline/cline
[BMAD-METHOD]: https://github.com/bmad-code-org/BMAD-METHOD
[Aider]: https://github.com/Aider-AI/aider
[agent-skills]: https://github.com/addyosmani/agent-skills

</div>

<div class="full-width text-2xl italic text-orange-400 mt-10">
We'll get deeper into MCPs, Skills, Plugins, ... in another session!
</div>

<!--
Or:
- ⭐ 184k https://github.com/obra/superpowers
- ⭐ 73k https://github.com/OpenHands/OpenHands
- ⭐ 61k https://github.com/cline/cline
- ⭐ 61k https://github.com/gsd-build/get-shit-done
- ⭐ 46k https://github.com/bmad-code-org/BMAD-METHOD
- ⭐ 44k https://github.com/aaif-goose/goose
- ⭐ 44k https://github.com/Aider-AI/aider
- ⭐ 37k https://github.com/addyosmani/agent-skills
- ⭐ 33k https://github.com/continuedev/continue
- ⭐ 27k https://github.com/eyaltoledano/claude-task-master
- ⭐ 19k https://github.com/stitionai/devika
- ⭐ 15k https://github.com/plandex-ai/plandex
-->



---
layout: comparison
---

# Why Superpowers
## Skills Framework

<div class="cols">
<div class="col">

### Roll your own

<v-clicks>

- Total control
- Months to build a real workflow
- Every team writes the same skills from scratch
- You learn a lot — slowly

</v-clicks>

</div>
<div class="col">

### Superpowers

<v-clicks>

- Opinionated workflow OOTB
- Brainstorming, TDD, debugging, code review, plan-writing
- ⭐ 184k stars, real usage
- Override what you don't like
- **Can help a lot without you having to think about it, today**

</v-clicks>

</div>
</div>

<!--
**Roll your own**:  
I attended "Coding is dead" where teams presented their work.
A team of two devs showed me 4+ weeks of work. They rolled their own.
Their demo looked like what one person would create in two days
with superpowers.

**Superpowers**:  
The FourOnARow modernization spec/plan and execution
is currently being done with Superpowers.
-->

---
layout: default-aside
---

# So... Superpowers?

<v-clicks depth="2">

- Brainstorm: what are we gonna build today!?
- **External State** as context offloading
  - Spec Writing: ensure the brainstorm doesn't rot
  - Plan Writing: turn the spec into instructions
- Handover to Sub-Agents: keep main window clean

</v-clicks>

<div v-click class="full-width text-5xl italic text-orange-400 mt-20">
!! This is Context Engineering 101 !!
</div>

::image::

![](./images/superpowers.jpg)


<!--
**Short demo of spec/plan + subagents (dispatching-parallel-agents)**
-->


---
layout: break
---

# ☕ Break / QA

::timer::

<Timer minutes="10" />

::image::

![](./images/break.jpg)





---
layout: section
---

# Compounding Engineering

::subtitle::

Improve every session


---
layout: default
---

# Compounding Engineering
## Each session improves the next ones

<CompoundingEngineering :items="[
  { title: 'PLAN',     sub: 'Plan it out in detail' },
  { title: 'DELEGATE', sub: 'Let the agent do the work' },
  { title: 'ASSESS',   sub: 'Make sure it works\n(tests, review)' },
  { title: 'CODIFY',   sub: 'Record what you learned' },
]" />


---
layout: default
---

# Codify
## Record what you learned

- What worked? What broke? What pattern to follow next time?
- 


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


<!--
FourOnARow Result:

My test run didn't work! The BotVsBot worked but human placement didn't!!  
- Empty Button + Background="Transparent" + UniformGrid cell sizing
- Three subtle Avalonia behaviors that combined to produce zero-pixel hit areas.
- Fixed with one additional prompt
- Reason? Avalonia is Windows only; Claude ran in Ubuntu, it could not verify
- **Mitigation? Avalonia.Headless UI test that simulates a pointer press at column-1 coords and asserts a disc state change** -- we will try this!!
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
