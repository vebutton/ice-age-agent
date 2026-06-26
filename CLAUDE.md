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
- [ ] **iOS skill review window: June 2026** — three Apple-platform skills queued (per `local/skill-watchlist.md`).
- [ ] Wire up Evernote MCP (deferred)
- [x] **AgentMail wired (2026-06-02):** Sage's own dedicated inbox, separate API key in `.env.local`, on-demand Bash+curl. `list_inboxes` + `list_threads?labels=unread` both returned HTTP 200 against the live endpoint. Robin's `del-infra/docs/agentmail-integration.md` is the inherited gotchas reference (labels-plural, size-gate >50KB, separate state keys for polling vs triage, archive-over-delete).
- [x] **First plugin install + first skill-creator run (2026-06-05):** Installed Anthropic `anthropic-agent-skills` marketplace + `example-skills` plugin (12 skills incl. `skill-creator`, `mcp-builder`). Used `skill-creator` to build `setup-github-pat` skill from earned scars across two prior PAT setups. Skill global at `~/.claude/skills/setup-github-pat/` (out of repo scope). Knocks out one "try plugins" TODO; `git-commit-from-file` named as the next natural candidate.
- [x] **Pocock 7-step engineering workflow vaulted (2026-06-02):** `vault/2026-06-02-pocock-7-step-engineering-workflow.md` — lightweight alternative to spec-first / waterfall-rebranded approaches. 5/7 steps backed by skills in `mattpocock/skills`. First executed worked example (Step 1, `grill-with-docs`) recorded with positive outcome on a personal-track project (2026-06-05/06). Status: seed (single-source).
- [x] **Skill-extraction methodology vaulted (2026-06-09):** `vault/2026-06-09-skills-extracted-not-anticipated.md` — Vince's articulated principle: skills are extracted from demonstrated reuse, not anticipated reuse. Two trigger axes — cross-project (spatial) + same-project temporal. Worked examples on the cross-project axis (`handoff`, `access-openstack`); same-project axis pre-worked-example with `git-commit-from-file` named as likely first. Source for the meeting-prep doc (`output/20260610-skills-when-to-use.md`, gitignored).
- [ ] **AgentMail MCP / Claude Skill adoption decision** — measured 2026-06-02. Vendor MCP = 17 tools, ~3,000 tokens steady-state; `--tools` flag can pin a 3-tool subset (~500 tokens). Vendor Claude Skill (`agentmail-to/agentmail-claude-skill`) = ~50-token trigger metadata + ~2,500 tokens on-demand. Raw curl wins on bloat; revisit if curl friction shows up in practice.
- [ ] Auto Dream — not yet in current CC install; check after upgrade. Revisit enable decision when memory crosses ~30 entries.

## Session State

> Forward-looking operating context only. Backward-looking archaeology (what got done thread-by-thread, decisions articulated, commits) lives in `docs/session-state.md` — consult on demand.

**Last:** 2026-06-26. Re-investigated a work repo's coding-subagent + MEMORY.md mechanism (feature-branch prototype: subagent via Task tool with delegate-logic in its description; MEMORY.md loaded prompt/frontmatter-driven via on-demand index routing; companion runtime skill; Karpathy-guidelines guardrail skill; simplifier-gate hooks) and researched the Karpathy skills (community `andrej-karpathy-skills` — one skill, four principles). Distilled both into a new vault entry — **the in-repo coding harness pattern** — plus a shareable scaffolding spec (`output/`) for the Inception project to productize into reusable bootstrap scaffolding. Confirmed the **shared work skills repo is now LIVE** (3 skills shipped). Created the project's **Atlas overview** (`.atlas/`, gitignored; `~/.atlas-host` = `mac-m4-pro`). Three commits pushed. **Fable 5 was pulled at US-Govt direction — Vince reverted all projects to Opus 4.8 (1M); Sage now runs on Opus 4.8.**

**Open items going into next session:**

