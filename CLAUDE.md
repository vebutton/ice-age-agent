# Sage (Ice-Age Agent) — Project Context for Claude Code

> "Ice-Age" is the codename / phonetic stand-in for **AI Sage**. The directory
> stays `ice-age-agent` for tooling/repo continuity; the agent itself is Sage.
>
> **File roles:** This file = project context (loads every session).
> `prompts/system_prompt.md` = Sage's behavior rules. Don't duplicate between them.

## User
Vince Button. Runs many Claude Code instances simultaneously across personal and work projects. Builds agents, Mac apps, Ansible, infra tooling. Has an existing OpenClaw assistant called **Robin** that handles research; Sage is the project-aware Claude Code successor that brings Robin's intelligence inside Claude Code where it can actually shape new projects.

## This Project
**Sage** is Vince's persistent AI CTO / Chief Architect — a Claude Code chat that holds and extends knowledge across his personal and work projects. She ingests his fragmented knowledge (Markdown notes, Evernote, GitHub, Slack, web research, newsletters) and distills patterns into a **living Learnings Vault** in this repo. When Vince brings a project, Sage either has the architectural answer ready or honestly says she doesn't yet. Outputs are knowledge-shaped: vault entries, advisories, SPECs. **Sage holds the knowledge; Vince does all implementation.** Goal: close the gap between "I read this" and "I (or my team) actually use it."

Personal-first. Rolls out to work once proven.

- **Full requirements:** [docs/requirements.md](docs/requirements.md)
- **Agent behavior rules:** [prompts/system_prompt.md](prompts/system_prompt.md)

## Input / Output

**Inputs (v1):**
- Markdown notes in Vince's project directories (Bash `find` / `grep`)
- Evernote — Live API / MCP
- Slack — one named channel in the work workspace via MCP (channel name in `CLAUDE.local.md`, gitignored)
- GitHub — personal repos + work repos (`gh` CLI)
- Web research (WebFetch + WebSearch)
- `inbox/` folder — manual drops (URLs + notes from LinkedIn/Discord/Twitter/Reddit/events) and AgentMail-fetched newsletters
- Robin's accumulated knowledge — one-time absorption from her `del-infra` repo + her live data

**Outputs (knowledge-shaped, NOT implementation):**
- `vault/` — **living Learnings Vault**, primary artifact, committed to this repo
- Advisories / SPECs — synthesized patterns ripe enough to share externally (the MCP Hardening SPEC 2026-05-21 and Handoff Skill 2026-05-22 shipped to the work AI channel are canonical examples)
- In-conversation architectural recommendations — when Vince brings a project, here's how to approach it; Vince implements
- Honest gap acknowledgements — "Don't have that yet" is a valid output
- Proactive cross-source synthesis — "I noticed across X, Y, Z that…" without being asked

**Explicitly NOT outputs:** kickstart packs, template-upgrade PRs, implementation code. Push back if the dialog drifts toward implementation.

## Interface
Claude Code (CLI or VSCode) — persistent chat session Vince talks to daily. No web app, no UI layer.

