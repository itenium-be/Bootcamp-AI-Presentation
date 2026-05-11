# AI Driven Development: From Vibe Coding to Agentic Engineering

*A comprehensive summary of the itenium bootcamp presentation (≈ 5-minute read)*

---

## The Big Picture

The era of "prompt engineering" as a standalone job title (circa 2023) is over. What matters now is a broader set of engineering disciplines that determine how effectively you collaborate with AI coding agents. This presentation lays out a **nine-level maturity ladder** — from manual coding all the way up to autonomous agent teams — and deep-dives into the three disciplines that drive real upward movement: Context Engineering, Compounding Engineering, and Harness Engineering.

A recurring case study — modernizing a VB.NET WinForms "Four on a Row" game into C#/.NET 10/Avalonia using the open-source **Superpowers** skills framework — grounds every concept in a live, working demo.

---

## The Maturity Ladder

The ladder runs through nine levels:

1. Manual coding
2. Tab-complete & chat copy/paste
3. Agent IDE (heavy use of plan mode)
4. **Context Engineering**
5. **Compounding Engineering**
6. MCP & Skills
7. **Harness Engineering**
8. Background Agents
9. Autonomous Agent Teams

Most developers today sit somewhere between levels 2 and 4. The presentation argues that levels 4, 5, and 7 are the high-leverage inflection points that unlock everything above them. Background agents (level 8) are "where we're at in 2026" — once context, compounding, and harness engineering are solid, you no longer need to babysit the plan.

---

## Context Engineering — Designing What the Model Sees

The central insight: **LLMs are stateless; the harness is where state lives.** Every turn sends the entire context to the model. That context window is your memory budget — spend it wisely.

### Five Sub-Disciplines

- **Memory Budgeting** — What have you already spent before you even start? System prompt, CLAUDE.md, MCP tool schemas, skills index, and memory all consume tokens before the user types anything.
- **Instruction Layering** — Where you place instructions matters. The "Lost in the Middle" problem (backed by research from arXiv and ETH Zurich 2026) means instructions buried deep in a large CLAUDE.md get followed less reliably. Keep important rules at the top or push them into hooks (which inject right next to the action).
- **Retrieval Strategy** — Claude Code uses agentic grep/read (precise, current), while Cursor and Copilot lean on RAG (fast but index goes stale). The "tool ladder" is: glob to narrow scope → grep to find symbols → read only when full files are needed. For noisy retrievals, delegate to a sub-agent that returns a summary.
- **Eviction Policy** — Monitor context usage (`/statusline`). Start thinking about a new session at 40% usage. Prefer `/clear` (clean restart) over `/compact` (lossy compression). Sub-agents sidestep eviction by running in their own context window.
- **Tool-output Shaping** — Return only `✓` on success. Verbose success logs eat context for nothing; save the words for failures.

### Progressive Context Disclosure

A recurring pattern: thin index loaded upfront → full detail on demand. This shows up in CLAUDE.md (≈100 lines, table of contents style), Memory (MEMORY.md index with topic files read on-demand), and Skills (only name + description in every context; body loads on invocation).

### Practical Takeaways

- Big `CLAUDE.md` files are harmful — don't add what's inferable from the code.
- MCPs give agents access to state they can't otherwise reach (Figma designs, Sentry errors, live databases) but every tool schema is "rent" — audit with `/mcp` periodically.
- Consider CLIs over MCPs: a bash invocation has zero schema cost.
- Don't paste 200-line stack traces into chat — extract the 5 lines that matter.

---

## Compounding Engineering — Each Session Should Improve the Next

The workflow follows a four-step loop: **Plan → Delegate → Assess → Compound.** The "Compound" step is the differentiator — after each feature, you record what you learned as machine-readable artifacts, not human wiki pages that get forgotten.

### Targets of Codification

When you find yourself correcting the agent the same way twice, it's time to codify:

- **Skill** — domain-specific guidance (e.g., "Investigating flaky tests in our pipeline").
- **Slash Command** — a multi-step workflow that repeats.
- **Hook** — behavior that must be enforced, not requested.
- **Sub-agent** — review/research that can fan out in parallel.
- **CLAUDE.md** — always-relevant project context.

