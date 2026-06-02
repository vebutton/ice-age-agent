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
- [~] **Cross-project sweep in progress** — see `CLAUDE.local.md` *Project Review Queue*. Reviewed: internal infra/ops project (2026-05-23); primary K8s-operator work repo partial (2026-05-26). Priority-bumped: del-infra / Robin absorption (Robin = Vince's OpenClaw research assistant; lives in `del-infra`). Two other work repos still uncertain — confirm access with admin.
- [ ] **Three pending seed patterns** still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- [ ] **Two tier-2 vault candidates ready for write:** on-demand context routing, negative-space documentation (both have worked examples now).
- [ ] **iOS skill review window: June 2026** — three Apple-platform skills queued (per `local/skill-watchlist.md`).
- [ ] Wire up Evernote MCP (deferred)
- [x] **AgentMail wired (2026-06-02):** Sage's own dedicated inbox, separate API key in `.env.local`, on-demand Bash+curl. `list_inboxes` + `list_threads?labels=unread` both returned HTTP 200 against the live endpoint. Robin's `del-infra/docs/agentmail-integration.md` is the inherited gotchas reference (labels-plural, size-gate >50KB, separate state keys for polling vs triage, archive-over-delete).
- [ ] **AgentMail MCP / Claude Skill adoption decision** — measured 2026-06-02. Vendor MCP = 17 tools, ~3,000 tokens steady-state; `--tools` flag can pin a 3-tool subset (~500 tokens). Vendor Claude Skill (`agentmail-to/agentmail-claude-skill`) = ~50-token trigger metadata + ~2,500 tokens on-demand. Raw curl wins on bloat; revisit if curl friction shows up in practice.
- [ ] Auto Dream — not yet in current CC install; check after upgrade. Revisit enable decision when memory crosses ~30 entries.

## Session State
**Last session:** 2026-06-02 (single-day; AgentMail wireup + MCP-bloat measurement + first real inbox roundtrip). Five threads. **No new memories banked** — existing memories sufficient and all applied (build-vs-buy stance, vault-vs-memory distinction, pattern-domain-specificity, partial-coverage-OK, plain-language-first).

**What got done — by thread:**

**Thread 1: AgentMail wired end-to-end (committed as `c7a7c19`).**
- Sage's own dedicated AgentMail inbox stood up (literal address in `CLAUDE.local.md`), separate API key from Robin's stored in `.env.local`. Template entry added to `.env.local.example`.
- Retrieval pattern: on-demand Bash+curl only. No auto-poll, no cron. The Python helper v1 had earmarked is unnecessary for this access pattern — Tech/Tooling line updated accordingly.
- Robin's `del-infra/docs/agentmail-integration.md` is the inherited gotchas reference (labels=PLURAL, size-gate >50KB, separate polling vs triage state keys, archive-over-delete) — referenced rather than copied to avoid drift.
- Live confirmation across the canonical flow: `list_inboxes` (200), `list_threads?labels=unread` (200, with plural-labels gotcha respected on first call), `get_thread` (200), PATCH archive with snake_case `{"remove_labels":["unread"],"add_labels":["archived"]}` (200). Robin's gotchas held on the first try.
- CLAUDE.md updated in three places (Key Integrations, Tech/Tooling, Project Status); `CLAUDE.local.md` got a new "AgentMail (Sage's own inbox)" section above the Slack section.

**Thread 2: AgentMail MCP / Skill bloat measured.**
- Vendor MCP (`agentmail-to/agentmail-mcp`) thin-wraps `agentmail-toolkit` and exposes **17 tools**. Estimated ~3,000 tokens steady-state when fully loaded (Zod schemas serialized as JSONSchema in the wire format). `--tools` flag can pin a 3-tool subset (`list_threads`, `get_thread`, `update_message`) → ~500 tokens steady-state.
- Vendor Claude Skill (`agentmail-to/agentmail-claude-skill`) ≈ ~50 tokens steady-state (trigger metadata only) + ~2,500 tokens on demand when invoked. SKILL.md packaged for `.cursor/skills/`; format is identical to `.claude/skills/`.
- Raw curl baseline = 0 steady-state. **Won for Sage's on-demand pattern.** MCP/Skill adoption deferred until curl friction shows up in real practice — logged as an open `[ ]` in Project Status.

**Thread 3: Argosbrain captured in MCP watchlist (Robin-sourced).**
- New `local/mcp-watchlist.md` companion to `local/skill-watchlist.md` (same hybrid on-demand routing pattern — pointer in `CLAUDE.local.md` Back-burner watchlists).
- Argosbrain summary: local Rust daemon parsing repos into queryable graphs (tree-sitter/SCIP/LSP, 28 languages, 51 MCP tools). Vendor claims ~73% reduction in "re-read tax" input tokens on large repos via sub-millisecond graph queries. Pricing: free 1-project / $19/mo / enterprise.
- Single-source, vendor-claimed numbers — pending real-codebase validation before any vault entry. Likely-real-fit candidates: deep-call-graph repos (TVK, TSR, del-infra). Likely-poor-fit: prose-heavy projects like Sage itself.

**Thread 4: First inbox synthesis output (AgentNews #14, fetched and archived).**
- The forwarded newsletter Vince sent to test the wireup carried the **MCP 101 link** he flagged: `aibuilderclub.com/blog/mcp-101-build-mcp-servers` — directly relevant to his in-flight Salesforce-MCP build (Thread 5).
- Four other cross-thread items surfaced:
  1. **AgentMail just shipped IMAP** (this week per the newsletter). Settings: `imap.agentmail.to:993`, SSL, password = API key. Adds a *fourth* retrieval option (curl / MCP / Skill / IMAP) — human-side mail-client monitoring becomes possible without going through the API.
  2. **LangGraph 1.2 — durable runtimes.** Hot take: durability solves *internal* agent state; *external* state ("the internet has no checkpointer") stays unsolved. Single-source observation; worth tracking for any long-running agent design, not vault material yet.
  3. **OpenClaw Pancake** (startup spotlight) — AI Co-founder in Slack+Browser, *Powered by OpenClaw*. Direct connection to Robin / `del-infra` / `clawhub.ai` ecosystem.
  4. **LangChain vs CrewAI vs Raw API** (AI Builder Club) — not-a-winner decision tree. Raw SDK for single-loop, CrewAI for role-based crews, LangChain for tool/RAG ecosystem. Reasonable framing if/when Sage advises on a framework choice.
- The 50KB triage-threshold was breached *deliberately* on this fetch — curated forward, not heartbeat traffic, and Vince explicitly asked. Rule's reason (don't pay HTML-newsletter token cost on every hourly poll) doesn't apply to one-off on-demand reads.

**Thread 5: Salesforce MCP for Cowork — observation parked.**
- Vince is building a local Salesforce MCP because the official one is read-only. **Cowork** = Claude Desktop feature (post-training-cutoff for me; Vince clarified). MCP lives outside Sage's blast radius — different consumer.
- Build-vs-buy stance ([[project-mcp-build-vs-buy-stance]]) already has slack for this edge case: when the buy-path is missing the write surface you need, the stance auto-routes to build. Single instance; vault refinement deferred until a second data point per [[feedback-pattern-domain-specificity]].

**Decisions Vince articulated this session (durable):**
- **AgentMail = on-demand curl baseline.** No auto-poll, no cron. Address is Sage's own (not shared with Robin), separate API key. Python helper deferred indefinitely; not needed for this access pattern.
- **MCP/Skill adoption for AgentMail: deferred.** Raw curl wins on bloat; revisit only if curl friction is real in practice.
- **Robin's AgentMail gotchas → vault entry deferred** until Sage's own usage gives a second data point (tier-1 → tier-2 discipline).
- **Hybrid on-demand routing pattern generalized** — `local/mcp-watchlist.md` proved the watchlist pattern from skill-watchlist transfers cleanly. Next watchlist topic gets the same shape (pointer in `CLAUDE.local.md`, deep content in `local/*-watchlist.md`).
- **Personal-repo GH access path established:** `GITHUB_TOKEN= gh ...` prefix lets the keyring token (`repo` scope) override the active work-PAT for personal-repo reads. No new PAT needed.

**New memories banked this session:** none.

**Commits pushed this session:**
- `c7a7c19` — Wire AgentMail for Sage; defer MCP/Skill adoption.

**Open items going into next session:**

*From this session (held for write/decision):*
- **AgentMail MCP / Skill adoption decision** — revisit when curl friction shows up, or proactively after a few real usages give a friction baseline.
- **Argosbrain validation** — try against a real codebase (TVK or del-infra most likely) before any vault entry.
- **Robin's AgentMail gotchas vault entry** — write once Sage hits any of the four landmines naturally.

*Skill watchlist (`local/skill-watchlist.md`):*
- iOS app skill review window: **June 2026 — this month**. Three queued: `instruments-profiling`, `native-app-performance`, `hopper-debugger`.
- `skill-creator` (strongest near-term skill candidate), `taskflow`, GitHub Spec-Kit templates from Robin's batch.
- 9 unclassified Steinberger skills available for deeper pass.
- Verify clawhub.ai CC vs OpenClaw coverage.

*MCP watchlist (`local/mcp-watchlist.md`):*
- `argosbrain` pending validation pass.

*Work AI initiative (tracked in `CLAUDE.local.md`):*
- Vince's TODOs: create shared work GH repo, upload handoff skill as first validated entry, try plugins, try hooks.
- Wait for team deliverables tracked in `CLAUDE.local.md`.

*Work-repo crawl (carried):*
- Full review pending: remaining context files, skill reference docs, repo source.
- Two other work repos still uncertain — verify access with admin.

*Robin / del-infra (priority-bumped, carried):*
- Vince to point Sage at Robin's data when ready for the one-time absorption.
- ansible-vault alternatives evaluation still on the original goals list.

*Tier-2 vault candidates ready for write (carried):*
- On-demand context routing (TVK = worked example, TSR = monolithic-counter-example).
- Negative-space documentation (TVK `webhooks.md` = worked example).

*From earlier sessions (carried, unchanged):*
- MVP decision for work harness experiment.
- 32-item drop list at `output/20260524-ai-notebook-drop-list.md` pending checkbox review.
- Mum stories / Mind Maker app — 30-min brain-dump needed.
- Voice/transcription unifier standardization (superwhisper front-runner).
- Three seed patterns still uncaptured: skills-complement-MCPs, master-agents-self-architect, Context Seven.
- Auto Dream: not yet in current CC install. Sage memory ~18 entries; revisit enable decision when it crosses ~30.

**Inbox saved this session:** none new — Sage's *email* inbox got its first real message (AgentNews #14) and processed it inline; no `inbox/` folder saves needed.

**Next session — pick up here:**
1. Cold start — re-read `CLAUDE.md` **AND** `CLAUDE.local.md` (both touched this session).
2. AgentMail is wired and warm. Sage can fetch on-demand via `source .env.local && curl -H "Authorization: Bearer $AGENTMAIL_API_KEY" ...`. The `?labels=unread` plural gotcha and snake_case PATCH fields are the two patterns to remember (or consult Robin's `del-infra/docs/agentmail-integration.md`).
3. For personal-repo GH reads, use the `GITHUB_TOKEN= gh ...` prefix so the keyring token (not the work-scoped PAT) is in play.
4. If next session is **del-infra / Robin absorption**: `docs/agentmail-integration.md` already read; rest of `docs/`, source, and Robin's live data still pending.
5. If next session involves **iOS skills**, the priority window is open now (June 2026) — start with the three queued.
6. Tier-2 vault candidates (on-demand context routing, negative-space documentation) remain write-ready if Vince wants to clear that backlog.

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
