# Learnings Vault — Index

Sage's accumulated wisdom. One line per entry. Add the entry, then add a row here.

Format: `- [YYYY-MM-DD — Title](YYYY-MM-DD-slug.md) — one-line hook. *Status: seed | confirmed | promoted-to-template | obsolete*`

---

## Skills & MCPs

- [2026-05-18 — Git commits need a Claude skill, not per-project HEREDOC rediscovery](2026-05-18-git-commit-skill-needed.md) — file-based commit messages over inline / HEREDOC; build a `git-commit` skill. *Status: seed*
- [2026-05-20 — CLAUDE.md stays lean — let the harness lazy-load skills and MCP tools](2026-05-20-claude-md-stays-lean-lazy-loading.md) — don't enumerate MCP tool inventories or full skill descriptions in CLAUDE.md; the harness already lazy-loads both. *Status: seed*
- [2026-05-20 — Third-party MCP wireup pattern: env-var token, version pin, audit-then-trust](2026-05-20-third-party-mcp-wireup-pattern.md) — `${VAR}` from gitignored `.env.local` + pin (never `@latest`) + four-step audit + binary-vs-source caveat. Worked example: Slack MCP `1.3.0`. *Status: seed*

*(other seeds named in `prompts/system_prompt.md` "Patterns Vince has already named" still to capture: skills-complement-MCPs, master-agents-self-architect, Context-Seven)*

## Tooling & External References

- [2026-05-18 — Bootstrap templates shouldn't ship their own process doc to children](2026-05-18-template-shouldnt-ship-process-doc-to-children.md) — `project-bootstrap-process.md` belongs in `claude-agent-template`, not every child repo. Candidate template-upgrade PR. *Status: seed*
- [2026-05-22 — Gitignored repo-local configs need tracked `.sample` companions](2026-05-22-gitignored-configs-need-tracked-samples.md) — gitignoring a config without shipping its `.sample` makes fresh clones archaeology. Generalizes `.env.local.example` to any proprietary config (CLAUDE.md, vars/main.yml, etc.). Candidate template-upgrade PR. *Status: seed*

## Agent Architecture

- [2026-05-23 — Handoff documents: structured context transfer when fresh context beats accumulated context](2026-05-23-handoff-pattern-structured-context-transfer.md) — 10-section handoff doc + fresh second-session pattern; "What's been ruled out" is the highest-value section. Sourced from an internal infra/ops project review + Matt Pocock convergent-discovery footnote. *Status: seed*
- [2026-05-24 — Agent-mediated structured writeup: encode domain conventions, accelerate first draft](2026-05-24-agent-mediated-structured-writeup.md) — dedicated agent encodes section structure + phrasing conventions + reviewer expectations for a recurring writeup type; author moves from "writing from scratch" to iterating on a strong first draft. Worked example: 30 min vs ~2 weeks on a vendor questionnaire. HITL calibration is the open question — candidate harness experiment to provide data. *Status: seed*

## Work-specific

*(no entries yet)*