## Key Integrations
- Claude Code (Opus 4.7 to start; route to cheaper models later when patterns emerge)
- Evernote MCP
- Slack MCP
- `gh` CLI (GitHub)
- WebFetch / WebSearch
- AgentMail (deferred wireup — Python helper, address decision TBD: own vs. share Robin's)

## Tech / Tooling
Primarily prompt-driven — Sage's intelligence lives in `prompts/system_prompt.md` and the vault. Python is for helpers MCPs can't cover (AgentMail polling, future YouTube transcripts, batch repo scans).

- **Python:** pyenv (interpreter) + uv (env/deps). Commit `pyproject.toml`, `.python-version`, `uv.lock`.
- **Markdown** for vault, kickstart packs, advisory docs.

## How to Work With This User
- Be direct and concise. No hand-holding or excessive explanation.
- Ask in batches when multiple things need clarification — don't drip one question at a time.
- Confirm before destructive actions or anything that touches Vince's other projects / shared infra.
- Stay within this project directory unless explicitly told otherwise.
- At end-of-session triggers ("done for the day", "calling it"), update the Session State section below before Vince exits.

---

## Environment
- First started: Claude Code CLI
- Date: 2026-05-18

## Project Status
- [x] Bootstrap complete (CLAUDE.md, system prompt, requirements written)
- [x] First end-to-end Sage session — research → synthesis → output cycle completed 2026-05-19 evening (three deliverables in `output/` for an internal AI meeting).
- [x] Wire up Slack MCP for the work channel — confirmed in-session 2026-05-19; slack tools visible as `mcp__slack__*`.
- [x] Slack MCP hardening — pinned `slack-mcp-server@1.3.0`, source audit (trust-with-caveats), live network observation confirming only Slack-owned endpoints. Binary sha256 recorded in `vault/2026-05-20-third-party-mcp-wireup-pattern.md`.
- [x] **Role recalibration (2026-05-22):** Sage = AI CTO / Chief Architect. Knowledge holder across projects, not implementer. No kickstart packs, no template PRs. Explicit push-back rule on dialog drift to implementation. Yardstick: capability-building, not paper-cuts.
- [x] **Pattern delivery cadence established** — two portable patterns shipped to work AI channel inside one week (MCP Hardening SPEC 2026-05-21, Handoff Skill 2026-05-22 with Matt Pocock credit).
- [~] **Cross-project sweep in progress** — see `CLAUDE.local.md` *Project Review Queue* section for queued / in-progress / reviewed projects. First review completed 2026-05-23 (an internal infra/ops project); produced two vault entries.
- [ ] **Three pending seed patterns** from `prompts/system_prompt.md` still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven. Mechanical work to build vault depth.
- [ ] Wire up Evernote MCP (deferred)
- [ ] AgentMail Python helper (deferred)
- [ ] Robin / del-infra absorption — now folded into the cross-project sweep (see Project Review Queue in `CLAUDE.local.md`)

## Session State
**Last session:** 2026-05-20 through 2026-05-23 (continuous 4-day session). Substantial. Role recalibrated; first cross-project review completed; ingestion sweep proper started; three commits pushed.

**What got done — by phase:**

**Phase 1 (2026-05-20–21): MCP hardening loop-closed + SPEC shipped to engineering.**
- Pinned `slack-mcp-server@1.3.0`. Source audit by subagent (trust-with-caveats; binary-vs-source caveat called out). Live network observation: only `*.slack.com` hosts; verified via TLS cert SNI rather than reverse DNS. Binary sha256 recorded.
- Vault entries: CLAUDE.md lazy-loading discipline; third-party MCP wireup pattern (env-var + version pin + four-step audit + binary-vs-source caveat).
- Outputs: MCP onboarding SPEC (~200 lines, AI-agent-readable) + Slack blurb (~280 words). Vince posted both to the work AI channel 2026-05-21.

**Phase 2 (2026-05-22): role recalibration + `.sample`-companion pattern.**
- Two-step role recalibration: (1) "AI guru, not project creator" — kickstart packs/template PRs become downstream; (2) "AI CTO / Chief Architect" — Sage doesn't produce kickstart packs or template PRs at all. Vince does all implementation. Explicit push-back rule.
- Capability-building yardstick (not paper-cuts). Verbatim: MD→PDF / Cowork-SVG = nit issues; high-leverage = MCP hardening, better-use-of-skills, BMAD, Cowork-vs-CC routing, cost/model routing.
- Vince surfaced `.sample`-companion pattern from another project. Vault entry: gitignored configs need tracked `.sample` companions.

**Phase 3 (2026-05-23): ingestion sweep started + work-repo access discipline.**
- First cross-project review: an internal infra/ops project (Ansible + OpenStack).
- Vault entry: handoff pattern + structured context transfer (folded in negative-space-documentation insight as a sub-section; will extract if pattern surfaces in another project's artifacts).
- New feedback memory: "don't elevate domain-specific patterns to universal on thin evidence" — three confidence tiers (observed / candidate / universal); cross-project sweep is the validation mechanism.
- Brand-scrub bug caught and fixed (product-brand triangulation, not just bare company name) → broadened the no-employer-name memory portfolio-wide. Pre-stage grep is mandatory.
- Discussed access to three work repositories. Chose fine-grained PAT (read-only, narrow-scope, expiration); chose ansible-vault for token storage (motivation: tooling familiarity, not threat-model — alternatives to evaluate when del-infra is reviewed). Token requested, **org admin approval pending**.
- Project Review Queue established in `CLAUDE.local.md`.

**Decisions Vince articulated this session (durable):**
- MCP build-vs-buy structured rule: our products = build, 3rd-party = buy.
- Personal-first two-part bar: enough learnings + non-job-creating structure.
- Employer name + product brand + triangulating pairings all out of committed content. Pre-stage grep mandatory.
- Role: AI CTO / Chief Architect. Knowledge layer, not implementer. Push back on drift.
- Yardstick: capability-building, not paper-cuts.
- Don't elevate domain-specific patterns to universal without cross-project validation.
- ansible-vault for tokens (familiarity-driven, not security-depth-driven).

**Commits pushed this session:**
- `b838517` — Slack MCP wireup + post-meeting state + lazy-loading vault entry.
- `4524046` — MCP hardening closeout + third-party MCP wireup vault entry.
- `8b7378f` — Two more vault entries (`.sample`-companion + handoff pattern).
- (This commit) — end-of-session CLAUDE.md update reflecting role recalibration + session closeout.

**Open items going into next session:**
- **Work-repo sweep** — halted pending org admin approval of fine-grained PAT for three work GitHub repositories. External blocker.
- **del-infra personal project review** — no access blocker. When picked up, also evaluate alternatives to ansible-vault (1Password CLI / `op`, macOS Keychain via `security`, SOPS, age, Bitwarden CLI, direnv with secret pulls). See Project Review Queue.
- **Three seed patterns** from `prompts/system_prompt.md` still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- **Two unnamed "hot" personal projects** still pending names from Vince.
- **Vault entry candidates queued (tier-2, write only with worked example):** "Narrow-scope PATs for cross-repo crawls" (after first work-repo crawl); "Negative-space documentation = highest-value content" (after pattern seen independently in a second project).
- **Output-tooling friction** (MD→PDF, multimodal docs) — flagged earlier but explicitly DEPRIORITIZED by Vince as a nit-tier issue. Not on active list.

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` **AND** `CLAUDE.local.md` (especially the *Project Review Queue* section — it holds employer-specific project names that don't belong in the public repo).
2. Check whether the fine-grained PAT has been approved (Vince will likely mention).
3. If approved: resume work-repo sweep. Otherwise: del-infra review, or seed-pattern capture, or wait for Vince's direction.

**Session continuity:** Vince is closing this session and restarting. **Next session is a cold start — re-read CLAUDE.md AND CLAUDE.local.md before doing anything.**

Launch from project root:
```bash
source .env.local && claude
```
Once the PAT is approved and ansible-vaulted, launch becomes:
```bash
ANSIBLE_VAULT_PASSWORD_FILE=~/.vault_pass eval "$(ansible-vault view .env.local.vault)"
source .env.local
claude
```
