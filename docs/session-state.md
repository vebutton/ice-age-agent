# Session State — archaeology

> Companion to `CLAUDE.md`. The project root `CLAUDE.md` Session State holds the **forward-looking** operating context (open items, next-session pickup, continuity, task list state). This file holds the **backward-looking** archaeology — what got done thread-by-thread, decisions Vince articulated, memories banked, commits made. Consult on demand when archaeology is needed.

---

**Last session:** 2026-06-10 evening. Five threads: two new work repos scanned + review closed; scrub rule broadened; AI-call outcome recorded (skills objection resolved); negative-space documentation vaulted; git-commit skill plan retired in favor of a global rule. Five commits (`3ab2999`, `d0da8e0`, `795b0d3`, `42bdcd2`, plus the session-close commit). Two memories *updated* (none newly banked).

**Prior sessions:** 2026-06-10 midday (short; on-demand routing → bootstrapping agent; Fable 5 research + model pin; commits `51858c6`, `dc4a7e2`) · 2026-06-05..09 (multi-day, eight threads, two memories banked). Threads for both retained below under *Earlier archaeology*.

## What got done — 2026-06-10 evening session

### Thread A: Two additional work repos — PAT landed, AI-marker scan, review closed

- The org-admin approval for the PAT request (submitted 2026-06-09) landed; both repos readable. First action: full recursive tree scan of each default branch (both untruncated) grepping for AI markers — CLAUDE.md, AGENTS.md, `.claude/`, `.cursor*`, `.mcp.json`, copilot/GEMINI files, skills, prompt/LLM paths.
- Result: both repos are greenfield for AI tooling. Classic pre-AI-era OpenStack codebases. Caveat recorded: default branch only (~90–100 branches each unswept — acceptable per the partial-coverage-is-fine discipline).
- **Vince closed the review**: no full source pass. The scan is the complete review for these two; the Project Review Queue (CLAUDE.local.md, where the repo names live) records the closure.
- This also completes the live PAT validation that Task #3 (qualitative test of the `setup-github-pat` skill) was waiting on.

### Thread B: Scrub rule broadened — scan findings about work repos stay out of committed content

- Vince rejected a draft commit message containing the explicit scan finding (the literal asset-count result) while accepting the neutral "greenfield for AI tooling" phrasing; he then confirmed the same scrub for CLAUDE.md file content (commit `d0da8e0` applied it).
- New scrub category appended to the `feedback-no-company-name-in-checkins` memory: explicit audit/scan results characterizing the state of work codebases don't belong in ANY committed content. Neutral forward-looking characterization is the acceptable substitute; specifics live in CLAUDE.local.md.
- Note: commit `3ab2999` retains the original phrasing in history; Vince explicitly let it stand ("that's fine").

### Thread C: AI-call outcome recorded — skills objection resolved

- Vince's report-back from the 2026-06-10 morning call: engineers are onboard with building skills and have been trained in it; the CTO now understands the repeatability case. The pre-call "skills are overblown" stance is superseded — the prepared scale-with-project-count frame never needed to be argued as a fight.
- CLAUDE.local.md call-prep section converted to an outcome record (prep frame kept as a collapsed historical footnote).
- Practical consequence noted: the Phase-1 shared skills repo workstream is now politically unblocked AND has trained supply. Open threads deliberately left open: any new team commitments from the call; shared-repo creation date.

### Thread D: Negative-space documentation vaulted — tier-2 pair complete

- While preparing to write both queued tier-2 entries, discovered on-demand context routing was *already vaulted* (`vault/2026-06-09-on-demand-context-routing.md`, landed with the restructure) — the Project Status checklist was stale. Corrected.
- Wrote `vault/2026-06-10-negative-space-documentation.md`: record what was ruled out / rejected / deliberately absent, not just what was done. Agent-era double rationale: fresh contexts re-explore undocumented dead ends at full cost; positive-only docs are hallucination bait. Boundary rule: record the absences someone would plausibly assume or re-attempt, not every absence.
- Sources: handoff "What's been ruled out" section (Vince + Pocock; extraction was pre-announced in that entry), a standing agent-context doc observed in the 2026-05-26 work-repo review, Sage's own session archaeology schema. Steinberger's omission documented as the counter-signal. Status: seed (multi-source observed). Handoff entry now points here as canonical for the insight.

