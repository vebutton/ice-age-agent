# Sage — Requirements

The structured spec Sage is built against. Behavior rules live in
`prompts/system_prompt.md`; project context lives in `CLAUDE.md`; this file
captures the input/output contracts, integrations, and decision log.

---

## 1. Project Identity

- **Codename:** ice-age-agent (directory, repo).
- **Agent name:** Sage (phonetic origin of "Ice-Age" → "AI Sage").
- **Type:** Agent project — persona, structured language outputs, defined refusals.
- **Scope:** Personal first. Roll out to work when proven.
- **Interface:** Persistent Claude Code chat session on Vince's laptop (CLI or VSCode).
- **Primary model:** Claude Opus 4.7. Re-evaluate routing once usage patterns are visible.

## 2. Core Function

Sage closes the gap between **a learning being available somewhere in Vince's surface area** and **the next project benefiting from it**. She runs three loops: **Ingest → Distill → Feed forward**. The Learnings Vault (`vault/`) is the durable accumulator; every other output is downstream of it.

## 3. Input Sources

### 3.1 v1 (build now)

| Source | Mechanism | Notes |
|---|---|---|
| Markdown notes in Vince's project directories | Bash `find` + `grep` from Claude Code | Targeted scans, not broad sweeps. Vince names the directory/repo. |
| Evernote | Live API / MCP | Specific MCP TBD at wireup. Sage should preserve note metadata (date, tags) in vault citations. |
| Slack | Slack MCP | **Channel:** one named work channel, name in `CLAUDE.local.md` (gitignored — this is a public repo). v1 = this one channel only. |
| GitHub — personal repos | `gh` CLI | Commits, PRs, READMEs. Vince names the repo when relevant. |
| GitHub — work repos | `gh` CLI | Same as above; Vince has credentials on his work machine. |
| Web research | WebFetch + WebSearch (built into Claude Code) | Targeted lookups during sessions. No autonomous crawls. |
| `inbox/` folder | Plain markdown files | Manual drops from Vince (LinkedIn / Discord / Twitter / Reddit / event findings). Filename convention: `inbox/YYYYMMDD-<source>-<topic>.md`. Gitignored. |
| AgentMail (newsletters) | Light Python helper → drops to `inbox/` | **Wireup deferred** (see §6). Helper polls AgentMail API, writes each email as `inbox/email-YYYYMMDD-<subject-slug>.md`. |
| Robin's prior art | One-time absorption from `del-infra` repo + Robin's runtime data | Sage reads Robin's docs/code, extracts patterns into the vault. Ongoing handoffs handled session-by-session. |

### 3.2 Deferred (named, not built v1)

These are real input surfaces Vince wants eventually but should not block v1:

- **YouTube transcript ingestion** — Python helper (e.g., `yt-dlp` + transcript extract). Use case: AI talks Vince saved but hasn't watched. Trigger when first YouTube link Vince actually wants ingested arrives.
- **Local / in-person event ingestion** — format unknown (calendar export? notes?). Decide when Vince has a real example to point at.
- **Robin live API coexistence** — if Robin keeps generating insights in OpenClaw post-absorption, build a thin handoff (Robin writes to Sage's `inbox/` via API or shared filesystem). Decide once Robin's post-Sage role is clear.
- **Cross-machine sync** — Vince has personal + work laptops. v1 = single-machine. If Sage needs to run on both with shared vault, design later (likely: vault is in a git repo, both machines pull).

## 4. Output Artifacts

All outputs flow from the Learnings Vault.

### 4.1 Learnings Vault (`vault/`) — PRIMARY

- Location: `vault/` in this repo. **Committed.**
- Entry filename: `vault/YYYY-MM-DD-<slug>.md`.
- Index: `vault/INDEX.md` — one-line summary per entry, grouped by topic.
- Entry format (enforced by system prompt):
  ```markdown
  # [Title]
  **Pattern:** ...
  **Why it matters:** ...
  **Where this came from:** ...
  **How to apply:** ...
  **Status:** seed | confirmed | promoted-to-template | obsolete
  ```
- Promotion path: `seed → confirmed → promoted-to-template` (or `obsolete`).
- Hard rule: **every vault entry cites its source.** No source = no entry.

### 4.2 Kickstart packs (`output/kickstart-<project>-YYYYMMDD/`)

When Vince says "I want to build X," Sage produces:
- `PRD.md` — tailored project brief synthesized from the conversation + vault.
- `CLAUDE.md.seed` — a populated `CLAUDE.md` ready to drop into the new project's bootstrap, with relevant vault entries already referenced.
- `notes.md` — what Sage drew from the vault, why, and what gaps remain.

Sage **does not** create the new project's repo. She produces the seed pack; Vince (or his existing bootstrap workflow) creates the repo.

### 4.3 Template-upgrade PRs against `claude-agent-template`

When a vault entry graduates to `promoted-to-template`:
- Sage drafts the diff against `claude-agent-template` locally.
- Shows Vince the diff and rationale.
- **Requires Vince's explicit go-ahead before pushing.** Sage never auto-pushes.

### 4.4 Advisory docs (`output/advisory-YYYYMMDD-<topic>.md`)

One-off "you should look at X" notes. Format:
- What it is
- Why it matters to Vince's current stack
- How to try it (concrete next step)
- Source

## 5. Constraints / Refusals

- **No autonomous GitHub pushes.** Every push gated on Vince's explicit OK.
- **No code without alignment.** Sage drifts toward advisory output, not implementation. Implementation belongs in the project being advised.
- **No claim without source.** Vault entries always cite. Mark inference as `Status: seed`.
- **No broad scans by default.** Targeted reads only. "Scan all my repos" requires Vince to confirm — token budget is real.
- **No drift outside this project directory** without Vince's explicit OK. Per Vince's global rules.
- **No work bleeding into other Claude Code instances.** Sage advises; she does not reach into other CC sessions.

## 6. Open Decisions

These are real questions deferred from bootstrap. Each should be resolved when Sage hits the relevant moment, not preemptively.

| Decision | When to resolve | Default if Vince is unavailable |
|---|---|---|
| AgentMail address — Sage's own vs. share Robin's | At AgentMail helper wireup | Defer further — don't pick silently. |
| Evernote MCP — which implementation | At Evernote wireup | Pick the most-starred maintained one, confirm with Vince. |
| Slack MCP — which implementation | At Slack wireup | Same as above. |
| Cross-machine Sage (work laptop + personal laptop) | When Vince first tries to use Sage on the second machine | Stay single-machine until asked. |
| Model routing (Opus → cheaper for low-stakes) | When token usage shows a clear pattern | Stay on Opus 4.7. |
| Whether Sage absorbs Robin first or kicks off a hot personal project first | First post-bootstrap session | Ask Vince. |

## 7. Bootstrap Status (2026-05-18)

- [x] CLAUDE.md populated from template
- [x] System prompt written
- [x] Requirements doc written (this file)
- [x] Python tooling (pyenv + uv) — `pyproject.toml`, `.python-version`, `uv.lock`
- [x] `vault/` directory seeded with INDEX
- [ ] del-infra existence + shape verified (deferred to first real session)
- [ ] Bootstrap step 13 — finalize-and-ship (awaiting Vince's go-ahead)
