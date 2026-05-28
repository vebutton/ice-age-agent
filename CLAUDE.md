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

Used across both personal and work projects (currently ~70% work — work hours dominate). Sage attribution stays low-profile at work — outputs ship under Vince's name; AI-interested colleagues know the high-level gist without a formal pitch.

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
- [x] **Mobile-app research baseline (2026-05-24):** Matt Pocock's 7-step workflow synthesized from `collateral/HOW do you engineer with AI_.eml` + `mattpocock/skills` open-source catalogue mapped. 5/7 steps directly covered by skills; Step 6 (Ralph loops / execution) is the cohort's residual value as a *pattern*, not a skill. Mobile substitutions identified for the two side projects: Expo before Step 4; slices-not-walk-away at Step 6; vendor-API reconnaissance at Step 2 for the Android solar/EV-charging app.
- [x] **AI notebook one-shot ingestion (2026-05-24):** 21 Evernote notes synthesized to `output/20260524-ai-notebook-synthesis.md` via sub-agent; 32-item drop list at `output/20260524-ai-notebook-drop-list.md` pending Vince's review. Evernote MCP remains correctly deferred — notebook is input-dump not memory, no value in live sync yet.
- [x] **Structured-writeup pattern vaulted (2026-05-24):** `vault/2026-05-24-agent-mediated-structured-writeup.md` — domain-conventions-encoding agent + multiplicative compression vs. a manual baseline (worked-example details kept in gitignored memory). HITL calibration listed as the open question feeding the harness experiment.
- [~] **Work harness experiment scoped (2026-05-24):** primary deliverable is the *process writeup* (cost-per-output / HITL calibration / 7-step application notes), NOT the final tool. MVP candidate selected and held in gitignored memory + synthesis output, pending Vince's confirmation. Operational Qs on the work CC access path tracked in memory.
- [~] **Cross-project sweep in progress** — see `CLAUDE.local.md` *Project Review Queue* section for queued / in-progress / reviewed projects. First review completed 2026-05-23 (an internal infra/ops project); produced two vault entries.
- [ ] **Three pending seed patterns** from `prompts/system_prompt.md` still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven. Mechanical work to build vault depth.
- [ ] Wire up Evernote MCP (deferred)
- [ ] AgentMail Python helper (deferred)
- [ ] Robin / del-infra absorption — now folded into the cross-project sweep (see Project Review Queue in `CLAUDE.local.md`)

## Session State
**Last session:** 2026-05-24 (single day; substantial). Four phases — Matt Pocock 7-step research, memory hygiene + cost-model clarification, Evernote AI notebook one-shot ingestion, work harness experiment scoping + structured-writeup pattern vaulted. **6 new memories** banked.

**What got done — by phase:**