### Thread E: git-commit skill plan retired — rule promoted to global CLAUDE.md

- Vince clarified after a terminology mix-up: the `-F` commit-message tweak is too small to carry a skill ("I need more git capabilities than just the commit message file before we promote it to a skill"); since everything he does uses repos, it belongs in global `~/.claude/CLAUDE.md`.
- Done: rule added to global General Preferences (file-based message via `git commit -F`, no inline `-m`/HEREDOC, new-commit-not-`--amend` on hook failure, no `--no-verify` unasked). Reaches every project at cold start — previously the rule lived only in Sage's private memory.
- Vault `2026-05-18-git-commit-skill-needed.md` → status *resolved — promoted-to-global-rule; skill deferred* pending a `git-ops` bundle (hook recovery, PR-body files, force-push safety).
- The skills-extracted methodology entry gained a **placement spectrum** from this decision: one-line universal rule → global CLAUDE.md; project-specific rule → project CLAUDE.md; multi-step workflow with gotchas → skill. Demonstrated reuse is necessary but not sufficient — the artifact needs enough body to justify trigger machinery. The declined skill-ification is recorded there as a demonstration of the entry's own discipline.

## Decisions Vince articulated (2026-06-10 evening, durable)

- **No full source review** for the two newly-accessible work repos — the AI-marker scan closes them out.
- **Scan/audit findings about work repos' internals stay out of all committed content** — commit messages and file content alike; neutral characterization only.
- **Skill mass threshold**: a single valuable tweak is not a skill; it's a global-CLAUDE.md rule. Skills wait for demonstrated capability mass (the placement spectrum above).
- **AI-call outcome stands as recorded** — engineers trained + onboard; CTO gets repeatability; no further advocacy needed.

## Memories updated (2026-06-10 evening)

- `feedback_no_company_name_in_checkins` — new scrub category (scan findings), broadened to all committed content.
- `feedback_git_commits` — global rule now exists (memory is belt-and-braces); skill deferred; placement-spectrum principle noted.

## Commits (2026-06-10 evening)

- `3ab2999` — PAT + scan marked complete in Project Status.
- `d0da8e0` — scrubbed scan-finding detail from CLAUDE.md.
- `795b0d3` — negative-space vault entry + status closeouts (AI-call outcome, repo closeout, tier-2 checklist).
- `42bdcd2` — git-commit skill plan retired; vault + INDEX + Session State updates.
- *session-close commit* — this archaeology entry + brief refresh.

## Inbox saved (2026-06-10 evening)

None new.

---

## Earlier archaeology — 2026-06-10 midday + 2026-06-05..09 sessions

### Thread 0: On-demand Session State routing added to the bootstrapping agent (2026-06-10)

- Vince asked for paste-ready instructions for his `claude-agent-template` session (separate project, used to bootstrap new projects) to bake the brief+archaeology Session State split into the template, so future bootstrapped projects ship with on-demand routing from day one.
- Instructions delivered self-contained (carried the forward-vs-backward routing principle, not just the file shapes) since the template session doesn't have access to this repo. Included: CLAUDE.md brief skeleton, `docs/session-state.md` placeholder, the standardized pointer line, bootstrap-only scope (no migration of existing projects), and a verify step.
- **Vince confirmed implemented.** The bootstrapping agent now seeds on-demand Session State routing into every new project. This makes the global `~/.claude/CLAUDE.md` rule self-propagating at project creation rather than retrofitted per project.

### Thread 0b: Claude Fable 5 capability research + vault (2026-06-10)

