# Sage (Ice-Age Agent) — Project Context for Claude Code

> "Ice-Age" is the codename / phonetic stand-in for **AI Sage**. The directory
> stays `ice-age-agent` for tooling/repo continuity; the agent itself is Sage.
>
> **File roles:** This file = project context (loads every session).
> `prompts/system_prompt.md` = Sage's behavior rules. Don't duplicate between them.

## User
Vince Button. Runs many Claude Code instances simultaneously across personal and work projects. Builds agents, Mac apps, Ansible, infra tooling. Has an existing OpenClaw assistant called **Robin** that handles research; Sage is the project-aware Claude Code successor that brings Robin's intelligence inside Claude Code where it can actually shape new projects.

## This Project
**Sage** is a persistent Claude Code chat that acts as an AI master / sage across Vince's personal and work AI projects. She ingests his fragmented knowledge (Markdown notes, Evernote, GitHub, Slack, web research, newsletters), distills patterns into a **living Learnings Vault** in this repo, and feeds that wisdom forward into new projects via kickstart packs, template-upgrade PRs, and advisory docs. The goal: close the gap between "someone learned this" and "the next project benefits from it."

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

**Outputs (all flow from the vault):**
- `vault/` — **living Learnings Vault**, primary artifact, committed to this repo
- Project Kickstart packs — tailored PRD + seeded `CLAUDE.md` for a new project, written into the new project's repo
- Template-upgrade PRs against `claude-agent-template` when a learning should be permanent
- Advisory docs — one-off "you should look at X" notes when something new surfaces

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
- [~] First end-to-end Sage session — research → synthesis → output cycle completed 2026-05-19 evening (three deliverables in `output/` for the internal AI meeting). Formal vault entry still pending.
- [ ] Absorb Robin's intelligence from `del-infra` (one-time pass) — on hold per Vince
- [ ] Wire up Evernote MCP
- [x] Wire up Slack MCP for the work channel — confirmed in-session 2026-05-19 evening. 30d of history pulled successfully; slack tools visible as `mcp__slack__*`.
- [x] Slack MCP hardening — pinned `slack-mcp-server@1.3.0`, source audit (trust-with-caveats), live network observation confirming Slack-owned endpoints. Binary sha256 recorded in `vault/2026-05-20-third-party-mcp-wireup-pattern.md`.
- [ ] First Kickstart pack — deferred until Sage has real intelligence to differentiate the pack
- [ ] AgentMail Python helper (address decision + wiring)
- [ ] First template-upgrade PR against `claude-agent-template`

## Session State
**Last session:** 2026-05-20 evening — full post-meeting debrief, MCP hardening end-to-end, two vault entries, two commits pushed. Clean state at close.

**What got done this session:**
- **Debriefed the rest of the preso feedback.** Refined the MCP build-vs-buy rule into a clean split: our own products → build our own MCP; 3rd-party apps → Anthropic-official first, well-supported community second. Memory `project-mcp-build-vs-buy-stance` rewritten with the new structure.
- **Sharpened the personal-first rule with a two-part bar:** enough learnings AND a path where externalizing doesn't become a new job. Vince's reasoning: in a small company, internal AI evangelism = an actual additional job assignment; AI is a multiplier for current work, not a new function. Memory `project-sage-stays-personal-first` updated.
- **New feedback memory `feedback-no-company-name-in-checkins`** — never write the employer name in commits or committed files; use "work" or "internal". `CLAUDE.local.md` is the only place for brand names. Portfolio-wide rule.
- **Two new vault entries:**
  - `vault/2026-05-20-claude-md-stays-lean-lazy-loading.md` — don't enumerate skills/MCPs in CLAUDE.md; the harness lazy-loads both.
  - `vault/2026-05-20-third-party-mcp-wireup-pattern.md` — env-var token + version pin + four-step audit + binary-vs-source caveat. Worked example with Slack MCP includes a full verification log (source audit summary, network observation result with captured IP + TLS-cert proof, binary sha256 hash for drift detection).
- **Slack MCP hardening fully closed:**
  - Pinned `slack-mcp-server@1.3.0` in `.mcp.json` (was `@latest`).
  - Source audit by subagent → trust-with-caveats: clean for our use, but npm ships a Go binary with no SLSA provenance, so source review only validates if binary == source.
  - Live network observation during a real `mcp__slack__conversations_history` call → captured `18.169.61.189:443` (AWS eu-west-2). Verified Slack-owned via TLS cert (`*.slack.com`, Let's Encrypt R13) and forward DNS (`slack-files.com` resolves to it).
  - Binary sha256 recorded: `8f0ba1fff09d61d51a090ef447995f778c0fd7234d6f1d6c568cb2fba0e7da83`.
- **Two commits pushed to `origin/main`** (Slack MCP wireup + Session State + lazy-loading vault entry; then this end-of-session commit with the hardening vault entry + CLAUDE.md update).

**Decisions Vince articulated this session:**
- MCP build-vs-buy rule has structure now: our products = build, 3rd-party = buy. Not "case-by-case."
- Personal-first bar is two-part (learnings + non-job structure), not just track record.
- Employer name never enters committed content or commit messages. Portfolio-wide.
- Pin + audit + observe is the standing discipline for any third-party MCP, codified in the wireup vault entry. Two strong template-upgrade-PR candidates against `claude-agent-template`: lazy-loading discipline + third-party MCP wireup defaults.

**Carried forward / open:**
- Remaining seed patterns from system prompt (skills-complement-MCPs, master-agents-self-architect, Context Seven) — still pending vault capture.
- Robin/del-infra absorption — still on hold per Vince.
- Two hot personal projects to name — when Vince is ready. Informs kickstart-pack prioritization.
- Evernote MCP wireup — not yet started.
- AgentMail Python helper — deferred.
- First template-upgrade PR against `claude-agent-template` — two strong candidates now: lazy-loading discipline + third-party MCP wireup defaults (gitignored `.env.local`, `${VAR}`-referenced `.mcp.json`, version pin, audit checklist).
- **Small breadcrumb worth noticing next session:** Vince posted twice in the work AI channel today about output-tooling friction — MD→PDF export chaos this morning ("it's a mess!"), Cowork's multimodal SVG-from-.eml capability this evening. Two data points in one day hinting at an output-rendering problem area. Could be a vault candidate or an advisory.

**Next session — pick up here:**
1. Cold start — re-read CLAUDE.md first.
2. No outstanding blockers. Reasonable next moves if Vince doesn't specify:
   - Capture one of the remaining seed patterns from `prompts/system_prompt.md`.
   - Surface the output-tooling-friction breadcrumb (above) as either an advisory or a vault candidate.
   - Move toward scaffolding the first kickstart pack.

**Session continuity:** Vince is closing this session and restarting. **Next session is a cold start — re-read CLAUDE.md before doing anything.** Launch from project root: `source .env.local && claude` (the `source` is required — without it the Slack MCP token won't be in env and Slack tools will silently disappear).