**Phase 1: Matt Pocock 7-step research baseline.**
- Read `collateral/HOW do you engineer with AI_.eml` (the AI Hero cohort pitch). Synthesized the 7-step workflow (Idea → Research → Prototype → PRD → Kanban → Execution → QA). Thesis: "people problems" don't change with AI; explicit rebuttal of Sean Grove's "Specs Over Code" as waterfall-rebranded.
- For the two side projects (Android solar/EV-charging + iOS storyteller / Mum's stories), identified three mobile-specific substitutions: pick Expo before Step 4; treat Step 6 as "slices not walk-away" on mobile (no Simulator-driving agent yet); vendor-API-access reconnaissance at Step 2 is the real gate for the solar/EV app.
- Investigated whether Matt's skills are open-source. They are — `mattpocock/skills` is canonical (one-line install). 17 skills total; 9 engineering. Mapped to the 7 steps: 5/7 directly covered (Idea / PRD / Kanban / QA via tdd+diagnose+triage, plus bonus `improve-codebase-architecture`). NOT skills: Research, Prototype, Execution (Ralph loops). Step 6 is the cohort's residual value as a *pattern*. Matt explicitly critiques BMAD/GSD/Spec-Kit in the README.
- Initial recommendation: install Matt's skills as-is (free), write own ~50-line headless loop instead of the $1K cohort. That recommendation got revised in Phase 4 once Vince noted `claude -p` is API-billed.

**Phase 2: memory hygiene + cost-model clarified.**
- Banked new memories this session, several driven by Vince's explicit corrections:
  - `feedback-vault-vs-memory-distinction` — Vault = concrete portable artifacts; memory = behavioral rules. Cataloguing concrete artifacts is single-source-OK vault content, exempt from the cross-validation rule in [[feedback-pattern-domain-specificity]].
  - `feedback-cc-subscription-vs-api-cost` — `claude -p` is API-billed; interactive CC + Task sub-agents are subscription-included. For unmonetized projects = interactive only. Per-project monetization is the flag.
  - `feedback-filename-date-convention` — Compact `YYYYMMDD-topic.ext`. Vault keeps existing dashed convention.
  - `feedback-personal-business-info-out-of-checkins` — Personal-business / monetization-plan details stay out of committed content. Same pre-commit grep discipline as [[feedback-no-company-name-in-checkins]].
  - `feedback-no-company-name-in-checkins` was *broadened* this session: internal infrastructure topology + subscription tier/account specifics + internal artifact/agent names + internal MVP candidate names also belong out of committed files. Memory + `CLAUDE.local.md` hold the fidelity.

**Phase 3: Evernote AI notebook one-shot ingestion.**
- Vince exported AI notebook as `.enex` (21 notes); dropped at `inbox/20260524-AI-notebook.enex` (gitignored).
- Sub-agent extracted structured summary → `output/20260524-ai-notebook-synthesis.md` (gitignored). Index + 4 warmup MVP candidates scored + side-project context dumps + 9 research-debt clusters + 10 surprise patterns.
- Drop list written to `output/20260524-ai-notebook-drop-list.md` — ~32 items across R1 (agentic-runners FOMO), R4 (on-prem GPU, by Vince's own prior ruling), R8 (lifestyle agent stubs), R9 (webinar attendance logs). Pending Vince's checkbox-review pass.
- Cross-cutting observation: **voice/transcription is a unifying primitive** under multiple captured ideas (Mum stories + several voice-capture workflows whose specifics stay in gitignored memory). Recommendation: standardize on one (superwhisper is the front-runner by mention-count).
- Meta-observation: **Evernote lags this repo** — the last two weeks of Sage work didn't make it back to Evernote. Confirms Evernote is input-dump, not memory. Evernote MCP wireup remains correctly deferred.

**Phase 4: work harness experiment scoped + structured-writeup pattern vaulted.**
- Vince surfaced internal-context corrections that reshaped both the experiment design and the MVP candidate selection. Details kept in gitignored memory + `CLAUDE.local.md` per the broadened [[feedback-no-company-name-in-checkins]] rule.
- Experiment design sharpened: it's not "does the loop work" — it's **"what's the cost-per-output curve as HITL relaxes?"** Instrument from day one (per-call token logs + intervention count + wall-clock). Pre-declare manual baseline before runs. Time-box harness runs (overnight/weekend) to separate signal from everyday CC usage.
- MVP recommendation finalized; candidate held in gitignored memory + synthesis output. Repo-archive-triage stays optional as a half-day harness shakedown.
- Vault entry: `vault/2026-05-24-agent-mediated-structured-writeup.md` — pattern shape (encode domain conventions → accelerate first draft); portable to blogs, briefs, security reviews, CFPs, etc.; HITL calibration listed as the open question. Indexed under Agent Architecture.

**Decisions Vince articulated this session (durable):**
- **Filename date convention is compact `YYYYMMDD`** (vault exception keeps dashes).
- **Vault vs. memory rule:** vault = concrete portable artifacts; memory = behavioral rules. Cataloguing concrete artifacts is exempt from the cross-validation tier-rule.
- **`claude -p` cost model:** API-billed per token; for unmonetized projects = interactive only. For work, check the existing CC subscription's API-credit allowance before framing as a new budget ask.
- **"Process value > tool value"** for the harness experiment — primary deliverable is the *process writeup* (cost-per-output / HITL calibration / 7-step application notes), not the artifact built along the way.
- **Don't pay $1K for Matt's cohort.** Skills are open-source; the residual value is the Step 6 (Ralph loops) pattern, which is well-understood orchestration over CC headless and writeable in-house.

**Commits pushed this session:**
- (Pending at session close — committable changes are `vault/2026-05-24-agent-mediated-structured-writeup.md` NEW, `vault/INDEX.md` modified, `CLAUDE.md` modified. Awaiting Vince's go-ahead.)

**Open items going into next session:**
- **MVP decision for the work harness experiment** — Vince said "Let me think this over a bit." Candidates and rankings held in gitignored memory + synthesis output. One gets picked.
- **Operational Qs on the work CC access path** — tracked in memory. One DM (not a meeting) once Vince is ready.
- **32-item drop list at `output/20260524-ai-notebook-drop-list.md`** — pending Vince's checkbox-review pass.
- **One aged internal-tool TODO** (Evernote note 1, carried over from earlier work-account migration; specifics held in gitignored memory) — explicit decision whether to fold into the harness experiment or drop.
- **Mum stories / Mind Maker app** — needs a 30-min brain-dump before any scoping. One line of Evernote context exists; not enough.
- **Voice/transcription unifier** — chase & standardize. superwhisper front-runner.
- **Sage's Evernote stub (note 3) lags the repo definition** — update Evernote or treat as time-capsule of the original "find reusable AI skills" framing.
- **Work-repo sweep** (carried from prior session) — still halted pending org admin approval of fine-grained PAT. External blocker.
- **del-infra personal project review** (carried) — no access blocker; queued. Evaluate ansible-vault alternatives when picked up.
- **Three seed patterns** (carried) still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- **Two unnamed "hot" personal projects** (carried) — **likely now named:** Android solar/EV-charging app + Mum stories iOS app surfaced in this session's Evernote ingestion. Confirm with Vince.
- **Vault entry candidates queued (tier-2):** "Narrow-scope PATs for cross-repo crawls" (after first work-repo crawl); "Negative-space documentation = highest-value content" (after pattern seen independently); "Two-phase de-risking for harness experiments at work" (after the harness MVP completes).

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` **AND** `CLAUDE.local.md` (Project Review Queue + work channel name live there).
2. Check (a) Vince's warmup MVP pick, (b) credit-pool answers if he asked the admin, (c) PAT approval status (carried from prior session).
3. If MVP picked: scope first slice. Otherwise: drop-list review, Mum stories brain-dump, seed-pattern capture, or wait for direction.

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