- Vince got `claude-fable-5` access via Pro for ~2 weeks; free through June 22, usage-credit after. Available in Claude Code + Cowork. Dropped a benchmark screenshot in `collateral/` (filename has a U+202F narrow-no-break-space — needed glob/loop to read, not quoted path) + the announcement URL.
- Researched: read screenshot benchmark table + WebFetch'd the Anthropic announcement. Synthesized a capability brief angled at Vince's bootstrapping use.
- Key findings: SWE-Bench Pro 80.3% / Terminal-Bench 88% / FrontierCode 29.3% (agentic coding standout); file-based memory 3× over Opus 4.8 (independent validation of [[on-demand context routing]] — Sage's own architecture); vision→code from screenshots; auto-fallback to Opus 4.8 on cyber/bio/distillation queries (the starred benchmark rows).
- Vaulted as `vault/2026-06-10-claude-fable-5-capability-profile.md` (reference / single-source) + INDEX row. **Kept project names out of the committed vault entry** — the bootstrap roadmap (which names a work-product POC) went to gitignored `CLAUDE.local.md` → "Fable 5 bootstrap window" instead, with a scrubbed pointer in committed `CLAUDE.md`.
- Bootstrap roadmap for the free window (in CLAUDE.local.md): new "coding harness" project first → top priority is the work-product POC leveraging that harness → solar-sync (personal) → Sage itself runs on Fable 5 next session.

### Thread 1: First plugin install — Anthropic skills marketplace + example-skills plugin (2026-06-05)

- Confirmed install commands from the repo README: `/plugin marketplace add anthropics/skills` then `/plugin install example-skills@anthropic-agent-skills`.
- Scoped install to `example-skills` only (12 skills incl. `skill-creator`, `mcp-builder`); skipped `document-skills` (heavy, irrelevant for now) and the standalone `claude-api` plugin (already loaded in this session via a different path).
- Hot-reload worked — no session restart needed. Knocks out one of Vince's "try plugins" TODO items.

### Thread 2: First skill-creator run — built `setup-github-pat` skill (2026-06-05)

- Drafted an intake brief at `output/20260605-setup-github-pat-skill-intake.md` (gitignored — local-only) distilling earned scars from prior PAT setup experience.
- Invoked Anthropic's `skill-creator` with the brief; scope tightened to work-org PAT setup only (linear SKILL.md, not router-with-references — per Vince).
- Skill saved to `~/.claude/skills/setup-github-pat/` (global, out of repo scope). Three files: SKILL.md + `assets/env.local.example.template` + `scripts/validate-pat.sh`.
- Eval rigor: vibe-with-me. No benchmark loop, no description optimization yet.
- First field validation arrived 2026-06-09 (Thread 5): the skill's Stage 3 ("submit-and-wait — assume the org-admin delay cuts scope") was the correct call when a PAT request had to be re-scoped due to approval timing.

### Thread 3: SSH-key question for a second Mac (2026-06-05)

- Discovered this Mac doesn't use SSH at all for git — operations are HTTPS-via-PAT. Both `gh auth` identities (active env-var fine-grained PAT + keyring) use HTTPS.
- Recommended: fresh key per-machine if SSH is wanted on the second Mac (never copy private keys between machines — audit attribution and revocation work per-key).
- Flagged that marketplace add against a public repo needs zero auth, so the second Mac may not need SSH for that use case at all.

### Thread 4: Pocock 7-step engineering workflow vaulted

- Created `vault/2026-06-02-pocock-7-step-engineering-workflow.md`. Verified `github.com/mattpocock/skills` against the live tree; 7 step names confirmed; 5 of 7 mapped to skills in the repo (`grill-with-docs`, `prototype`, `to-prd`, `to-issues`, plus `tdd` adjacent to Step 7 QA). Step 6 (execution) is residual pattern value, not a packaged skill.
- 2026-06-09 update: first executed worked example landed earlier in the week. Step 1 (`grill-with-docs`) applied to a personal-track project. Vince's read: "worked incredibly well." Vault entry updated to reflect first executed worked example with positive outcome (replacing the previously-planned status).
- Status: *seed (single-source)* — single author so far, with one positive personal-track field test.

### Thread 5: 2026-06-09 update from Vince — multiple substantive items

- PAT request submitted to admin for two additional work repos; one repo originally in scope was cut due to approval-timing risk. First field validation of the `setup-github-pat` skill's Stage 3 session-boundary call.
- Vince published a new `access-openstack` skill to the team-skills repo. Created in one project that had the capability, tested in a second project that needed it, then published — concrete worked example of the cross-project skill-extraction methodology.
- Vince articulated the *third* motivation for skills (beyond cross-project reuse and earned scars from multiple prior uses): same-project temporal recurrence, where `CLAUDE.md` or memory fail to surface a recurring lesson reliably across sessions (context overflow drops it, restart loses it).
- CTO will be on the 2026-06-10 morning AI call. CTO's stance: skills are overblown. Vince's read on the disagreement: CTO has built code without skills, but only across a single project.
- Correction banked as a project memory: Pocock's `grill-me` is deprecated in favor of `grill-with-docs`. Cross-source reference (a YouTube callout from Nate Herk) used the deprecated name; the corroboration applies to the workflow, not a separate skill.

### Thread 6: Skill-extraction methodology vaulted (2026-06-09)

- Created `vault/2026-06-09-skills-extracted-not-anticipated.md` capturing Vince's principle: build the capability concretely in the project that needs it; recognize skill-shape only when a second instance of the same need arrives; extract then.
- Extended same day with the same-project axis (the third motivation): not all reuse signal is cross-project; sometimes it's temporal within one project.
- Two parallel flows documented (cross-project / same-project). Cross-project has two worked examples (`handoff`, `access-openstack`); same-project axis is pre-worked-example, with `git-commit-from-file` named as the likely first.
- Status: *seed (single-source)* — Vince's own methodology articulated 2026-06-09.

### Thread 7: Meeting prep for the 2026-06-10 AI call

- Created `output/20260610-skills-when-to-use.md` (gitignored — local-only) — short field guide on when a workflow rule should become a skill vs live elsewhere (memory, CLAUDE.md, bootstrap).
- Structure: what a skill is → three triggers to build one → when NOT to build one → cost of always-on metadata → how we know they work (Anthropic + Pocock + Steinberger convergence) → three concrete shapes → discipline → one frame for the room.
- Closes with: *"The skeptic who has built code without skills, but only across one project, is correct for that scope. The case for skills lives at the next scale up."* Diplomatic bridge — concedes scope, makes case at next level up, signals discipline over evangelism.

### Thread 8: CLAUDE.md restructure — Session State moves to on-demand routing (2026-06-09)

- Vince's observation: he sees CLAUDE.md Session State bloat across his projects. Asked whether to move it out.
- First proposal (split into two files, both loaded at start) was rejected by Vince as "equivalent to a huge CLAUDE.md" — same context cost.
- Refined: on-demand context routing — brief Session State stays in CLAUDE.md (~30-50 lines); full archaeology moves to `docs/session-state.md`; loaded only when more depth is needed.
- First implementation of the brief put *forward-looking* content (next-session pickup, continuity) into the on-demand file — Vince corrected: forward-looking content MUST be in CLAUDE.md (always loaded) because the agent needs it at cold start to behave correctly. This refinement banked as feedback memory.
- This commit lands the restructure plus a new vault entry on the pattern: `vault/2026-06-09-on-demand-context-routing.md`. Third worked example for the on-demand routing pattern (which has been a tier-2 vault candidate for weeks); promoted to seed (multi-source observed).
- Global `~/.claude/CLAUDE.md` updated with the new Session State routing rule, so future projects start with the brief+detail split by default.

## Decisions Vince articulated this session (durable)

- **Anthropic's `skill-creator` over Pocock's `write-a-skill`** for company-shared skill work — lower trust-overhead, neutral provenance.
- **Plugin install path** (`example-skills`) over manual single-skill copy — two birds (skill-creator + plugin experiment) in one go.
- **Per-machine SSH keys** — discipline recommended; Vince hasn't acted on the second Mac yet but it's the standing advice.
- **Pocock's `grill-me` → `grill-with-docs`** — single skill, renamed (banked as project memory).
- **Skills are extracted from demonstrated reuse, not anticipated** — both cross-project and same-project temporal axes are valid forms of demonstrated reuse. Vaulted.
- **CTO-call frame**: concede the skeptic's view at single-project scope; make the case at the next scale up. Common ground on the doc, not just in the remarks.
- **Session State follows on-demand context routing**: brief in `CLAUDE.md`, full archaeology in `docs/session-state.md`. Promoted to global rule via `~/.claude/CLAUDE.md` update.
- **The routing decision is forward vs backward, not summary vs detail.** Forward-looking content (open items, pickup, continuity, task state) MUST be in the always-loaded layer; only backward-looking archaeology goes on demand. Banked as feedback memory.

## New memories banked this session

- `project_pocock_grill_me_deprecated`.
- `feedback_session_state_forward_vs_backward`.

## Commits pushed this session

- `54aaafe` — vault accretion + CLAUDE.md restructure + Session State refresh.

## Inbox saved this session

None new.
