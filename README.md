# Sage (Ice-Age Agent)

A persistent Claude Code chat that closes the gap between *"someone learned this"* and *"the next project benefits from it."*

"Ice-Age" is the phonetic stand-in for **AI Sage**. The directory keeps the codename; the agent goes by Sage.

## What Sage does

Three loops:

1. **Ingest** — Markdown notes across projects, Evernote, one named work Slack channel, GitHub commits / PRs / READMEs (personal + work repos), web research, manual drops into `inbox/`, AgentMail newsletters.
2. **Distill** — write durable, sourced entries to the **Learnings Vault** (`vault/`). Each entry is one pattern, one source citation, one "how to apply." The vault is the primary artifact and the only thing that compounds.
3. **Feed forward** — when a new project starts, produce a tailored **Kickstart pack** (PRD + seeded `CLAUDE.md`) with relevant vault entries already baked in. When a learning should be permanent for *all* future projects, propose a PR against [claude-agent-template](https://github.com/vebutton/claude-agent-template). When something new and noteworthy surfaces, drop an advisory doc.

Sage runs in Claude Code on the user's laptop — no web app, no UI. Personal-first; rolls out to work once proven.

## Repo layout

```
CLAUDE.md                  Project context, loaded by Claude Code every session
CLAUDE.local.md            Local-only context (gitignored)
prompts/system_prompt.md   Sage's behavior rules: persona, refusals, vault format
docs/requirements.md       Input sources, output contracts, deferred decisions
vault/                     The Learnings Vault — the point of the project
inbox/                     Unprocessed inbound content (gitignored)
collateral/                One-time bootstrap inputs (gitignored)
output/                    Drafts: kickstart packs, advisory docs (gitignored)
src/sage/                  Light Python helpers (AgentMail polling, future glue)
```

## Origin

Bootstrapped from [claude-agent-template](https://github.com/vebutton/claude-agent-template) on 2026-05-18 — see that repo for the bootstrap workflow itself. The conversation that defined Sage is captured in `collateral/` (gitignored, one-time input).

Sage absorbs the accumulated wisdom of an existing research assistant the user runs in another harness, then becomes the project-aware Claude Code master going forward.
