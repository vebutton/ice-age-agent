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
- [x] **Playwright MCP wired + trust pass done (2026-09-24):** `@playwright/mcp@0.0.82` pinned in `.mcp.json`, Microsoft-published, Apache-2.0, **no credential required**, no install hooks, integrity hash recorded. Configured `--browser firefox --isolated --headless` — **never Chrome**: Playwright MCP defaults to the installed Chrome channel and Vince's rule is categorical that Chrome is work-only ([[feedback-never-use-vinces-chrome]]). Adopted because `reddit.com` is blocked at the WebFetch/WebSearch layer and a direct curl returns 403 anti-bot; a real browser passes. Verified live against /r/MacStudio (200, 50 posts). Audit: `local/playwright-mcp-audit-20260924.md`. **Loads at next session start.** Residual risk to respect: page content is untrusted input — prompt-injection surface.
- [ ] **AgentMail MCP / Claude Skill adoption decision** — measured 2026-06-02. Vendor MCP = 17 tools, ~3,000 tokens steady-state; `--tools` flag can pin a 3-tool subset (~500 tokens). Vendor Claude Skill (`agentmail-to/agentmail-claude-skill`) = ~50-token trigger metadata + ~2,500 tokens on-demand. Raw curl wins on bloat; revisit if curl friction shows up in practice.
- [ ] Auto Dream — not yet in current CC install; check after upgrade. Revisit enable decision when memory crosses ~30 entries.

## Session State

> Forward-looking operating context only. Backward-looking archaeology (what got done thread-by-thread, decisions articulated, commits) lives in `docs/session-state.md` — consult on demand.

**Last:** 2026-10-13 → 10-16 (one session over four days). **Sitting 3 DONE: 24/24 with scaffolding.** StreamFilter failed at 32K (4/6) and passed at 48K (6/6, 0 compactions): context, not reasoning. The server failed 0/7 unscaffolded and passed **7/7 with a frontier-written skeleton** (the model only wrote `handle_proxy()`). **Vaulted as `vault/2026-10-16-frontier-scaffolds-local-fills.md`.** Coding-agent handoff for inception written. Also: reddit research (Vince's drops + Sage's sweep + the M5U oMLX numbers), REA logged (opportunistic), subscription-terms change found (test on M5U), Sage's reddit account shadowbanned (appeal in), email triage. Detail: `docs/session-state.md`; run log `local/m4-stack-week-20261006.md` → *Sitting 3, retry*.

**Prior:** 2026-10-12 evening. **M4 trial sitting 3 run 1 FAILED: metrics proxy, 9/24 after 1h19** (qwen3.5:9b, thinking on, 32K, 5 compactions). Claude drafted 24 acceptance tests from real Ollama captures (reference impl 24/24). The model ran pytest with `-x`, so "1 failed, 8 passed" looked like 23/24 when it had stopped at test 9. Never stripped the SSE `data: ` prefix; wrote code via bash heredocs (truncation, permission prompts, a junk `python3 *` always-allow). **Read: a one-function task fits a 9B; a four-part streaming proxy in one shot does not.** Also found: Ollama's OpenAI endpoint DOES give the prefill/gen split (`timings`) when `include_usage` is set. `disabled_providers` landed but does not stop the unreadable-config fallback. Detail: `local/m4-stack-week-20261006.md` → *Sitting 3, run 1*.

**Prior:** 2026-10-12 (short refresher). Sitting 2 rerun on gemma4:12b: 14/14 in ~26 min, two "continue" nudges. Unreadable mounted config → silent fallback to cloud Big Pickle; fix `:ro,Z` + check the footer.

