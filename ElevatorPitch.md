AI Driven Development
=====================

*From Vibe Coding to Agentic Engineering*


## Abstract

What changes when AI does the typing. A practical tour of context engineering (the lever that moves you up the AI maturity ladder), compounding engineering (each session writes the playbook the next agent reads), and harness engineering (deterministic guardrails the model can't talk its way out of). A live Superpowers demo runs on a real modernization in parallel — we check in at the end.


## Target Audience

Bootcamp grads and Claude-Code-curious developers. Mixed levels — from chat-and-paste up through MCPs and skills. Anyone who's used Claude Code and felt either "this is magic" or "this keeps getting it wrong" will find their current rung on the maturity ladder and a path one rung up.


## Key Takeaways

Three concrete changes for Monday morning:

- **Install Superpowers** — brainstorming, TDD, debugging, code review and plan-writing skills, today
- **Write one skill** — codify the thing the agent gets wrong every week; that's your first compound-interest payment
- **Wire up an LSP MCP** — give your agent go-to-def, find-refs, type info, instead of grep-and-guess

Plus the mental models:

- Context engineering as the lever, not a separate topic
- Hooks > instructions for anything you want enforced every time
- "Humans steer. Agents execute."


## Session Format

120 minutes. Four sections (Maturity Ladder · Context Engineering · Compounding Engineering · Harness Engineering), one live demo running throughout, one 10-minute break.


## Summaries

### One Sentence

This presentation maps out a maturity ladder from basic AI-assisted coding to autonomous agent teams, arguing that the real leverage comes from **Context Engineering** (curating what the model sees), **Compounding Engineering** (codifying learnings into skills/hooks so each session improves the next), and **Harness Engineering** (deterministic feedback loops like TDD gates and linting hooks that the model cannot bypass).

### Five Sentences

The presentation introduces a nine-rung **AI Maturity Ladder** — from manual coding up to autonomous agent teams — and identifies Context Engineering, Compounding Engineering, and Harness Engineering as the three disciplines that move you up it. **Context Engineering** is about treating the LLM's context window as a scarce budget: keep CLAUDE.md lean, use progressive disclosure (skills load on demand, not upfront), prefer CLIs over schema-heavy MCPs, and offload noisy reads to sub-agents that return summaries instead of raw output. **Compounding Engineering** follows a Plan → Delegate → Assess → Compound loop where every session's learnings are codified into skills, hooks, or CLAUDE.md entries so the *next* agent session starts smarter — "corrected the same thing twice? It doesn't belong in chat." **Harness Engineering** makes quality deterministic rather than optional: PreToolUse hooks enforce TDD (no source edit without a failing test), PostToolUse hooks auto-run type checks and linters feeding errors back as agent context, and structured backpressure (LSP diagnostics, coverage gates, Playwright screenshots) keeps the flood of generated code under control. A live case study modernizing a VB.NET WinForms game to C#/Avalonia using the *Superpowers* skills framework ties it all together, demonstrating brainstorming → spec → plan → parallel sub-agent execution and multi-perspective code review as a practical workflow teams can adopt today.

### Five Minutes

See [FiveMinSummary.md](FiveMinSummary.md).
