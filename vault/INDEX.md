# Learnings Vault — Index

Sage's accumulated wisdom. One line per entry. Add the entry, then add a row here.

Format: `- [YYYY-MM-DD — Title](YYYY-MM-DD-slug.md) — one-line hook. *Status: seed | confirmed | promoted-to-template | obsolete*`

---

## Skills & MCPs

- [2026-05-18 — Git commits need a Claude skill, not per-project HEREDOC rediscovery](2026-05-18-git-commit-skill-needed.md) — file-based commit messages over inline / HEREDOC. Rule promoted to global `~/.claude/CLAUDE.md` 2026-06-10; skill deferred until a `git-ops` bundle has capability mass (single tweak too small to carry a skill). *Status: resolved — promoted-to-global-rule; skill deferred*
- [2026-05-20 — CLAUDE.md stays lean — let the harness lazy-load skills and MCP tools](2026-05-20-claude-md-stays-lean-lazy-loading.md) — don't enumerate MCP tool inventories or full skill descriptions in CLAUDE.md; the harness already lazy-loads both. *Status: seed*
- [2026-05-20 — Third-party MCP wireup pattern: env-var token, version pin, audit-then-trust](2026-05-20-third-party-mcp-wireup-pattern.md) — `${VAR}` from gitignored `.env.local` + pin (never `@latest`) + four-step audit + binary-vs-source caveat. Worked example: Slack MCP `1.3.0`. *Status: seed*
- [2026-06-09 — Skills are extracted from demonstrated reuse, not anticipated reuse](2026-06-09-skills-extracted-not-anticipated.md) — build capabilities concretely; recognize skill-shape only when a second project needs the same thing; extract then. Rule-of-three discipline applied to skill creation. Worked examples: `handoff`, `access-openstack`. Anti-pattern: skill-ifying for the sake of skill-ifying. *Status: seed (single-source)*

*(other seeds named in `prompts/system_prompt.md` "Patterns Vince has already named" still to capture: skills-complement-MCPs, master-agents-self-architect, Context-Seven)*

## Tooling & External References

- [2026-05-18 — Bootstrap templates shouldn't ship their own process doc to children](2026-05-18-template-shouldnt-ship-process-doc-to-children.md) — `project-bootstrap-process.md` belongs in `claude-agent-template`, not every child repo. Candidate template-upgrade PR. *Status: seed*
- [2026-05-22 — Gitignored repo-local configs need tracked `.sample` companions](2026-05-22-gitignored-configs-need-tracked-samples.md) — gitignoring a config without shipping its `.sample` makes fresh clones archaeology. Generalizes `.env.local.example` to any proprietary config (CLAUDE.md, vars/main.yml, etc.). Candidate template-upgrade PR. *Status: seed*
- [2026-06-10 — Claude Fable 5 capability profile + free-window deadline](2026-06-10-claude-fable-5-capability-profile.md) — `claude-fable-5`, Mythos-class, SOTA agentic coding (SWE-Bench Pro 80.3%, Terminal-Bench 88%); file-based memory 3× over Opus 4.8 (validates on-demand routing); free on Pro June 9–22 then usage-credit. Auto-fallback to Opus 4.8 on cyber/bio/distillation. **RESTRICTED 2026-06-26 — Fable withdrawn at US-Govt direction (temporary, pending Anthropic securing it); all projects on Opus 4.8. Profile is historical.** *Status: reference — restricted*

## Agent Architecture

- [2026-05-23 — Handoff documents: structured context transfer when fresh context beats accumulated context](2026-05-23-handoff-pattern-structured-context-transfer.md) — 11-section handoff doc + fresh second-session pattern; "What's been ruled out" is the highest-value section (distinctive to Vince/Pocock branch, not universal). Sourced from an internal infra/ops project review + Matt Pocock + steipete/agent-scripts three-way convergence. *Status: confirmed (2026-05-27)*
- [2026-06-10 — Negative-space documentation: record what was ruled out, not just what was done](2026-06-10-negative-space-documentation.md) — "X doesn't work because Y" outlives "we did Z"; fresh agent contexts re-explore undocumented dead ends at full cost, and positive-only docs are hallucination bait. Extracted from the handoff entry's negative-space insight; second branch: a standing agent-context doc in a work K8s-operator repo. *Status: seed (multi-source observed)*
- [2026-05-24 — Agent-mediated structured writeup: encode domain conventions, accelerate first draft](2026-05-24-agent-mediated-structured-writeup.md) — dedicated agent encodes section structure + phrasing conventions + reviewer expectations for a recurring writeup type; author moves from "writing from scratch" to iterating on a strong first draft. Worked example: 30 min vs ~2 weeks on a vendor questionnaire. HITL calibration is the open question — candidate harness experiment to provide data. *Status: seed*
- [2026-06-09 — On-demand context routing: front-loaded summary, lazy-loaded detail](2026-06-09-on-demand-context-routing.md) — always-loaded layer (CLAUDE.md, watchlist pointer) carries summary + routing logic; full detail in lazy-loaded files (docs/, local/) consulted on demand. Three worked examples (multi-file work-repo split, Sage's `local/` watchlists, this commit's Session State split). Counter-example: monolithic CLAUDE.md is the right call for bounded-scope. *Status: seed (multi-source observed)*
- [2026-06-17 — In-repo coding harness: subagent + runtime skill + on-demand memory + guardrail + enforcement hooks](2026-06-17-in-repo-coding-harness.md) — five composable pieces laid before the first line of code: a domain coding subagent (launched by the normal Task tool; delegate-or-not logic in its `description`/`<example>` blocks), a thin conventions/runtime skill it delegates standards to, project-scoped agent memory loaded on demand via a MEMORY.md index (prompt-driven, not hook-injected; staleness-checked before recall), a fixed `karpathy-guidelines` guardrail skill, and exit-2-on-Stop enforcement hooks. Ship the mechanism + guardrail + templates fully-formed; ship the conventions-skill body as a skeleton (pre-writing it = anticipated reuse). Worked example: an internal Go/k8s-controller repo (feature branch, prototype-stage). Companion shareable spec for Inception in `output/`. *Status: seed (single-source)*
- [2026-06-02 — Pocock's 7-step engineering workflow: a lightweight alternative to spec-first](2026-06-02-pocock-7-step-engineering-workflow.md) — Idea / Research / Prototype / PRD / Kanban / Execution / QA. Several optional. Anti-Specs-Over-Code / anti-BMAD stance. 5/7 steps backed by skills in `mattpocock/skills`; Step 6 (execution) is residual pattern value. First executed worked example (Step 1, `grill-with-docs`) recorded with positive outcome from a personal-track project. *Status: seed (single-source)*

## Work-specific

*(no entries yet)*
