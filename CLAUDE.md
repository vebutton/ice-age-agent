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
- [ ] First end-to-end Sage session — chat, vault entry, kickstart pack draft
- [ ] Absorb Robin's intelligence from `del-infra` (one-time pass)
- [ ] Wire up Evernote MCP
- [ ] Wire up Slack MCP for the work channel (see `CLAUDE.local.md`)
- [ ] First Kickstart pack for one of Vince's 2 hot personal projects
- [ ] AgentMail Python helper (address decision + wiring)
- [ ] First template-upgrade PR against `claude-agent-template`

## Session State
**Last session:** 2026-05-18 — bootstrap session. Walked through the conversation in `collateral/Ice-Age-agent.md`, locked v1 scope, wrote CLAUDE.md / system prompt / requirements, set up Python tooling.

**Key decisions made:**
- Project type: agent (Sage has persona, produces synthesized outputs)
- Output shape: all four (vault primary; kickstart packs, template-upgrade PRs, advisory docs downstream)
- Sources v1: Markdown notes, Evernote (MCP), Slack (one work channel, MCP), GitHub (gh CLI, personal + work), web, inbox/, AgentMail
- Robin's fate: absorb her intelligence into Sage; she keeps running in OpenClaw for now; Sage becomes the master
- Slack channel: one named work channel (name in `CLAUDE.local.md`, gitignored)
- AgentMail address: deferred to wireup time
- Tech: prompt-driven + light Python helpers for AgentMail and future glue

**First vault entry captured at bootstrap time** (2026-05-18-git-commit-skill-needed.md) — meta moment: Vince flagged that Claude was about to inline-HEREDOC the bootstrap's own first commit instead of using the file-based pattern. Exactly the kind of "skill-worthy, but reinvented every project" friction Sage exists to catch. Captured before the commit shipped.

**Open items for next session:**
- Vince to name the 2 hot personal projects so Sage knows what kickstart packs are coming.
- Decide if del-infra absorption is the first real Sage task or comes after a hot-project kickstart.
- Capture remaining seed patterns named in the system prompt (skills-complement-MCPs, master-agents-self-architect, Context Seven) as vault entries.
