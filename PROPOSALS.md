# Plugin & skill proposals

Status as of 2026-10-14, written against Camelia-Agent main (post catalog search, PR #11).

## Channel status (measured, not assumed)

| Channel | Status | Evidence |
| --- | --- | --- |
| Skills | **Open and verified** | Two shipped samples (`catalog-maintainer`, `quiet-shell-notes`); install path `~/.camelia/skills` works off this catalog |
| Theme plugins | Open, one sample | `quiet-activity-bar`, `kind: "theme"`, `entry: theme.json`, `permissions: []` |
| Tool plugins | **No schema sample yet** | No plugin in this repo declares a tool; loading behavior for non-theme kinds is unverified |

## Plugin candidates, ranked

1. **git (read-only)** — `git.status` / `git.diff` / `git.log`. Highest value: feeding a real diff to a weak model beats relying on its memory of prior edits; read-only verbs map cleanly onto the existing approval policy (auto for reads, ask for writes). Blocked on: a tool-kind plugin schema sample.
2. **web.read** — fetch one page's readable text. `web_search` exists but cannot open a specific URL; needed for docs, papers, contest statements. Same blocker.
3. **provider.ping** — probe model providers for availability/latency/rate limits. Feeds the cognition layer's environment predictions (Iteration 27 direction) and the fallback-port downgrade decision. Same blocker.
4. **session export** — replay the events table into a shareable Markdown/HTML debrief. The event-sourcing store makes this cheap; mostly UI/CLI work, possibly not a plugin at all.
5. **LaTeX compile** — deliberately **not** proposed as a plugin: `shell.exec` already runs xelatex; ~80% of the value is in the log-parsing-and-repair routine, which a **skill** delivers today (see `cumcm-format-checklist` for the pattern; a `latex-build-fix` skill would follow the same shape).

## Recommendation

- Ship skills now; they are verified end to end and carry the near-term value.
- Do **not** hand-write a tool plugin before one official tool-kind sample lands — an untestable guess is exactly the "declared but never effective" failure mode the project's discipline exists to prevent.
- When the tool schema lands, start with git read-only; it is the smallest surface with the clearest reliability payoff.