### Skills as Living Documentation

Skills live in `<project>/.claude/skills/` (team) or `~/.claude/skills/` (personal). Block's three design principles apply: descriptive name (discoverable from the index), scoped trigger (clear "when does this apply?"), and layered detail (overview first, references on-demand). The same skill file that guides the AI also onboards a new human teammate.

### The Compounding Payoff

> "AI engineering makes you faster today. Compounding makes you faster tomorrow, and each day after." — Kieran Klaassen

Without codified knowledge, agents make the same mistakes forever. The presentation reframes documentation as "Skills" and notes that developers who wouldn't write docs will happily write skills — same artifact, new name.

---

## Harness Engineering — Automated Feedback Loops

The model can forget instructions, skip steps, or rationalize deviations. Hooks are deterministic commands the model **cannot bypass**.

### Hooks in Practice

Claude Code hooks fire on lifecycle events:

- **PreToolUse** — block dangerous edits. The TDD gate script checks if a failing test exists before allowing source edits. Test files themselves are always allowed (otherwise you can't write the failing test).
- **PostToolUse** — auto-run `tsc --noEmit && eslint --quiet` after every edit. Errors flow back to the agent like it ran the linter itself.
- **UserPromptSubmit** — inject context, validate scope.
- **SessionStart** — set up memory, kick off background tasks.

### Agent Backpressure

Give the agent a structured feedback loop:

- **LSP diagnostics** — squiggly lines for the agent (go-to-def, find-refs, type info — not grep + guess).
- **Type checks** with hardest constraints enabled.
- **Tests** with coverage gates (90%? 95%? 99%?).
- **Linting & formatting** so all generated code looks identical.
- **Visual inspection** via Playwright MCP — give the agent "eyes."
- **Observability tooling** — Sentry MCP for production errors.

### TDD Is No Longer Optional

The presentation makes a strong claim: for the last 20 years TDD was a choice; today, if you're not doing TDD with AI, you're doing it wrong. Tests have become extraordinarily cheap to write. A full-stack project (frontend + gateway + backends + databases) that was once a two-month testing effort is now essentially one prompt — and after the first project, you codify it as a skill for future use.

---

## Code Reviews in the AI Era

With agents generating code at unprecedented speed, review becomes the new bottleneck. The presentation advocates:

- **Never let the model rubber-stamp itself** — the generating agent is too invested in its own code. Use a fresh context (same LLM, new session) or a different model entirely.
- **Multi-agent review** — fan out specialized reviewers (security, performance, integration, complexity) as parallel sub-agents.
- **Human role** — arbitrate disagreements and sign off on architecture, not line-by-line inspection.

---

## The Dark Factory (Aspirational)

StrongDM's vision: a development pipeline where humans only set direction. Agents write, agents review, agents merge. Humans monitor metrics, not commits. The presentation treats this as aspirational for 2026 but acknowledges it's the direction the industry is heading.

---

## Industry Validation

The Thoughtworks Tech Radar Vol. 34 independently validates the presentation's themes:

| Ring   | Blip                               | Presentation Section         |
|--------|-------------------------------------|------------------------------|
| Adopt  | Mutation testing                    | Harness · TDD                |
| Trial  | Superpowers                         | Case Study                   |
| Trial  | Agent Skills                        | Context · Skills             |
| Trial  | Feedback sensors for coding agents  | Harness · Backpressure       |
| Trial  | Spec-driven development             | Compounding · Spec/Plan      |
| Hold   | Agent instruction bloat             | Context · Lost in the Middle |

---

## What's Actionable Today

1. **Install Superpowers** — get brainstorming, TDD, debugging, and code-review skills out of the box.
2. **Write one skill** — the thing you do every day that the agent always gets wrong. That's your first compound interest payment.
3. **Wire up LSP via MCP** (e.g., Serena ⭐24k) — the agent gets go-to-def, find-refs, and type info instead of grep + guess. An MCP that earns its rent.

---

*Humans steer. Agents execute.*