*From this session (held for action):*
- **Fable 5 RESTRICTED (US-Govt direction) — standardized on Opus 4.8 (1M) across all projects.** Restricted-not-retired: temporary, pending Anthropic securing it better. Vince removed the `claude-fable-5` pin from `.claude/settings.local.json` this session; Sage runs on Opus 4.8. Capability-profile vault entry + INDEX already marked RESTRICTED/historical (not obsolete). Bootstrap-window project priorities in `CLAUDE.local.md` still stand; only the model changed. **Guard:** "needs Fable" is not a valid blocker — Opus 4.8 (1M) is top-tier; a stall is scope/approach, not model.
- **Shared work skills repo — now LIVE; Phase 1 realized.** `handoff`, `access-openstack`, and `share-skill-with-team` (the meta-skill for creating team skills) are shipped to it. Repo name + remaining upload candidates kept in `CLAUDE.local.md` → Skill upload candidates. This closes the standing 5/27 TODO; the unblock came from the 2026-06-10 call (engineers trained, CTO repeatability case landed).
- **In-repo coding harness pattern captured + Inception spec drafted.** Vault entry `vault/2026-06-17-in-repo-coding-harness.md` (seed, single-source) + a shareable scaffolding spec at `output/20260617-coding-harness-scaffolding-spec.md` (gitignored) for the Inception project to build reusable, bootstrappable scaffolding from. This is the productization path for the top-priority coding-harness build.
- **Atlas overview now exists** (`.atlas/overview.md`, gitignored; `.atlas/` added to `.gitignore`; `~/.atlas-host` = `mac-m4-pro`). Refresh at session-end with deliverables + key todos only — no minutia (Vince's instruction).
- **Inception (scaffolding-builder) was STALLED — plan now set (Vince accepted 2026-06-26).** Vince felt it was "too advanced and needed Fable." Accepted approach: don't design the abstract generator top-down — build the concrete **tvo-skyline** harness BY HAND first (using the scaffolding spec as a checklist), then have inception *extract* the reusable scaffolding from that one worked example (extracted-not-anticipated applied to the project itself). Vince will share the harness framework with inception positioned exactly that way. This reorders the dependency so inception follows tvo-skyline instead of blocking it. **tvo-skyline harness build = confirmed top priority.** (Memory: [[project-inception-stalled-decompose-via-tvo-skyline]].)
- Apply `grill-with-docs` to a second personal-track project (Vince is trying it on **inception** — see stalled-inception item above).
- Migrate other projects to the brief+detail Session State pattern opportunistically — no forced retrofit; each project does it at its next session-end housekeeping.
- Audit follow-ups from 2026-06-09 routing restructure: Project Status checklist (this CLAUDE.md) has the same forward-vs-backward shape problem one level up; `CLAUDE.local.md` review-queue + AI-initiative sections same in the gitignored file. Canonical schema for `docs/session-state.md` across projects deferred until a second project migrates.

*Carried from earlier sessions:*
- MVP decision for work harness experiment.
- 32-item drop list at `output/20260524-ai-notebook-drop-list.md` (gitignored) pending checkbox review.
- Mum stories / Mind Maker app — 30-min brain-dump needed.
- Voice/transcription unifier standardization (superwhisper front-runner).
- Three seed patterns still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- Auto Dream: not yet in current CC install. Memory now ~20 entries; revisit enable decision when it crosses ~30.
- Robin / del-infra absorption pending Vince's go; ansible-vault alternatives evaluation queued.
- Tier-2 vault candidate remaining: fine-grained PAT scope discipline (now has the `setup-github-pat` skill as a worked-example sibling). Negative-space documentation written 2026-06-10.

**Task list state:**
- Task #3 (qualitative test for `setup-github-pat`) — the live PAT validation it was waiting on landed 2026-06-10 (approval came through; authenticated reads worked first try). Assess and likely close next session.
- Task #4 (optional description optimization + package) — pending.

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` AND `CLAUDE.local.md`. Consult `docs/session-state.md` for thread-level archaeology on demand. **Sage runs on Opus 4.8 (1M)**; Fable 5 restricted (US-Govt direction, temporary); the `claude-fable-5` pin was removed from `.claude/settings.local.json` this session.
2. **TOP PRIORITY — build the `tvo-skyline` harness BY HAND**, using the scaffolding spec (`output/20260617-coding-harness-scaffolding-spec.md`) as a checklist. This is the concrete worked example that inception will extract its reusable scaffolding from.
3. **Inception** — Vince is sharing the harness framework with it, positioned as "extract from the tvo-skyline harness, don't build the generator top-down." Check progress / unblock as needed; `grill-with-docs` is being applied here.
4. Shared work skills repo — LIVE with 3 skills shipped (`handoff`, `access-openstack`, `share-skill-with-team`). Next: continue uploading remaining validated candidates — see `CLAUDE.local.md` → Skill upload candidates.

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
