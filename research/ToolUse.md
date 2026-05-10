Hooks — research notes
======================

Backing material for the `Hooks in practice` slide. Two example hooks, full
scripts, schema notes, performance trade-offs.


## The two example hooks

### PreToolUse — TDD gate

Block source edits when no test is currently failing. Test files are always
allowed (otherwise you can't write the failing test).

`scripts/tdd-gate.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

file="${1:-}"

# Test files always pass through
case "$file" in
  *test*|*spec*|*Test.cs|*Tests.cs|*.test.*|*.spec.*) exit 0 ;;
esac

# Adapt to your project's test runner
if bun test --bail >/dev/null 2>&1; then
  echo "All tests pass. Write a failing test before editing source." >&2
  exit 2
fi

exit 0
```

Exit-code semantics for `PreToolUse`:

- `0` — allow the tool call
- `2` — block; stderr is sent back to the model as a system message
- non-zero non-2 — error; behavior depends on Claude Code version


### PostToolUse — type-check + lint

Surface diagnostics back to the model after every Edit or Write.

```json
"PostToolUse": [{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "tsc --noEmit && eslint --quiet"
  }]
}]
```

The model sees the squiggles you see. Stdout/stderr is injected as a
follow-up system message; failures are surfaced as actionable feedback,
not silent.


## Settings.json shape

Full file — concrete starting point:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          { "type": "command", "command": "scripts/tdd-gate.sh" }
        ]
      },
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "scripts/block-dangerous-bash.sh" }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          { "type": "command", "command": "tsc --noEmit && eslint --quiet" }
        ]
      }
    ]
  }
}
```


## Lifecycle events

| Event              | Fires                                  | Common uses                                              |
| ------------------ | -------------------------------------- | -------------------------------------------------------- |
| `PreToolUse`       | Before a tool runs                     | Block dangerous edits, enforce TDD, gate `Bash` commands |
| `PostToolUse`      | After a tool succeeds                  | Auto-format, run linter, surface type errors             |
| `UserPromptSubmit` | Before user prompt enters context      | Inject project context, validate scope                   |
| `SessionStart`     | Beginning of a session                 | Set up memory, start background tasks, print MOTD        |


## Performance — full suite on every edit is brutal

Mitigations, in order of preference:

1. Run the smallest fast subset that proves the contract.
   - `bun test --changed`
   - `vitest --changed`
   - `pytest --testmon` / `pytest -k <related>`
2. Use a watch process and check its last status, instead of running tests inline.
3. Type-check only the affected file: `tsc --noEmit <path>` (slower for project
   references, faster for flat projects).
4. For monorepos: scope `eslint` to the changed file path explicitly.

If the hook takes more than ~2-3s, the agent feels sluggish and the audience
notices on stage.


## Schema variations across versions

Hook env shape has shifted across Claude Code releases. Verify against your
own installed version before paste:

- Older versions passed file paths as positional args (`$1`, `$2`).
- Newer versions pass a single JSON document on stdin or in `$CLAUDE_TOOL_INPUT`,
  with the file path nested inside the tool params.

Smoke test: write a hook that simply does `env > /tmp/hook.env; cat > /tmp/hook.in`
and trigger it once. Inspect the files. Then write the real hook around the
shape you actually see.

The slide JSON is intentionally minimal — the complexity lives in the script,
not the settings file.


## Why this beats `CLAUDE.md` instructions

The `CLAUDE.md` failure mode: the model can forget, skip, or rationalize away
any rule you wrote in prose. Concrete examples:

- "Never `git push --force` to main" — the model decides "this is the exception."
- "Always write a failing test first" — the model decides "this is too small to need a test."
- "Don't add `Co-Authored-By` to commits" — the model adds it anyway, you correct, it adds it the next session.

Hooks remove the choice. The action either runs or it doesn't. The model can't
talk its way out.

A useful test: anything you've corrected the agent on twice is a hook candidate.


## Companion patterns

- **Tool-output Shaping** — print `✓` on success; only return text on failure.
  Cuts context burn on no-news outcomes. Pairs naturally with `PostToolUse`.
- **Subagent fan-out** — for expensive checks (security review, architectural
  audit), don't run as a hook; run as a subagent in its own context window.
- **Statusline** — `~/.claude/settings.json` `statusLine` shows context % and
  recent hook results, which is how you'll discover when a hook is failing
  silently.


## Sources

- [Claude Code Docs — Hooks](https://code.claude.com/docs/en/hooks)
- [paddo.dev — Claude Code Hooks Guardrails](https://paddo.dev/blog/claude-code-hooks-guardrails/)
- `RESEARCH.md` §5 (Harness Engineering)
- `NOTES.md` (Harness Engineering link)
