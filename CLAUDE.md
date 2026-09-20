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
- AgentMail (wired 2026-06-02 — Sage's own dedicated inbox, address in `CLAUDE.local.md`; on-demand Bash+curl via `AGENTMAIL_API_KEY` in `.env.local`. Vendor MCP / Claude Skill both available; adoption deferred — see Project Status.)

## Tech / Tooling
Primarily prompt-driven — Sage's intelligence lives in `prompts/system_prompt.md` and the vault. Python is for helpers MCPs can't cover (future YouTube transcripts, batch repo scans). AgentMail is on-demand Bash+curl — no helper needed for v1.

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
- [x] **Mobile-app research baseline (2026-05-24):** Matt Pocock's 7-step workflow synthesized from `collateral/HOW do you engineer with AI_.eml` + `mattpocock/skills` open-source catalogue mapped. 5/7 steps directly covered by skills; Step 6 (Ralph loops / execution) is the cohort's residual value as a *pattern*, not a skill. Mobile substitutions identified for the two side projects.
- [x] **AI notebook one-shot ingestion (2026-05-24):** 21 Evernote notes synthesized to `output/20260524-ai-notebook-synthesis.md` via sub-agent; 32-item drop list at `output/20260524-ai-notebook-drop-list.md` pending Vince's review.
- [x] **Structured-writeup pattern vaulted (2026-05-24):** `vault/2026-05-24-agent-mediated-structured-writeup.md` — domain-conventions-encoding agent + multiplicative compression vs. a manual baseline. HITL calibration listed as the open question feeding the harness experiment.
- [x] **Handoff vault entry promoted to confirmed (2026-05-26):** third independent inventor (Steinberger via `steipete/agent-scripts`) triggered promotion. Section #3 (Live state / running processes) added from his version. Negative-space-insight isolated as distinctive to the Vince/Pocock branch. Committed as `13124c3`.
- [x] **Hybrid on-demand routing adopted for Sage itself (2026-05-26):** `local/` directory (gitignored) for deep content files, paired with one-line CLAUDE.local.md pointers + time-sensitive callouts. First instance: `local/skill-watchlist.md`. Eating own work-repo-pattern dog food.
- [x] **Work-repo GitHub crawl — partial review done (2026-05-26):** fine-grained PAT for the work GH org working; AI-marker scan across 42 visible repos; read CLAUDE.md, both SKILL.md files, and on-demand context for architecture + webhooks in the primary K8s-operator repo. Full review (remaining context files, source) pending.
- [x] **Work AI initiative captured (2026-05-27):** decision direction, Vince's TODOs, team commitments, open architectural questions all in CLAUDE.local.md. First meeting transcript + continuation notes in `collateral/`.
- [x] **Sage scope reframed (2026-05-28):** v1 "personal-first → rolls out to work" framing replaced with current cross-context operating practice. Companion memory captures the attribution discipline. Committed as `1c47578`.
- [~] **Work harness experiment scoped (2026-05-24):** primary deliverable is the *process writeup*. MVP candidate selected and held in gitignored memory, pending Vince's confirmation.
- [~] **Cross-project sweep in progress** — see `CLAUDE.local.md` *Project Review Queue*. Reviewed: internal infra/ops project (2026-05-23); primary K8s-operator work repo partial (2026-05-26). Priority-bumped: del-infra / Robin absorption (Robin = Vince's OpenClaw research assistant; lives in `del-infra`). Two additional work repos: PAT approved + working 2026-06-10; AI-marker scan done same day — both greenfield for AI tooling (findings in `CLAUDE.local.md` Project Review Queue). A third repo originally in scope was cut due to approval-timing risk.
- [ ] **Three pending seed patterns** still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- [x] **Both tier-2 vault candidates written:** on-demand context routing (`vault/2026-06-09-on-demand-context-routing.md`, landed with the 2026-06-09 restructure) and negative-space documentation (`vault/2026-06-10-negative-space-documentation.md`, extracted from the handoff entry's insight block). Both seed (multi-source observed).
- [ ] **Three Apple-platform skills queued** — gated on the Mum-stories app actually starting, not on a date (the June 2026 window lapsed unworked; re-scoped 2026-09-18). Per `local/skill-watchlist.md`.
- [ ] Wire up Evernote MCP (deferred)
- [x] **AgentMail wired (2026-06-02):** Sage's own dedicated inbox, separate API key in `.env.local`, on-demand Bash+curl. `list_inboxes` + `list_threads?labels=unread` both returned HTTP 200 against the live endpoint. Robin's `del-infra/docs/agentmail-integration.md` is the inherited gotchas reference (labels-plural, size-gate >50KB, separate state keys for polling vs triage, archive-over-delete).
- [x] **First plugin install + first skill-creator run (2026-06-05):** Installed Anthropic `anthropic-agent-skills` marketplace + `example-skills` plugin (12 skills incl. `skill-creator`, `mcp-builder`). Used `skill-creator` to build `setup-github-pat` skill from earned scars across two prior PAT setups. Skill global at `~/.claude/skills/setup-github-pat/` (out of repo scope). Knocks out one "try plugins" TODO; `git-commit-from-file` named as the next natural candidate.
- [x] **Pocock 7-step engineering workflow vaulted (2026-06-02):** `vault/2026-06-02-pocock-7-step-engineering-workflow.md` — lightweight alternative to spec-first / waterfall-rebranded approaches. 5/7 steps backed by skills in `mattpocock/skills`. First executed worked example (Step 1, `grill-with-docs`) recorded with positive outcome on a personal-track project (2026-06-05/06). Status: seed (single-source).
- [x] **Skill-extraction methodology vaulted (2026-06-09):** `vault/2026-06-09-skills-extracted-not-anticipated.md` — Vince's articulated principle: skills are extracted from demonstrated reuse, not anticipated reuse. Two trigger axes — cross-project (spatial) + same-project temporal. Worked examples on the cross-project axis (`handoff`, `access-openstack`); same-project axis pre-worked-example with `git-commit-from-file` named as likely first. Source for the meeting-prep doc (`output/20260610-skills-when-to-use.md`, gitignored).
- [ ] **AgentMail MCP / Claude Skill adoption decision** — measured 2026-06-02. Vendor MCP = 17 tools, ~3,000 tokens steady-state; `--tools` flag can pin a 3-tool subset (~500 tokens). Vendor Claude Skill (`agentmail-to/agentmail-claude-skill`) = ~50-token trigger metadata + ~2,500 tokens on-demand. Raw curl wins on bloat; revisit if curl friction shows up in practice.
- [ ] Auto Dream — not yet in current CC install; check after upgrade. Revisit enable decision when memory crosses ~30 entries.

## Session State

> Forward-looking operating context only. Backward-looking archaeology (what got done thread-by-thread, decisions articulated, commits) lives in `docs/session-state.md` — consult on demand.

**Last:** 2026-09-20. **Output-shape fix landed**: Vince's standing complaint (a 2-line question returning 50 lines while he multitasks) is fixed durably by the `Sage Dialog` output style — always on, no invocation, active from this session's start. `claude plugin eval` used for the first time against the superseded interim skill and **parked** (net-zero delta, one real regression; the first paid run was invalid because the control arm never answered). Local-inference track opened: M5 Ultra Mac Studio (256GB) arrives December — planning only, detail in `CLAUDE.local.md`. Atlas overview refreshed; Homecrew repo URL confirmed. Two memories banked.

**Prior:** 2026-09-18. **Housekeeping pass after a ~10-week idle gap** (no commits 2026-07-05 → 2026-09-18). Every expired date-gated item cleared: the Fable-5 bootstrap-window section in `CLAUDE.local.md` replaced by a plain priority list; the June-2026 iOS skill window re-gated to "when the Mum-stories app actually starts"; the model reconcile closed (`opus[1m]` pinned); the Fable vault entry un-stuck from RESTRICTED and its free-window material marked expired. No project work advanced — the queue below is unchanged apart from the prunes.

**Prior:** 2026-07-05. Vince reported **inception built the coding harness and it's PROVEN OUT on `tvo-skyline`** (tvo-skyline was the test bed) — inception is unstalled and producing; Vince's next focus on tvo-skyline is **optimizing the UI work**. Evaluated `apoorvjain25/frontier` (21-craft quality-loop skill, incl. a Design craft) for that UI work — **ruled out** (reasoning in archaeology). First real AgentMail triage: **Homecrew** (open-source package manager for agent skills) logged to the skill watchlist; AgentNews #15 checklist distilled to `inbox/`; both threads archived. That session ran on Fable 5 (access restored 2026-07-05). **Model state since settled:** this project pins `opus[1m]` in `.claude/settings.local.json`.

**Open items going into next session:**

*PRIORITY — FIX LANDED 2026-09-20, now in live validation:*
- **Sage communication shape.** Vince's complaint: a 2-line question returns 50 lines; he is usually multitasking and wants dialog, not essays. **Durable fix in place:** output style `Sage Dialog` (`~/.claude/output-styles/sage-dialog.md`), activated for this project only via `outputStyle` in `.claude/settings.local.json`. Always on, no invocation, survives cold start. Three rules: one thing per turn; ask instead of pre-answering the forks; **long-form goes to a file and chat carries the pointer** (on-demand routing applied to conversation). Open: confirm it holds over a few real sessions, then decide whether to promote it to `~/.claude/settings.json` portfolio-wide.
- `action-first-dialog` skill = superseded as the interim measure; it shapes monologues, not turn-taking. Eval parked at net-zero delta with one real regression (see archaeology).

*From 2026-09-20 (held for action):*
- **Local-inference track — planning only until December.** M5 Ultra Mac Studio (256GB, headless, always-on) arrives then. Positions already taken, don't relitigate: the box is Sage's ingestion/retrieval backend not a second Sage; models are chosen last; one project named for the service, not the technology. **Next and highest-value: the cross-portfolio workload inventory** — needs Vince. Full detail: `CLAUDE.local.md` → Local-inference track. Memory [[project-local-inference-track]].
- **Homecrew trust pass still open.** Repo URL now confirmed (`github.com/with-logic/crew`, site `crew.logic.inc`, by Logic Inc.) — the source audit per the MCP-hardening pattern has NOT been done, and that is what gates any team recommendation.
- Eval suite for `action-first-dialog` is parked at `~/.claude/skills/action-first-dialog/evals/` (5 cases, graders written). If resumed, rewrite the cases against Sage's real traffic — architectural advice and research synthesis — not generic coding prompts, and raise runs-per-case above 3.

*From the 2026-07-05 session (held for action):*
- **Model pin — CLOSED 2026-09-18.** Vince's call: **follow the global default**. The `model` key is deleted from `.claude/settings.local.json`; Sage now runs whatever `/model` is set to globally, with no project-level override. Don't reintroduce a pin here without an explicit ask. Memory [[project-fable5-pulled-opus48-standard]] carries the "'needs Fable' is not a valid blocker" guard — still stands.
- **Harness proven (2026-07-05): inception BUILT it, tvo-skyline TESTED it → two consequences:** (1) tvo-skyline's next phase is **UI-work optimization** — frontier was evaluated for this and ruled out; toolset stays built-in `frontend-design`/`dataviz` + the harness's own review step. (2) **Inception is unstalled** — it delivered the harness and now has a validated worked example to generalize the reusable scaffolding from (the 2026-06-26 decompose-then-extract framing played out with inception doing the building; memory [[project-inception-stalled-decompose-via-tvo-skyline]] updated).
- **Homecrew — trust pass pending** before any team recommendation (source audit per the MCP-hardening pattern; exact repo URL still to verify — newsletter links were tracking redirects). Details: `local/skill-watchlist.md` → Ecosystem tooling; pointer added under the WORKFLOW-skill-mechanics open question in `CLAUDE.local.md`. Candidate answer to that question.
- AgentNews #15 note is background only (`inbox/20260604-agentnews15-ai-employees-checklist.md`, gitignored — deliberately uncommitted); becomes actionable if the work AI initiative moves from sharing skills to deploying agents.

*Carried from earlier sessions:*
- Shared work skills repo LIVE (3 skills shipped: `handoff`, `access-openstack`, `share-skill-with-team`). Continue uploading validated candidates — `CLAUDE.local.md` → Skill upload candidates.
- Apply `grill-with-docs` to a second personal-track project (in progress on **inception**).
- Atlas overview (`.atlas/`, gitignored): refresh at session-end, deliverables + key todos only — no minutia (refreshed 2026-07-05).
- Migrate other projects to the brief+detail Session State pattern opportunistically — no forced retrofit.
- Audit follow-ups from 2026-06-09 routing restructure: Project Status checklist (this CLAUDE.md) has the same forward-vs-backward shape problem one level up; `CLAUDE.local.md` review-queue + AI-initiative sections same in the gitignored file. Canonical `docs/session-state.md` schema deferred until a second project migrates.
- MVP decision for work harness experiment.
- 32-item drop list at `output/20260524-ai-notebook-drop-list.md` (gitignored) pending checkbox review.
- Mum stories / Mind Maker app — 30-min brain-dump needed.
- Voice/transcription unifier standardization (superwhisper front-runner).
- Three seed patterns still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- Auto Dream: not yet in current CC install. Memory ~20 entries; revisit enable decision when it crosses ~30.
- Robin / del-infra absorption pending Vince's go; ansible-vault alternatives evaluation queued.
- Tier-2 vault candidate remaining: fine-grained PAT scope discipline (has the `setup-github-pat` skill as a worked-example sibling).

**Task list state:**
- Task #3 (qualitative test for `setup-github-pat`) — the live PAT validation it was waiting on landed 2026-06-10. Assess and likely close next session.
- Task #4 (optional description optimization + package) — pending.

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` AND `CLAUDE.local.md`; consult `docs/session-state.md` on demand. No model pin (follows the global default). **The `Sage Dialog` output style is active — short turns, one thing per turn, long-form to files.** Watch whether it holds; that is the live validation.
2. **tvo-skyline — optimize the UI work.** Harness proven there 2026-07-05; this is the new top thread.
3. **Inception — generalize from the win:** it built the harness that tvo-skyline validated; next is turning that into the reusable scaffolding deliverable (`grill-with-docs` in progress there).
4. Homecrew trust pass (`local/skill-watchlist.md` → Ecosystem tooling), then decide whether to surface it to the team.
5. Shared work skills repo — continue uploads (`CLAUDE.local.md` → Skill upload candidates).

**Session continuity:** Vince closes sessions and never re-enters. Next session is a cold start — re-read `CLAUDE.md` AND `CLAUDE.local.md` before doing anything (per Vince's global rule).

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