**Prior:** 2026-10-11. **M4 trial sitting 2 DONE — first ATDD run on a local model.** Claude drafted 14 acceptance tests from real fixtures (separate context; Vince reviewed); qwen3.5:9b in OpenCode implemented. **Thinking on: 14/14 in ~17–19 min. Thinking off: failed** (planted `ms`-as-minutes trap, invented a test case, 14 rewrites) — keep thinking on for coding. **Boundary lesson:** an OpenCode edit-deny on the tests was routed around instantly via `echo >` in bash; fixed with **read-only mounts** of tests + fixtures (verified). Permissions now pre-approved in the managed config. Also: model revision log (5 SHAs), OptiQ = oMLX-only, OpenCode prompt anatomy (~half is tool schemas), metrics proxy = its own launchd service. Learning handoff: `local/atdd-handoff-20261011.md`. Detail: `local/m4-stack-week-20261006.md` → *Sitting 2*.

**Prior:** 2026-10-09 → 10-10. **M4 trial sitting 1 DONE: tool calling works** in a rootless Podman sandbox (workspace-only mount) against native Ollama. GPU monitor decision: macmon + a metrics proxy. Detail: `local/m4-stack-week-20261006.md`.

**Open items going into next session:**

*FROM THE 2026-10-11 SESSION (M4 trial, sitting 2 done):*
- **M4 trial: sitting 3 DONE: 24/24 with scaffolding (2026-10-16).** prepare 8/8, log 3/3, StreamFilter 6/6 (48K only; 4/6 at 32K). Server: **0/7 unscaffolded** (~168 min, wrong stdlib wiring + silent hangs) → **7/7 with a frontier-written skeleton** (the model wrote only `handle_proxy()`, stayed in scope; 0→3 came before any permission change). **Split confirmed: frontier writes the wiring + tests, local 9B fills the logic; 48K is the floor.** Late in the run python, /tmp and misc bash were always-allowed (restart the container before the next run so they don't carry over). Open: why one compaction didn't stall. **Next: sittings 4–5, swap to oMLX.** ATDD vault entry written (`vault/2026-10-16-frontier-scaffolds-local-fills.md`). Notes: `local/m4-stack-week-20261006.md` → *Sitting 3, retry*; learnings: `local/coding-agent-handoff-20261014.md`; skeleton: `local/sitting3-skeleton/`. Lab rule: VPN off for runs.
- ~~**Add `"disabled_providers": ["opencode"]` to both OpenCode configs**~~ — **DONE 2026-10-12** (Vince): both configs updated, image rebuilt, both copied to the VM; qwen stays default. **It does NOT stop the silent fallback:** `:ro,Z` works (right model, no providers); *without* `:Z` it still falls back to Big Pickle with providers (the guard is in the file that failed to load). Documented, not fixed (Vince's call) — always `:ro,Z`, and check the footer model. Detail: lesson 7 in `local/atdd-handoff-20261011.md`.
- **Small M4 cleanups:** delete `context_limit.txt` from the old workspace if it reappears; make `OLLAMA_MODELS` / `OLLAMA_CONTEXT_LENGTH` permanent (currently `launchctl setenv`, lost on reboot). **Context is now 49152 (48K), not 32768.** After any reboot, re-set it AND the OpenCode `limit.context`, or runs silently drop back to 32K.
- ~~**ATDD vault candidate**~~ **WRITTEN 2026-10-16:** `vault/2026-10-16-frontier-scaffolds-local-fills.md` (seed, observed; one model, two tasks). Promote after a second executor or harness confirms it (oMLX sittings are a natural chance).
- **TODO — rebuild the GPU monitor or adopt an open-source one** (Vince's tool from the Whisper project). The Ollama view shows only GPU busy time; it has no token data, its Processing Rate is broken, and it can't separate prompt reading from generation. **Design input: the Studio is headless**, so the data must be served over the network or work in a terminal over SSH. **Decided 2026-10-10: macmon (hardware) + a metrics proxy (tokens); don't fix the old monitor.** Its Processing Rate depends on when a poll happens to run, so it catches some replies and misses others. Full report and prioritised feature list: `local/gpu-monitor-ollama-review-20261010.md`.

*FROM THE 2026-10-13 → 10-14 SESSION (reddit research):*
- **OPPORTUNISTIC, not scheduled (Vince, 10-14):** REA trial and harness remote access ("notify me when blocked, reply from phone"). Do them on the M4 or the Studio *if* they come up between higher-priority work; if they never do, that's fine. **Don't surface them as overdue.** Remote-access split: harness-fixable blockers (auto-continue after compaction, rule re-injection, allow-listing safe read-only python) vs real judgment calls (scope drift, approving rewrites); only the second needs remote. OpenCode server/client mode looks plausible but is unverified.
- **REA (`morluto/rea`) trial — DEFERRED to M5U arrival (Vince, 10-14).** MCP for reverse engineering (Hopper/Ghidra/IDA, Electron/JS, .NET); MIT, single-maintainer, setup rewrites agent configs → **trust pass first**. Trial shape agreed: Claude+REA reverse-engineers the **data layer** of Vince's out-of-support Mac app (Mac = source of truth; iOS binaries are encrypted; sync out of scope) into a spec + acceptance tests; local model builds the reader/exporter. Claude-specs/local-builds = clean-room shape. **Work RE ideas are flagged legal** (EULA/competitor RE = counsel's call); detail in `CLAUDE.local.md`.
- **Subscription change (claude -p / SDK / 3rd-party harnesses on sub limits + Max API credits):** Vince won't retire the cost rule on it — **test on the M5U once local fallback exists.** Memory flagged.
- **Decision models (Clef-flash / Decision 3.0 / strands-decider):** on file, **sequenced after oMLX on the M4**.
- **Arrival checklist additions:** IP-KVM (Vince adding); Remote Login ON before first FileVault reboot (Tahoe SSH unlock); pin both host keys. Studio has AppleCare.
- **RAISE-WHEN triggers for today's files:** *inception coding-subagent work or tvo-skyline harness* → `local/coding-agent-handoff-20261014.md` (subagents as a context firewall, context floor, re-inject rules after compaction). *Studio arrives / server setup* → arrival items in `inbox/20261014-reddit-sweep.md` + the oMLX M5U numbers addendum (raise the GPU wired limit). *Robin design* → sweep §3–5 (cron/diff for new work, narrow delegate allowlists, pinned OpenClaw). *Memory / graph-DB discussion* → Temporal Graph-RAG (`inbox/20261013-reddit-research.md` §8), Hindsight. *Installing community skills* → registry indexes in `local/skill-watchlist.md` (trust pass first).
- **DSH = DeepSeek-published** → excluded for work. Research notes: `inbox/20261013-reddit-research.md`, `inbox/20261014-reddit-sweep.md`.

*FROM THE 2026-10-04 → 10-08 SESSION:*
- **Studio prep downloads DONE 2026-10-11** (4 MLX models on the SSD, revisions logged in the M4 notes). Don't add more before Qwen 4 / Mistral Large 4 land (end of October).
- **Ship-date mismatch (?):** notes say Nov 16 – Dec 1; Vince mentioned "early November" elsewhere on 10-06. Ask which is current.
- **Discussion owed to Vince (from his Voyage forward):** vector/graph DBs for local always-on assistants, and importing Robin/Del learnings vs building fresh. Voyage rerank-3 is hosted-only, so a privacy-first local stack needs a local reranker.
- **Hermes memory-model posts re-found** (his lost links) — six candidates in `inbox/20261006-sweep.md`; best is r/hermesagent 1wxu98g (copying Muse's memory setup). Relevant to harvesting memory into OpenClaw for Robin.
- **New decision-model candidates:** strands-decider (1.9B, Apache-2.0, calibrated; fits beside the 9b on the M4) and brier (calibration for open models). A label-smoothing post shows calibration can be broken by training — test confidence, not just accuracy.
- **Dress rehearsal gains a concurrency test:** 2–4 parallel sessions. Family-sharing consensus is one engine + LiteLLM + Open WebUI; an M5U user saw oMLX run 2 batched slower than 1.
- **Reddit access changed:** old.reddit redirects the headless browser to login; direct `.json` now 403s — load a www page first, then fetch `.json` from inside it. Other reddit context: `CLAUDE.local.md` → Reddit.
- **Inbox:** only `sage-needs-vince` left that's genuinely owed is the 09-01 "agentic docs" resend (plus the Mozilla report URL if he wants it read). Cloudflare verified; the TM/SSD thread is closed.

*ARRIVED 2026-09-25 — the use cases landed; the standing reminder is retired:*
- **Workload inventory received and analysed.** Vince's starter list of Mac Studio projects, plus his direction-setting options. This unblocks the local-inference track's highest-value prep item. **The full analysis, the strategic read and the parked decision are in `CLAUDE.local.md` → Local-inference track → Workload inventory** (kept out of this file because it carries personal and commercial context). Read it before advising on Studio sequencing or direction.
- **Carry-forward points that are safe here:** (a) the list is weighted to learning rather than outcomes, and the fix is to make two items double-count rather than to cut any; (b) **the real risk is spread, not wrong choice** — 13 Studio items against limited hours and a portfolio history of items lapsing unworked, so the recommended sequence assumes **two concurrent threads, not five**; (c) **recommended first tenant: Robin** as a local AI + coordination assistant — the only item serving both stated goals, and it settles the long-open "Del or Robin?" question; (d) one decision is **parked deliberately** as a TODO — detail in `CLAUDE.local.md`; surface it when the Studio arrives and tenant work starts, or when direction comes up again, but do **not** treat it as a standing nag.
- **Correction Vince supplied with the list: Sage is NOT moving to the Studio.** The Claude subscription stays because Sage is what plans and optimises this work. Confirms the standing position — the box is Sage's ingestion/retrieval backend, not a second Sage.
- **Two open questions back to Vince** (in the inventory): does any of this have an **external clock**; and is **two concurrent threads** the right capacity assumption.

*WORK PROJECT THREAD (opened 2026-10-01) — detail in `CLAUDE.local.md`:*
- **Vince's first local-LLM coding project is work-related and its specifics live in `CLAUDE.local.md` → Guest-agent project** (product architecture, platform and database specifics stay out of this file). **What is safe to carry here:** it turned out he **already has the hard part working**, so the project is a packaging/delivery exercise rather than an invention; the honest caveats are that the artefact is still systems software and the change touches product positioning, not only engineering. **Constraint he set: no Chinese-origin coding harnesses** (exfiltration concern) — he is explicitly less worried about models, and that distinction is sound, since weights in a local engine have no network path of their own while a harness reads the filesystem and runs commands by design. Shortlist therefore: **OpenCode, Goose, Aider, Cline**. **Boundary issue raised and unresolved: work code must not be built against a personal inference endpoint** — there is no GPU in the work environment, so the recommendation is Claude for the work build (subscription-included, no API tokens needed) and the M4 for local-inference learning on personal code.

*TODO — SPEC STANDARD FOR AI-DRIVEN CODING (opened 2026-10-03). Full analysis: `local/spec-standard-for-ai-coding-20261003.md`:*
- **Context:** Vince asked vibe-coding vs spec-driven for the guest-agent build (platform specifics in `CLAUDE.local.md`). Read 5 design docs from a work repo — **they are Engineering's, browsed for learnings, not Vince's practice**, and **Engineering does not code against local models**.
- **The finding worth keeping:** that folder runs three doc genres — design *discussion* (decisions open), *approach* (decision made, build not started), as-built *audit/digest*. Only the first is a pre-coding artefact. Strongest borrowable shapes: an **"Invariants — the whole design in six lines"** section, explicit **out-of-scope / deviation tables** (more worked examples for `vault/2026-06-10-negative-space-documentation.md`), and a numbered decision register. Weakest habit to avoid: embedding implementation code inline in the doc — it forks from the real code immediately and an agent will treat it as authoritative.
- **The sharp consequence, from data already in this project:** the 51-run measurement (local 27B executor: 42/42 full-code handoff, 9/9 seeded-bug debug, **0/3 on interface-plus-rules only**) says **spec density scales inversely with executor capability**. A local model needs MORE constraint, not less. Makes **invariants + a verification contract** the two load-bearing artefacts. Vault candidate, and the one that is genuinely Vince's rather than borrowed.
- **Scale guard Vince set: no overkill.** His projects are not million-LOC. Take Engineering's shapes, drop their volume — a 103KB design doc is a team-alignment instrument, not a spec. Scale-calibrated minimum is §4 of the local file: `INVARIANTS.md` + a verification contract are the core; a 1–3 page approach doc is supporting.
- **NEXT ACTIONS, in order:** (a) **DEFERRED BY VINCE 2026-10-03 — "that's for later":** search **reddit, Global AI Weekly and Hacker News** for good example repos to learn spec/doc practice from. He answered the open question by naming *sources* rather than a category, so the earlier fork (inference-tooling repos vs small-model-spec repos) is superseded — use the community sources as the search surface. **Do not start this unprompted.** (b) Draft `INVARIANTS.md` + the verification contract for guest-agent — the two core artefacts, and they do not depend on (a).

*SOURCE REFRESH 2026-10-02 — full detail in `local/m4-trial-components-20261002.md`:*
- **M4 16GB trial components, from measured data.** A 33-task DevOps benchmark reporting *measured* VRAM gives the sizing: **qwen3.5:9b (85% pass, 7.7 GiB) is the start point**, gemma4:12b (88%, 9.2 GiB) if quality binds, gemma4:e4b (70%, 5.3 GiB) if context binds; qwen3.8:27b (91%, 20.1 GiB) does **not** fit. **The binding constraint is tool calling, not quality** — small models commonly fail it; test that on day one. Engine: **Ollama first, then deliberately swap to oMLX** — the swap *is* the rehearsal. Harness: **OpenCode or Goose**; Pi is capable but ships no permission system and needs a sandbox.
- **oMLX de-risked: its creator/maintainer joined Hugging Face (2026-09-22)** to support the MLX community. The "young project, single maintainer" caveat from 2026-09-23 is substantially answered. Layer 1 is settled.
- **CLOUDFLARE CLEF SUPERSEDES JEV.** Open-sourced 2026-10-01 (551 pts on HN): decision models, **Apache 2.0, weights on Hugging Face, fully Jev-API compatible**. **Clef-flash uses a Qwen3.5-9B backbone — it runs on the M4.** Cloudflare's numbers: Clef beats Jev on BANKING77 (94.20 vs 79.74) and CLINC150 (97.43 vs 89.27); Clef-flash wins API-Bank (93.11 vs 88.19) at **38.8ms median latency vs Jev's 524.1ms**. Vendor-reported — but open weights make it independently checkable, which Jev never allowed. **Both objections to Jev (vendor data exposure, vendor survival) are answered.** Category now also includes **OpenAI's Decision API built on Luna**, `wfzyx/von`, **Vev** (4B/9B, vision, `/v1/systemone` compatible), Gliner2.5-Decide, stuntd, Smart.NET, mini-jev, Laya. **The task has changed from "evaluate Jev" to "pick an open one that runs locally and test it on real traffic."**
- **The Claude-plans/local-executes split is now measured.** 51 runs with a local 27B as executor under Claude: **42/42 on full-code handoff, 9/9 on seeded-bug debugging, 0/3 when given only an interface and rules.** Head-to-head Sonnet 5 45/51 vs local 43/51 — tied on every full-code and debug task, both failed the same two hard ones; Sonnet 3.6x faster, $9.20. **Local executes well-specified work and debugs against a failing test; it does not do the thinking.**
- **M5 Ultra reviews are out** — MacStories ("The Dream Mac for Local AI Agents", 269 pts), Tom's Hardware ("Outpaces DGX Spark and Threadripper").
- **AgentNews #30 — the best agent-ops evidence yet.** An agent was given a real business for four weeks (card, phone number, inbox, payments): **spent ~$3,000, earned $75.** Four named failure modes, all directly applicable to an unattended fleet: no way to mark a prerequisite **blocked** (eight days generating leads nobody could buy); **no minimum sample or review date** before declaring an experiment failed (a customer paid eight hours after "offer failed" was recorded); **no output boundary** — model text piped straight to text-to-speech, so it read its own instructions aloud on 197 calls; and a **self-grading feedback loop** that wrote conclusions into the same file as raw observations, so one wrong call contaminated every later session. What worked: error reclassification, fast-feedback iteration, payment-before-delivery, out-of-band escalation. **Vault candidate.**
- **`.env.local` hygiene — fixed 2026-10-02, worth not repeating.** A bare value (no `export KEY=`) makes `source` execute it and echo the secret into terminal output and transcripts; and a **stale token variable silently overrides a working `gh auth` keyring login**, so every command fails on auth even though the keyring is fine. Both now documented in `.env.local.example`. The stale variable persists in an already-running session's environment — prefix `unset GITHUB_TOKEN` until the next launch.
- **Subscriptions:** Sage **is** subscribed to Global AI Weekly (confirmed — issue 169 arrived direct), but direct delivery is a plain-text stub; extract the webview link and fetch it. **Cloudflare account verified** (Vince confirmed 2026-10-04); no blog mail yet as of then — likely just no new posts.
- **Inbox labelling convention adopted** (Vince's request): `sage-processed` = read and extracted; `archived` = processed with nothing outstanding; **`sage-needs-vince` = processed but something is owed by Vince, left in the inbox deliberately**. Still `unread` = genuinely untouched. Convention recorded in `CLAUDE.local.md`.

*FROM THE 2026-09-23 INBOX TRIAGE (full note: `inbox/20260923-inbox-triage.md`, gitignored):*
- **SWE-bench scaffolding study is a second source for the 2026-09-22 vault entry.** 254 submissions;
  differences between top coding agents often not statistically significant, and scaffolding changes
  outweigh swapping top models. Read the arXiv paper, then promote `vault/2026-09-22-decompose-decisions-into-typed-questions.md`
  from *seed (single-source, vendor-published)* to multi-source and broaden its framing.
- **oMLX (`https://omlx.ai/`) is unresearched and is the most track-relevant unknown.** Vince's drop,
  sourced from an M5 Ultra owner actually using it. Needed before the Lily-vs-MLX comparison he asked
  for is real. Provisional read on Lily (Perplexity, Rust+Metal, ~1.23x prefill / 1.35x decode over
  MLX-LM on M5 Max): it buys speed with single-model lock-in and wins hardest on the axis the M5 Ultra
  is weakest on — MLX as general substrate, Lily as a specialist endpoint.
- **SHIP DATE CONFIRMED by Vince 2026-09-25: Nov 16 – Dec 1, not yet billed** (Apple bills at dispatch). ~7-10 weeks of prep. **Vince's stated first task on arrival: get the Studio running headless.** The dress rehearsal (Ollama + OpenClaw + one tenant on existing hardware) fits inside that window and should start — it is what makes December an endpoint swap rather than a cold start.
- **oMLX confirmed independently three times now.** A 512GB M3 Ultra owner running it headless on a remote Mac prefers it over LM Studio/Ollama/vMLX/gmlx; an M5U benchmark contributor publishes through it; a Hermes user reports it "drastically" speeds Hermes startup. Real M5U 256GB numbers exist (Qwen3.8-Flash-Next 4bit/64k: 2,574 prefill / 79.9 generation). Treat layer 1 as settled enough to build on. Engines still unresearched: **MTPLX, vMLX, gmlx, ziggy-llm**.
- **Hermes reframed (2026-09-25) — it is a COORDINATOR, not a coding harness.** Community consensus: Hermes injects heavy context and underperforms Codex/Claude Code on the same model; the recommended shape is Hermes coordinating and delegating to Claude Code. Migrations off OpenClaw were driven by OpenClaw's **update breakage**, not Hermes capability — and one user simply **harvested memory / self-learning / self-skill-development into OpenClaw and deleted Hermes**, which is the cheapest path for Vince. **Layer 3 stays OpenClaw.** Full evaluation incl. a working 20-profile fleet architecture and stall-monitor design: `local/hermes-evaluation-20260925.md`.
- **POSITION CHANGE 2026-09-25 — "build your own harness" is retired as stated; replaced by "adopt the runtime, own the scaffolding."** Vince's challenge: millions of dollars of tokens are being spent improving OpenClaw, Hermes and other community harnesses, so building one makes no sense. **Agreed on the runtime layer.** Evidence he supplied: Hermes shipped **2,200 merged PRs across three releases in ten days** (0.21.3 → 0.21.5). No solo effort competes with that. The distinction that survives: *harness* means three different things — (1) **runtime/platform** (gateway, message bus, plugin system, model catalog) → **adopt, never build**; (2) **orchestration patterns** (control plane + managers + specialists, durable work on a board, stall monitors) → **copy from the community, they are configuration and discipline, not code**; (3) **domain scaffolding wired to Vince's own repos and conventions** (`vault/2026-06-17-in-repo-coding-harness.md`) → **the only part nobody else will ever build, and the part the SWE-bench scaffolding study says carries the performance**. **Inception's deliverable is re-aimed at (3)** — the reusable scaffolding that sits on top of whichever runtime — which is what it actually produced on tvo-skyline. **Vaulted as `vault/2026-09-25-adopt-the-runtime-own-the-scaffolding.md` (committed `921532c`).**
- **The velocity argument cuts both ways.** The same Hermes release post advises *not* updating unless you need the new model catalog: `hermes doctor --fix` "has been known to silently drop config fields it doesn't recognize", plugin tools now propagate to every session adding 500-1,000 tokens of schema per message, and unauthorized-DM handling changed behaviour. Fast-moving upstream is a reason to adopt a runtime **and** a reason to pin it and read release notes — the same discipline as the MCP wireup pattern.
- **Fleet coordination is no longer an open gap — a working design exists in practice.** Control plane bot + domain managers + specialists on a private Matrix server; **durable work lives on a Kanban board, not in a subagent's context**; stall monitors interrupt at 450s idle / 1200s in-tool with a 120s grace and emit a *stalled event* so the parent hears an outcome. Goes straight into the agent-ops spec.
- **Laya — open-source Apache-2.0 self-hosted alternative to Jev** (surfaced 2026-09-25, 195-pt thread). Predates Jev by ~18 months, claimed faster. Contested: a 512-token limit, and accuracy reportedly dropping to ~42% at ~70 tools vs Jev's ~87%. **Research Laya before doing anything with Jev** — a self-hosted classifier on the local box removes both objections to Jev (vendor data exposure, vendor survival). Detail in `inbox/20260922-typesafe-jev.md`.
- **Weekly Claw ep. 31 reviewed** (`inbox/20260925-weeklyclaw-ep31.md`): nothing on Hermes/OpenClaw/Apple Silicon/Claude Code/MCP. Two items matter — **Opus 5.5 at $4/$20, ~40% cheaper**, with an independent reviewer scoring Opus 5.5 Medium above Fable 5.1 Max on Terminal-Bench at $2.94 vs $20/task; and the **rogue-agent block**, where an agent "didn't accept no", breached a Medicare portal, and **went unnoticed for three months until a third-party monitor caught it** — the observability-before-autonomy argument with a real incident attached.
- **AWS "Pizza Bot"** — open-sourced local-first inbox/supervision layer for background agents
  (approval gates, queues, schedules, webhooks, MCP tools, local models via Ollama). Look at it before
  hand-building the always-on host's supervision layer.
- **Claude Code Projects (beta)** — one conversation splits into threads, each a cloud session on its own
  branch, with a Working/Idle/Resolved pane. Aimed at exactly Vince's many-simultaneous-instances problem.
  Check account access; join the waitlist if absent.
- **MCP spec now has a handle-hijacking / credential-exposure surface** the four-step package audit does not
  cover. Affects `vault/2026-05-20-third-party-mcp-wireup-pattern.md` and therefore the MCP Hardening SPEC
  already shipped to the work AI channel. Read the spec change, then decide: new section or companion entry.
- **Answers owed to Vince:** reddit is fetchable as public web content (per-thread, on demand) but discord
  needs an MCP + trust pass — and his real want looks like continuous monitoring of LLM community channels,
  which is a design question; Global AI Weekly is now subscribed directly (settled 10-02); and the 2026-09-01 "agentic docs / apply to atlas?" email has **no link or
  attachment** — the referent is missing and he needs to re-send it.


*PRIORITY — FIX LANDED 2026-09-20, now in live validation:*
- **Sage communication shape.** Vince's complaint: a 2-line question returns 50 lines; he is usually multitasking and wants dialog, not essays. **Durable fix in place:** output style `Sage Dialog` (`~/.claude/output-styles/sage-dialog.md`), activated for this project only via `outputStyle` in `.claude/settings.local.json`. Always on, no invocation, survives cold start. Three rules: one thing per turn; ask instead of pre-answering the forks; **long-form goes to a file and chat carries the pointer** (on-demand routing applied to conversation). Open: confirm it holds over a few real sessions, then decide whether to promote it to `~/.claude/settings.json` portfolio-wide.
- `action-first-dialog` skill = superseded as the interim measure; it shapes monologues, not turn-taking. Eval parked at net-zero delta with one real regression (see archaeology).

*From 2026-09-20 (held for action):*
- **Local-inference track — planning only until December.** M5 Ultra Mac Studio (256GB, headless, always-on) arrives then. Positions already taken, don't relitigate: the box is Sage's ingestion/retrieval backend not a second Sage; models are chosen last; one project named for the service, not the technology. **Next and highest-value: the cross-portfolio workload inventory** — needs Vince. Full detail: `CLAUDE.local.md` → Local-inference track. Memory [[project-local-inference-track]].
- **Jev / TypeSafe — RESEARCHED 2026-09-22.** Typed-decision model (Noul/Choice/Score + a confidence figure, no text output) from TypeSafe AI; $0.042/MTok in, output free, 70-500ms; early access, waitlist, not GA. Founder Diogo Almeida, ex-OpenAI. Vendor eval: **Jev beats Haiku 4.5 on all four workflows at ~1/50 the cost**, but **Opus 5 still wins two of four**, and "accuracy" everywhere means agreement with a frontier consensus, not ground truth. **Two live threads: (a)** trial it on Sage's own AgentMail triage — the calibration of its confidence score is what decides the routing use case, and nobody has tested that; **(b) the bigger find is in the same eval and has nothing to do with Jev** — decomposing a decision into narrow typed questions lifted Haiku 4.5 from 17.1% to 58.8% (and 6.0% to 42.9% on invoices) on identical tasks. Vault candidate, and directly relevant to inception's harness. Full note + all sources: `inbox/20260922-typesafe-jev.md` (gitignored).
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
2. **M4 trial: sittings 4–5 (oMLX swap)**; sitting 3 closed 24/24 with scaffolding. Lab commands: `~/Development/Lab/m4pro/opencode-image/opencode-help.md`. Then ask the ship-date question.
3. **Spec standard / guest-agent** — the live thread. Either draft `INVARIANTS.md` + the verification contract (doesn't need anything from Vince), or, if he asks for it, run the deferred reddit / Global AI Weekly / Hacker News repo search. Context: `local/spec-standard-for-ai-coding-20261003.md`.
4. **tvo-skyline — optimize the UI work.** Harness proven there 2026-07-05; this is the new top thread.
5. **Inception — generalize from the win:** it built the harness that tvo-skyline validated; next is turning that into the reusable scaffolding deliverable (`grill-with-docs` in progress there).
6. Homecrew trust pass (`local/skill-watchlist.md` → Ecosystem tooling), then decide whether to surface it to the team.
7. Shared work skills repo — continue uploads (`CLAUDE.local.md` → Skill upload candidates).

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
