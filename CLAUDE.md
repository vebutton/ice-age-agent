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
- [x] **Mobile-app research baseline (2026-05-24):** Matt Pocock's 7-step workflow synthesized from `collateral/HOW do you engineer with AI_.eml` + `mattpocock/skills` open-source catalogue mapped. 5/7 steps directly covered by skills; Step 6 (Ralph loops / execution) is the cohort's residual value as a *pattern*, not a skill. Mobile substitutions identified for the two side projects.
- [x] **AI notebook one-shot ingestion (2026-05-24):** 21 Evernote notes synthesized to `output/20260524-ai-notebook-synthesis.md` via sub-agent; 32-item drop list at `output/20260524-ai-notebook-drop-list.md` pending Vince's review.
- [x] **Structured-writeup pattern vaulted (2026-05-24):** `vault/2026-05-24-agent-mediated-structured-writeup.md` — domain-conventions-encoding agent + multiplicative compression vs. a manual baseline. HITL calibration listed as the open question feeding the harness experiment.
- [x] **Handoff vault entry promoted to confirmed (2026-05-26):** third independent inventor (Steinberger via `steipete/agent-scripts`) triggered promotion. Section #3 (Live state / running processes) added from his version. Negative-space-insight isolated as distinctive to the Vince/Pocock branch. Committed as `13124c3`.
- [x] **Hybrid on-demand routing adopted for Sage itself (2026-05-26):** `local/` directory (gitignored) for deep content files, paired with one-line CLAUDE.local.md pointers + time-sensitive callouts. First instance: `local/skill-watchlist.md`. Eating own work-repo-pattern dog food.
- [x] **Work-repo GitHub crawl — partial review done (2026-05-26):** fine-grained PAT for the work GH org working; AI-marker scan across 42 visible repos; read CLAUDE.md, both SKILL.md files, and on-demand context for architecture + webhooks in the primary K8s-operator repo. Full review (remaining context files, source) pending.
- [x] **Work AI initiative captured (2026-05-27):** decision direction, Vince's TODOs, team commitments, open architectural questions all in CLAUDE.local.md. First meeting transcript + continuation notes in `collateral/`.
- [x] **Sage scope reframed (2026-05-28):** v1 "personal-first → rolls out to work" framing replaced with current cross-context operating practice. Companion memory captures the attribution discipline. Committed as `1c47578`.
- [~] **Work harness experiment scoped (2026-05-24):** primary deliverable is the *process writeup*. MVP candidate selected and held in gitignored memory, pending Vince's confirmation.
- [~] **Cross-project sweep in progress** — see `CLAUDE.local.md` *Project Review Queue*. Reviewed: internal infra/ops project (2026-05-23); primary K8s-operator work repo partial (2026-05-26). Priority-bumped: del-infra / Robin absorption (Robin = Vince's OpenClaw research assistant; lives in `del-infra`). Two other work repos still uncertain — confirm access with admin.
- [ ] **Three pending seed patterns** still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- [ ] **Two tier-2 vault candidates ready for write:** on-demand context routing, negative-space documentation (both have worked examples now).
- [ ] **iOS skill review window: June 2026** — three Apple-platform skills queued (per `local/skill-watchlist.md`).
- [ ] Wire up Evernote MCP (deferred)
- [ ] AgentMail Python helper (deferred)
- [ ] Auto Dream — not yet in current CC install; check after upgrade. Revisit enable decision when memory crosses ~30 entries.

## Session State
**Last session:** 2026-05-26 through 2026-05-28 (multi-day; substantial). Five threads — work-repo GitHub crawl, handoff vault entry promotion, hybrid on-demand routing adopted for Sage itself, work AI initiative captured, Robin cross-agent exchange + Sage scope reframe. **Four new memories banked**, plus a fifth (`project_sage_low_profile_at_work`) replacing an outdated one.

**What got done — by thread:**

**Thread 1: Work-repo GitHub crawl (2026-05-26).**
- Fine-grained PAT for the work GH org approved + working. AI-marker scan across 42 visible repos returned exactly 1 CLAUDE.md (in the primary K8s-operator repo), 0 other markers (no AGENTS.md, no .cursorrules, no .mcp.json, no copilot-instructions.md).
- Read that repo's CLAUDE.md (~3.5KB, lean), both SKILL.md files (a Go/operator-development skill, a PR-workflow skill), and two on-demand context files (architecture overview, webhook architecture). Pending for full review: remaining context files (CRDs, flows), skill reference docs, repo source.
- **Discovery:** the PR-workflow skill in that repo = the canonical PR shape the team is converging on per the 2026-05-27 internal AI meeting recap.
- Two tier-2 vault candidates surfaced (not yet written): **on-demand context routing** (worked example: lean CLAUDE.md routing table + matching "When to load" headers in each context file) and **negative-space documentation** (worked example: the webhook context file — dense, irreplaceable).
- Two other work repos still not visible with current PAT — different org or different names; verify with admin.

**Thread 2: Handoff vault entry promoted to confirmed (committed as `13124c3`).**
- Steinberger's `/handoff` slash-command in `steipete/agent-scripts` is the third independent inventor (Vince informally, Matt Pocock named/structured, now Steinberger) — triggered promotion from seed to confirmed.
- Added section #3 "Live state / running processes" borrowed from Steinberger's checklist (tmux attach commands, dev servers, debuggers, background scripts).
- **Negative-space-insight isolation:** Steinberger's checklist does NOT have "What's been ruled out" as headline-value — only 2 of 3 inventors share that insight. Strengthens the case for promoting negative-space-documentation to its own standalone vault entry.
- `vault/INDEX.md` updated to match.

**Thread 3: Hybrid on-demand routing adopted for Sage itself.**
- Created `local/` directory (gitignored). Per-topic deep content files paired with one-line CLAUDE.local.md pointers + time-sensitive callouts. Eating our own on-demand-routing dog food from the work-repo crawl.
- First instance: `local/skill-watchlist.md` for third-party skill watchlist (steipete/agent-scripts + Robin's recommendations + clawhub.ai sources). Pointed at from CLAUDE.local.md's new "Back-burner watchlists" section with iOS June 2026 priority callout in the always-loaded reminder line.
- Pattern generalizes — next watchlist topic (newsletters, talks, events) gets the same shape: add `local/<topic>-watchlist.md` + one more line to CLAUDE.local.md.

**Thread 4: Work AI initiative captured (all in gitignored files).**
- First meeting transcript already in `collateral/` (received same day). Second meeting (post-Zoom-boot continuation) notes captured in `collateral/`.
- Full work-AI-initiative section added to CLAUDE.local.md: shared-repo decision direction (Phase 1: work GH org repo organized by team folder; Phase 2: enterprise plugin later), Vince's TODOs (create repo, upload handoff skill, continue skill search, try plugins, try hooks), team-member commitments, open architectural questions (skill-upload mechanics, two hook types, plugin vs skill distinction).
- **Discovery:** the "skill-finding tool" demoed at the first meeting was Sage (gist conveyed in recap, name withheld) — confirms the low-profile attribution rule is operating naturally.
- Cross-reference: a deterministic-hooks approach in the .eml = the "feature hooks (no tokens)" type from Vince's continuation notes (same thing, two vantage points).

**Thread 5: Robin cross-agent exchange + Sage scope reframe.**
- Robin (Vince's OpenClaw research assistant) sent 4 skill suggestions. Evaluated: `skill-creator` (best fit — would accelerate the "vault → shared work repo" pipeline), `taskflow` (worth a look if it complements Session State pattern), GitHub Spec-Kit templates (with the "spec-first = waterfall rebranded" caveat from the structured-writeup vault entry), `coding-agent` (skip — wrong fit for Sage's knowledge-holder role). All three viable plus clawhub.ai source added to `local/skill-watchlist.md`.
- del-infra/Robin priority-bumped in CLAUDE.local.md Project Review Queue. Robin's accumulated knowledge is the one-time-absorption target named in CLAUDE.md Inputs since v1 bootstrap.
- **Sage scope reframe:** Vince corrected the v1 "personal-first" framing. Memory file renamed with updated framing capturing current cross-context operating practice + low-profile attribution discipline. CLAUDE.md scope line updated to match (committed as `1c47578`).

**Decisions Vince articulated this session (durable):**
- **Hybrid routing pattern for Sage itself:** CLAUDE.local.md = lean reminders + routing pointers; `local/*.md` = deep content loaded on demand. Mirrors the work-repo pattern.
- **Sage scope is current cross-context operating practice**, not personal-first-experimental. Low-profile attribution at work — outputs ship under Vince's name; gist visibility OK in casual peer conversations; no formal pitch / no evangelism.
- **Commit-message discipline:** substantive bodies expected (the `13124c3` handoff body is the gold standard); personal-life specifics stay OUT regardless of file changed. Personal-repo amplifier rule: stricter for personal repos, not looser. Even when specifics appear in the diff file itself, do not amplify in commit-message body.
- **Skill watchlist priorities:** iOS app skills priority window is June 2026. `skill-creator` from Robin's batch is the strongest near-term fit because it accelerates the work shared-repo upload pipeline.

**New memories banked this session:**
- `feedback-plain-language-for-new-material` — lead with plain words + concrete examples when introducing material Vince hasn't worked on yet.
- `feedback-partial-coverage-is-fine-in-research` — ~60–70% coverage with a surfaced "still TBD" bucket beats burning tool calls to chase 100%.
- `project-sage-low-profile-at-work` — replaces `project-sage-stays-personal-first`; captures current cross-context practice + low-profile attribution discipline.
- `feedback-personal-life-specifics-out-of-commit-messages` — personal-life specifics stay out of commits regardless of file; substantive project-technical bodies are expected.

**Commits pushed this session:**
- `13124c3` — Promote handoff vault entry to confirmed; gitignore `local/` for routed context.
- `1c47578` — CLAUDE.md "This Project" scope description refreshed.

**Open items going into next session:**

*Work AI initiative (tracked in CLAUDE.local.md):*
- Vince's TODOs: create shared work GH repo, upload handoff skill as first validated entry, try plugins, try hooks.
- Wait for team deliverables tracked in CLAUDE.local.md (enterprise-skill-packaging research, repo-binding admin steps).

*Skill watchlist (`local/skill-watchlist.md`):*
- iOS app skill review window: **June 2026**.
- Strongest near-term candidates from Robin batch: `skill-creator` (strongest), `taskflow`, GitHub Spec-Kit templates.
- Verify clawhub.ai CC vs OpenClaw coverage.
- 9 unclassified Steinberger skills available for deeper pass on request.

*Work-repo crawl:*
- Full review pending: remaining context files (CRDs, flows), skill reference docs, repo source.
- Two other work repos still uncertain — verify access with admin.
- Tier-2 vault candidates queued for write: on-demand context routing, negative-space documentation (both have worked examples now).

*Robin / del-infra (priority-bumped):*
- Vince to point Sage at Robin's data when ready for the one-time absorption.
- del-infra original review goals: ansible-vault alternatives evaluation.

*Auto Dream:*
- Not yet in current CC install (confirmed via `/memory`). Check after next CC upgrade.
- Sage memory currently ~18 entries; revisit Auto Dream enable decision when it crosses ~30.

*From earlier sessions (carried):*
- MVP decision for work harness experiment.
- 32-item drop list at `output/20260524-ai-notebook-drop-list.md` pending checkbox review.
- Mum stories / Mind Maker app — 30-min brain-dump needed.
- Voice/transcription unifier standardization (superwhisper front-runner).
- Three seed patterns still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.

**Inbox saved this session:**
- `inbox/20260527-arps18-claude-code-mastery.md` — Arpan Patel's "Beyond the Prompt: Claude Code" guide (community Slack share). Substantial overlap with active Sage threads (CLAUDE.md style, skills, plugins, hooks, MCPs). Closer reading still pending.

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` **AND** `CLAUDE.local.md` (both have substantive updates).
2. Check Vince's progress on: (a) shared work GH repo creation, (b) plugins + hooks experimentation, (c) any team-deliverable arrivals.
3. If work-repo crawl access still working, consider continuing the review.
4. If del-infra/Robin absorption is the next priority, Vince to point Sage at her data.
5. Tier-2 vault candidates (on-demand context routing, negative-space documentation) are ready for write if Vince wants to clear that backlog.

**Session continuity:** Vince is closing this session. **Next session is a cold start — re-read CLAUDE.md AND CLAUDE.local.md before doing anything** (per Vince's global rule).

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
