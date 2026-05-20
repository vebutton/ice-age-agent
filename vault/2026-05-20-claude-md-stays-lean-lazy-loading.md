# CLAUDE.md stays lean — let the harness lazy-load skills and MCP tools

**Pattern:** Don't document skills or MCP tool inventories inside `CLAUDE.md`. The Claude Code harness already lazy-loads both. Putting that detail in `CLAUDE.md` wastes context on every session for things that are only used a fraction of the time.

**The two lazy-loading mechanisms:**

1. **Skills** — live in `~/.claude/skills/` (global) and project `.claude/skills/`. The harness surfaces each skill to the model as a single line: name + ~one-sentence description. The skill body (instructions, examples, scripts) is only loaded when the model invokes the skill. A project can have dozens of skills available at near-zero token cost until one is actually called.
2. **MCP tools** — configured in `.mcp.json`. Tool *names* are surfaced cheaply; full *schemas* are deferred and fetched via the harness's `ToolSearch` (or equivalent) only when a tool is about to be called. A Slack MCP exposing 15 tools adds 15 names to context, not 15 full JSONSchemas.

**Why it matters:**
- `CLAUDE.md` is loaded on *every* session, every turn. Anything in it pays its token cost continuously. Skills and MCP tools pay only when used.
- Listing MCP tool inventories in `CLAUDE.md` (e.g. "Slack MCP provides `channels_list`, `conversations_history`, ...") duplicates what the harness already surfaces — and worse, the static copy drifts when the MCP version changes.
- Same anti-pattern as [[2026-05-18-template-shouldnt-ship-process-doc-to-children]]: documenting elsewhere what the system already provides, then watching the copy decay.

**What `CLAUDE.md` should say about skills/MCPs:**
- **Which** MCPs are wired and **why** (e.g. "Slack MCP for #ai-claude-code-best-practices monitoring").
- **Which** skills are project-relevant or expected to be invoked.
- Runtime / launch notes that are not auto-discoverable (e.g. "must `source .env.local` before `claude` or the Slack token won't be in env").
- **Not**: tool-by-tool inventories, full skill descriptions, schemas, parameter lists.

**Where this came from:**
- Internal AI preso feedback debrief (2026-05-20). Vince recalled seeing a pattern of pointing `CLAUDE.md` at skill directories rather than enumerating MCPs inline.
- Confirmed in-session by observing this session's own context: ~40 MCP tools deferred via `ToolSearch`, skills listed as name + one-liner only. The mechanism is already there; the discipline is in *not undoing it* by stuffing inventories into `CLAUDE.md`.

**How to apply:**
- **In this repo:** audit `CLAUDE.md` for any MCP tool listings or skill descriptions that duplicate harness-surfaced info. None right now, but watch as Slack/Evernote integrations mature — resist the urge to enumerate.
- **In kickstart packs Sage produces:** the seeded `CLAUDE.md` should follow this rule. Reference skills by directory and MCPs by purpose, never by tool inventory.
- **As a template-upgrade candidate:** `claude-agent-template`'s `CLAUDE.md` scaffolding should bake in this discipline — short "## Skills" and "## MCPs" sections that name + justify, with a one-line note that tool details are auto-discovered.

**Status:** seed (will promote once a kickstart pack is produced and this discipline is exercised end-to-end)

Related: [[2026-05-18-template-shouldnt-ship-process-doc-to-children]] (same anti-pattern — duplicating what a live source already provides)
