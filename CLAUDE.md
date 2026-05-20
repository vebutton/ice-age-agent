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
- [ ] First Kickstart pack — deferred until Sage has real intelligence to differentiate the pack
- [ ] AgentMail Python helper (address decision + wiring)
- [ ] First template-upgrade PR against `claude-agent-template`

## Session State
**Last session:** 2026-05-20 morning — internal AI meeting at work happened. Short post-meeting turn: captured initial preso feedback (MCP build-vs-buy stance); more feedback still to debrief.

**What got done this session (brief post-meeting turn):**
- Discussed how to trust the community `korotovsky/slack-mcp-server` vs writing our own. Risk model: bot-token exposure, blast radius = channels the bot is invited to, `@latest` auto-update is the biggest live attack vector.
- Cheap mitigations identified (pin version, one-time source audit, network observation, scope discipline) — **none executed yet, all deferred.**
- Captured Vince's stance after preso feedback as memory `project-mcp-build-vs-buy-stance`:
  - For Sage: community MCPs only, no in-house build.
  - For future kickstart packs: MCP choice is per-project fit, not a default.
  - General principle: project-fit drives the decision (parallels the cost-frugality memory).

**Decisions Vince articulated this session:**
- Stay on `korotovsky/slack-mcp-server` for Sage. Don't maintain our own.
- For kickstarted projects, MCP build-vs-buy is part of the per-project fit assessment, not a default answer.

**Carried forward / open — IMPORTANT, several items unresolved across multiple sessions:**
- **🔴 More preso feedback to debrief.** Vince said *"I had some feedback during the preso about using community MCP servers vs writing my own"* and explicitly signaled there's more beyond what's in the MCP memory. **Next session should start by asking Vince for the full feedback list from the meeting.** This likely affects Sage's direction.
- **Sage-attribution sections in yesterday's advisory docs** — outcome unknown. Vince didn't say whether he stripped them, used them as-is, or skipped the docs. Ask, don't assume.
- **Uncommitted work piling up.** All of the following are still uncommitted:
  - `output/advisory-20260520-trilio-ai-meeting.md` (yesterday)
  - `output/skills-library-seed-20260520.md` (yesterday)
  - `output/howto-claude-code-slack-readonly.md` (yesterday)
  - `CLAUDE.md` updates (yesterday + today)
  - 4 new memory files + MEMORY.md (yesterday + today)
- **MCP hardening tasks deferred:** pin `slack-mcp-server@<version>` in `.mcp.json` (currently `@latest`), one-time source audit of the repo, one-time network observation. Worth doing before bot scope expands beyond one public channel.
- **Vault entry for Slack MCP wireup pattern** — still deferred. Source material already in `output/howto-claude-code-slack-readonly.md`.
- Robin/del-infra absorption — still on hold.
- Seed patterns from system prompt (skills-complement-MCPs, master-agents-self-architect, Context Seven) — still pending vault capture.
- Two hot personal projects to name (when Vince is ready) — informs kickstart-pack prioritization.

**Next session — pick up here:**
1. **First thing: debrief the rest of the preso feedback.** Don't dive into other work until that's captured — it likely informs everything else. Ask Vince openly: *"You mentioned more feedback from the meeting — what else came up?"*
2. Ask whether the advisory docs were used as-is or stripped.
3. Commit + push the pile (4 memory files, MEMORY.md, CLAUDE.md, 3 output docs).
4. Execute MCP hardening: pin korotovsky version, source audit, network observation.
5. Write the Slack MCP wireup vault entry from the existing how-to source material.
6. Revisit "Sage stays personal-first" rule based on meeting outcome.

**Session continuity:** Vince is closing this session and restarting. **Next session is a cold start — re-read CLAUDE.md before doing anything.** Launch from project root: `source .env.local && claude` (the `source` is required — without it the Slack MCP token won't be in env and Slack tools will silently disappear).
