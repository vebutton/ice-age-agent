# Session State — archaeology

> Companion to `CLAUDE.md`. The project root `CLAUDE.md` Session State holds the **forward-looking** operating context (open items, next-session pickup, continuity, task list state). This file holds the **backward-looking** archaeology — what got done thread-by-thread, decisions Vince articulated, memories banked, commits made. Consult on demand when archaeology is needed.

---

**Last session:** 2026-06-05..09 (multi-day; first plugin install + first skill-creator run + cross-source skill methodology articulated and vaulted + meeting prep for the 2026-06-10 morning AI call + late-session restructure of CLAUDE.md per on-demand routing). Eight threads. **Two new memories banked** (`project_pocock_grill_me_deprecated` + `feedback_session_state_forward_vs_backward`).

## What got done — by thread

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

- *this commit* — vault accretion + CLAUDE.md restructure + Session State refresh.

## Inbox saved this session

None new.
