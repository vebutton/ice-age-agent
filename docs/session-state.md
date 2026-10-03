# Session State — archaeology

> Companion to `CLAUDE.md`. The project root `CLAUDE.md` Session State holds the **forward-looking** operating context (open items, next-session pickup, continuity, task list state). This file holds the **backward-looking** archaeology — what got done thread-by-thread, decisions Vince articulated, memories banked, commits made. Consult on demand when archaeology is needed.

---

**Last session:** 2026-10-03. Short, single-thread: opened the **spec-standard-for-AI-coding** topic. Read five design docs from a work repo (**Engineering's practice, browsed for learnings — not Vince's**), extracted a three-genre model of design documentation, and landed the sharper finding that **spec density scales inversely with executor capability**. Full analysis in gitignored `local/spec-standard-for-ai-coding-20261003.md`. One memory banked, one commit.

**Prior session:** 2026-09-20. Communication-shape fix landed (output style `Sage Dialog`), `claude plugin eval` exercised against the interim skill and parked, first-pass planning for the local-inference track (detail in gitignored `CLAUDE.local.md`).

**Prior session:** 2026-09-18. Housekeeping pass after a ~10-week gap (no commits between 2026-07-05 and 2026-09-18) — cleared every expired date-gated item across the brief, the local file, the watchlist, the vault entry + INDEX row, and the model memory. No project work advanced; the queue is unchanged apart from the prunes.

**Prior session:** 2026-07-05. Vince reported **inception built the coding harness and proved it out on tvo-skyline** (tvo-skyline = test bed; inception unstalled; UI-work optimization is the next tvo-skyline phase). Evaluated **frontier** for UI work — ruled out. First real AgentMail triage: **Homecrew** logged (watchlist + local pointer), **AgentNews #15** distilled to `inbox/`, both threads archived. Session ran on **Fable 5** (access restored); model reconcile flagged. Two memories updated, one commit.

**Prior sessions:** 2026-06-26 (in-repo coding harness vaulted + Inception scaffolding spec; work skills repo confirmed live; Atlas overview created; Fable 5 restricted → Opus 4.8 standard; inception decomposition plan accepted; three commits) · 2026-06-10 evening (two new work repos scanned + review closed; scrub rule broadened; AI-call outcome resolved; negative-space documentation vaulted; git-commit skill retired to a global rule; five commits) · 2026-06-10 midday (short; on-demand routing → bootstrapping agent; Fable 5 research + model pin; commits `51858c6`, `dc4a7e2`) · 2026-06-05..09 (multi-day, eight threads, two memories banked). Threads retained below under *Earlier archaeology*.

## What got done — 2026-10-03 session

### Thread A: vibe coding vs spec-driven development — what to generate before coding

Vince opened a new topic for the guest-agent build (platform specifics in `CLAUDE.local.md`): he has written specs, but "in no
way to the extent that product have", and asked whether there is a good standard to follow and
what he and Claude should generate **before** coding to make AI-driven work go best. He offered
to upload engineering examples and linked one by URL.

**Framing given, and why.** The vibe-vs-spec dichotomy was declined rather than answered. The
SWE-bench scaffolding study (254 submissions — differences between top coding agents are often
not statistically significant, while scaffolding changes outweigh swapping top models) says the
leverage is in repo-specific scaffolding, not in which spec ritual is followed. External
standards (GitHub Spec Kit, Kiro, BMAD) were named and set aside as waterfall rebranded;
`vault/2026-06-02-pocock-7-step-engineering-workflow.md` is already the lighter alternative in
the vault, and `vault/2026-09-25-adopt-the-runtime-own-the-scaffolding.md` already says the
domain-scaffolding layer is the part nobody else will build.

**Rather than hand over a generic artefact list, offered to read his actual sources first.** He
chose "pull some and read 3-4". Five were fetched to scratchpad via the work PAT (`unset
GITHUB_TOKEN` prefix still required — the stale-variable gotcha from 2026-10-02 is live in any
already-running shell). The one he had linked turned out to live on a feature branch, not the
default branch.

**The finding: that folder is three document genres, not one.**

1. **Design discussion** — written before any decision; explicitly states that no implementation
   decision has been made; carries Approaches Considered with tradeoffs, Open Decisions, a
   suggested path forward marked *one perspective, not a decision*, and Out of Scope. **This is
   the only genre that is a pre-coding artefact.**
2. **Approach doc** — decision made, build not started. Background → Principles → chosen shape →
   phased implementation plan → migration. The contract an agent builds against.
3. **As-built audit / digest** — written during or after the build. Carries a deviation table
   (*What the requirement says · What ships · Where · Why*), inline marks where the draft was
   departed from, a numbered decision register, and a "requirement text to update" list.

The document Vince had linked as "perhaps this is one" is genre 3 — which is exactly why it did
not feel like an answer to his question. It is a handoff and requirement-correction record, and
a very good one; it is the wrong input for a fresh build.

**Shapes worth borrowing:** an **"Invariants — the whole design in six lines"** section (the
highest-value artefact in the folder for agent work); explicit **out-of-scope / deviation
tables** (two more worked examples for `vault/2026-06-10-negative-space-documentation.md` —
enough to firm that entry up); a **numbered decision register** with one line of reason each;
**matrices** where behaviour is multi-dimensional; a status/date header so an agent knows whether
the doc is live; and **ruled-out approaches preserved** so an agent does not re-propose them.

**Two gaps for agent-driven coding.** (a) No machine-checkable **verification contract** — one
doc has a test plan, none states up front the commands that must pass and the observable
conditions that prove the feature works. That is the difference between a loop that terminates
and one that declares victory. (b) The docs are too big to load (103KB / 72KB / 66KB) and one
embeds several hundred lines of implementation code inline, which forked from the real code
immediately — an agent handed stale inline code treats it as authoritative. The `-DIGEST`
convention already in that folder is the right instinct and is `vault/2026-06-09-on-demand-context-routing.md`
applied to design docs; the fix for the second is to reference code by path, never paste it.

### Thread B: two corrections from Vince, and the sharper finding one of them produced

1. **"That approach is not mine, it is Engineering's. We're just browsing it for learnings."**
   The analysis had been written as if describing his own house practice. Corrected throughout
   the local file and in the CLAUDE.md TODO — the material is borrowed, and attributing borrowed
   practice as existing practice would have misled a future cold start.
2. **"They aren't using local LLMs."** This one changed the substance. Engineering codes against
   frontier models, so their practice is a data point from the wrong side of Vince's actual
   problem. The answer their practice cannot give is already measured in this project's own
   session state: in the 51-run comparison, a local 27B executor scored **42/42 on full-code
   handoff, 9/9 on seeded-bug debugging, and 0/3 when given only an interface and rules**.
   Therefore **spec density scales inversely with executor capability** — a local model needs
   *more* constraint, not less — and the two load-bearing artefacts are the **invariants** and
   the **verification contract**, because those are what convert "thinking" into "well-specified
   work". Strongest vault candidate of the session, and the one genuinely Vince's rather than
   borrowed.
3. **"We don't want to do overkill — my projects are not going to be Million LOC any time soon."**
   Accepted and applied: the recommended pre-coding set was re-cut into a two-item core
   (`INVARIANTS.md` + verification contract) with the approach doc demoted to supporting and
   capped at one to three pages. Banked as a feedback memory.

### Thread C: deferred — scan community sources for repo examples

Vince asked for good examples of repos sourced from reddit, Global AI Weekly and Hacker News,
and explicitly deferred it: *"That's for later though."* This supersedes the question that had
been put to him (inference-tooling projects vs projects documenting small-model specs) — he
answered it by naming the **sources** rather than the category. Recorded as an open item, not
actioned.

## Decisions Vince articulated this session (durable)

- **Engineering's design-doc practice is borrowed material, not his own** — attribution matters
  for how a future session reads the analysis.
- **Right-size to project scale.** Take the shape of enterprise practice, drop the volume. Banked
  as `feedback_right_size_to_project_scale`.
- **Community sources are the next search surface** for spec/repo examples — reddit, Global AI
  Weekly, Hacker News — deferred to a later session.

## New memories banked this session

- `feedback_right_size_to_project_scale`.

## Commits this session

- Session-close commit: CLAUDE.md TODO block + Session State brief refresh + this archaeology
  entry. (Local analysis file is gitignored under `local/`.)

## Inbox saved this session

None new.

## What got done — 2026-09-20 session

### Thread A: Communication shape — Vince's complaint and the durable fix

- **The complaint, verbatim in substance:** "I cannot type a 2 line sentence and have you output 50 lines every time. You are my AI assistant so our communication should be more like a dialog." Context: he is usually multitasking. Opened as a **priority task** in the CLAUDE.md brief.
- **Interim measure:** he had already authored an `action-first-dialog` skill (global, `~/.claude/skills/`) and asked for it to be loaded. Loaded and active for the session.
- **Diagnosis that decided the fix:** the skill shapes a *monologue* — make the paragraph actionable. The complaint is about *turn-taking*. A perfectly action-first 50-line answer still fails the actual ask. Different problem, so a different mechanism was needed.
- **Durable fix:** Claude Code's **output style** mechanism — `~/.claude/output-styles/sage-dialog.md`, activated by `outputStyle` in this project's `.claude/settings.local.json`. Chosen over a skill because it modifies the system prompt persistently: always on, no invocation, survives cold start. The skill's own text concedes its persistence section "is not a mechanism". Scoped to this project only at Vince's direction; promotion to `~/.claude/settings.json` deferred until it proves out.
- **The three rules**, in priority order: (1) one thing per turn — answer the question asked, not the adjacent ones noticed while answering; (2) ask instead of pre-answering the forks — the single biggest source of over-long output; (3) **long-form goes to a file, chat carries the pointer** — this is the project's own on-demand routing applied to conversation rather than to session state. Sage's work product is knowledge artifacts, so a long analysis typed into chat is a vault entry thrown away.

### Thread B: `claude plugin eval` — first use, and what it cost to learn

Vince surfaced the feature from a Claude Code newsletter and asked for the interim skill to be tested "to make sure it is doing more good than harm". Capped at $5 on his instruction.

- **Early-access gate.** The command is gated; enabled with `CLAUDE_CODE_WALNUT_SPIRE=1`. The CLI explicitly warns against putting it in a repo's `.claude/settings.json` — shell or `~/.claude/settings.json` only. Not added to global settings; passed per-command.
- **Three failed invocations before any model call, all $0.** (1) Targeting by name from inside the skill directory resolves as a relative path. (2) A loose skill in `~/.claude/skills/` is not an installed plugin, so name resolution falls through to path resolution again. (3) With a path target, ablation fails outright: a skills-style directory with only a `SKILL.md` is **not auto-detected** as a plugin. Fix: declare `plugins: ["<abs path>"]` in each case's frontmatter.
- **Run 1 ($3.15) was invalid and the JSON did not say so.** Every case prompt opened with `/action-first-dialog` to force the skill to load. In the no-plugin arm that slash command does not resolve — all 15 baseline runs returned `Unknown command` and ended at **0 turns**. Deltas of +1.00 looked like a triumph; the control had simply died. **Lesson worth keeping: check the baseline arm actually answered before reading any delta.**
- **Run 2 ($4.98, 20 min, 3 runs/case) was valid.** Mean delta **0.00**. Two cases improved (+0.33 each), two unchanged, one regressed (**−0.67**).
- **The one real finding:** on "what are my options", baseline passed 3/3; under the skill, 1 pass / 1 fail / 1 timeout. The failing run replaced the options list with a decision heuristic ("two questions decide almost everything…"). The skill's own rules already forbid this — its §5 says an options question gets 2–4 ranked options — and the model read that and did it anyway. A wording problem in the skill, not grounds to abandon it. The −0.67 is inflated by the timeout (300s, 0 turns, infrastructure not behavior).
- **Two limits on the exercise, stated rather than glossed:** n=3 cannot resolve deltas that small — one run flipping moves a case by 0.33, larger than most deltas measured. And the cases were generic coding prompts, not Sage's actual traffic (architectural advice, research synthesis, cross-project knowledge), so the eval measured the skill against someone else's workload.
- **Parked** at Vince's direction in favour of the communication fix. A grader flaw was also found and fixed mid-run: "first line must be an action" penalised a correct caveat about missing project files, scoring a genuinely good response 0.00.

### Thread C: Local-inference track — planning only

Vince has bought an M5 Ultra Mac Studio, arriving December. Planning conversation only; no artifacts committed. **Detail lives in gitignored `CLAUDE.local.md`** under the personal-track priorities, per the standing rule on keeping purchase/monetisation context out of committed content. Hardware and routing findings that are portable will become a vault entry once there is evidence behind them — deliberately not written yet, since a routing rule promoted off hardware specs and secondary blog coverage would be exactly the thin-evidence elevation Vince has corrected before.

### Inbox triage (AgentMail)

7 unread found, 3 acted on: the Claude Code newsletter carrying the plugin-eval feature (used, above); Global AI Weekly #167 with Perplexity's **Lily** — a Rust+Metal inference engine that skips PyTorch and MLX to hand-optimise one model on one hardware family, trading generality for speed; and two items relevant to always-on agent work — a credential-stealing weakness in the new MCP spec, and AWS open-sourcing an agent built on the premise that watching an agent work defeats the point of delegating. A fourth thread ("agentic docs") arrived with its forwarded content missing and only Vince's question attached; flagged back to him rather than guessed at.

### Thread D: Atlas overview refresh + one open question closed

- `.atlas/overview.md` refreshed (gitignored): two accomplishments added — the output-shape fix and the plugin-eval methodology finding — todos reordered with the output-style validation and the local-inference host on top, and two new entries in *Context not in the repo* covering the communication constraint and the local-inference positions.
- **Homecrew repo URL confirmed**: `github.com/with-logic/crew` (site `crew.logic.inc`, Logic Inc.). The July newsletter linked through tracking redirects so the real URL had never been captured. Corrected in `local/skill-watchlist.md`. The **trust pass itself remains open** — knowing the repo is not auditing it.

### Decisions Vince articulated this session (durable)

- **Follow the global model default** — project-level model pin deleted, not to be reintroduced without an explicit ask.
- **Output style scoped to this project for now**, not portfolio-wide.
- **Park the eval**, fix the communication problem first.

### Memories banked

- `feedback_output_shape_dialog` — dialog not essays; the three rules and the mechanism.
- `project_local_inference_track` — the December machine, the motivations, and the positions already taken.

## What got done — 2026-09-18 session

### Thread A: Expired-item housekeeping pass (Vince's call: "housekeeping first")

Context: session opened as a cold-start catch-up after ~10 weeks idle. Nothing had moved in the repo since `7d4a9a2`, so several date-gated items had silently gone stale. Vince chose the housekeeping pass over resuming tvo-skyline or inception.

Prunes applied (all verified by exact-string replacement, one match each):

- **`CLAUDE.local.md` — "Fable 5 bootstrap window (2026-06-10 → June 22)" section replaced** by "Personal-track project priorities". The free-window rationale, the `claude-fable-5` pin instruction and the "REVERT after June 22" reminder are gone; the four-way priority ordering survived and was **re-stated against current reality** — tvo-skyline's harness is built and proven (phase = UI-work optimization, frontier ruled out), and the old item 2 "Coding harness (NEW project) — bootstrap this first" became "inception — built the harness; next is generalizing it".
- **iOS/Apple skills re-gated from a date to a dependency.** The "Priority — review within June 2026 (iOS app prep — Mum stories)" window lapsed unworked — the app's 30-min brain-dump is still owed, so there is nothing to profile. Re-scoped in three places: the `local/skill-watchlist.md` heading (now "Gated on the Mum-stories app starting"), the `CLAUDE.local.md` back-burner callout (the time-sensitive marker removed), and the `CLAUDE.md` Project Status checkbox. **Decision recorded:** dependency gates beat date gates for work that can't start until a prerequisite lands — a re-dated window would just lapse again.
- **Model reconcile closed.** `.claude/settings.local.json` now pins `"model": "opus[1m]"` (was Opus 4.8 at last session's close), so the reconcile open item was factually obsolete. Narrowed in the brief to the one live question — keep the project-level Opus pin, or delete the line so Sage follows Vince's global `/model` default. The "First: model reconcile" step was removed from the next-session pickup list.
- **Vault entry `2026-06-10-claude-fable-5-capability-profile.md` un-stuck from RESTRICTED.** Its status block still claimed Fable was withdrawn, which stopped being true on 2026-07-05. Header rewritten to: access restored, restriction episode historical, **dated free-window material expired** — capability content (benchmarks, 3× file-based memory, safeguard auto-fallback) still stands. Section heading "The time-sensitive bit (deadline)" → "Pricing / access history (EXPIRED — kept as record)"; the front-load-before-June-22 bullet struck through with the expiry noted. `vault/INDEX.md` row retitled and its status moved from *reference — restricted* to *reference*.

### Decisions Vince articulated this session (durable)

- **Housekeeping before project work** after a long gap — clear stale state first so the queue can be trusted.

### Memories updated

- `project_fable5_pulled_opus48_standard` — reconcile closed 2026-09-18; description rewritten; body records the `opus[1m]` pin and that all date-bound Fable framing has been pruned. MEMORY.md index line updated to match.

### Deliberately NOT touched

- **`.claude/settings.local.json` allow-list cruft** (one-off paths from the completed Slack MCP audit, one-off `gh api` calls from the June AgentMail review). Expired in spirit, but harmless — removing them only buys future permission prompts. Left in place by Sage's judgment; flagged to Vince.
- **The `.atlas/` overview** — refreshed at session-end per the standing rule, not as part of this pass.

## What got done — 2026-07-05 session

### Thread A: Inception's coding harness proven out on tvo-skyline (Vince's report)

- Vince opened with: he "proved out the inception-project coding harness on tvo-skyline," and clarified at session close the direction of the work: **inception built the harness; it was then applied to tvo-skyline to test it**. (Sage initially recorded this backwards — as a hand-built tvo-skyline harness awaiting inception's extraction — and Vince corrected it.) So inception is **unstalled and producing**: it delivered the harness, and tvo-skyline is the validated worked example. What remains for inception is generalizing that validated instance into the reusable scaffolding deliverable. Vince's stated next focus on tvo-skyline itself: **optimizing the UI work**. Memory [[project-inception-stalled-decompose-via-tvo-skyline]] updated to the proven/unstalled state.

### Thread B: frontier evaluated for UI work — ruled out

- Repo: `apoorvjain25/frontier` (MIT, prompt-files-only Claude Code plugin). Not a UI tool — a **quality-loop skill**: write a concrete rubric first, generate 3–5 independent candidates for creative work, run fresh-eyes judge passes until two consecutive clean sweeps. 21 "craft standards" incl. Design. Modes: `full` (~5–9× tokens), `quick` (~1.5–2×), `gate` (judge-only).
- Design craft file read in full: strong, concrete anti-AI-slop rubric (4/8px rhythm, modular type ramps, contrast minimums, 44px touch targets, mandatory empty/loading/error states, 360/768/1440px screenshot verification, "could this appear unchanged on any other product's site?" kill test) — but landing-page-shaped. Weak fit for tvo-skyline, where the host dashboard (Skyline) dictates the design system; only the UX-mechanics half transfers.
- Sage's assessment: useful only as an on-demand gate for greenfield UI; design knowledge overlaps built-in `frontend-design` + `dataviz`; `full`-mode cost profile conflicts with work-account frugality. The distinctive part is the procedure ("quality is a procedure, not a property"), same shape as adversarial-verify.
- **Vince passed on it entirely** — no install, no watchlist entry, no vault entry, no checklist crib. Recorded here as a **ruled-out path** so it isn't re-evaluated.

### Thread C: First real AgentMail triage — Homecrew + AgentNews #15

- Vince asked whether email was set up (yes — AgentMail, 2026-06-02), then had Sage check the inbox: two unread threads, both his early-June forwards.
- **Homecrew** (AI Tinkerers deep dive by Steve Krenzel of Logic, 2026-06-03): open-source **package manager for agent skills**. A "tap" is a plain git repo of skills — no registry, no publish step; `crew install` syncs a skill into every agent on the machine (Claude Code, Codex, Cursor, gemini-cli, Factory); team updates propagate PR → main → `crew update`. Directly relevant: the shared work skills repo is already tap-shaped — candidate answer to the open WORKFLOW-skill-mechanics question and input to the enterprise-packaging research. Logged to `local/skill-watchlist.md` (new **Ecosystem tooling** section) + pointer in `CLAUDE.local.md` under that open question. Exact repo URL still to verify (newsletter links were tracking redirects). **Trust pass required** before any team recommendation.
- **AgentNews #15** (AgentMail's own newsletter, 2026-06-04; Vince had forwarded it to Robin, then Sage): six-point agents-as-coworkers operational checklist (per-agent identity; job-scoped, short-lived permissions; append-only audit trail; escalation rules as *testable config*; offboarding as a default; a comms channel). Distilled to `inbox/20260604-agentnews15-ai-employees-checklist.md` (gitignored), marked background — rhymes with MCP-hardening/PAT-scope patterns; the genuinely new angle is escalation-as-testable-config.
- Both threads archived via label PATCH (`unread` removed, `archived` added — archive-over-delete per Robin's inherited gotchas). First live triage ran friction-free on raw curl, supporting the deferred-MCP/Skill decision.

### Thread D: Model observation

- This session ran on **Fable 5** — access evidently restored (Vince set it via `/model`; saved as his default for *new* sessions). This project's `.claude/settings.local.json` still pins Opus 4.8 (1M), which applies at restart — reconcile flagged as the first item next session. Memory [[project-fable5-pulled-opus48-standard]] updated: the unavailability is over as of 2026-07-05; the "'needs Fable' is not a valid blocker" guard retained.

### Decisions Vince articulated this session (durable)

- **Pass on frontier** — no adoption in any form.
- **Leave the AgentNews inbox note gitignored** — no commit; it earns a vault entry later or not at all.

### Memories updated

- `project_fable5_pulled_opus48_standard` — Fable 5 access restored 2026-07-05; guard retained.
- `project_inception_stalled_decompose_via_tvo_skyline` — inception built the harness, proven on tvo-skyline; unstalled.

### Commits

- One session-close commit (CLAUDE.md brief + this file). The content changes themselves (watchlist, CLAUDE.local.md, inbox note) live in gitignored paths and are deliberately uncommitted.

## What got done — 2026-06-26 session

### Thread A: Re-investigated a work repo's coding-subagent + MEMORY.md mechanism

- Trigger: Vince's weekly AI call — one work dev team (a Go/Kubernetes-controller repo) is ahead of the others, showing on-demand routing for MEMORY.md driven by their *coding subagents*. Vince asked how the subagents are kicked off.
- Findings (the full setup is on an unmerged feature branch — prototype-stage, one engineer's experiment; the default branch still runs a monolithic CLAUDE.md + a single skill):
  - **Subagent kickoff = standard Agent/Task tool with `subagent_type`.** No slash command, no orchestration script. The agent's `description` + `<example>` blocks ARE the delegate-or-not logic the main loop reads (multi-file domain work → subagent; small edit → main loop + skill).
  - **MEMORY.md loading is prompt/frontmatter-driven, NOT hook-injected.** `memory: project` frontmatter binds a project-scoped agent-memory dir; the agent's own prompt declares MEMORY.md as an *index* of one-line pointers, read on demand ("when relevant / user references prior work / explicit recall"), with a mandatory staleness re-check before acting on recall. Structurally identical to Sage's own on-demand routing, one level down.
  - Companion **runtime/conventions skill** holds the standards (the subagent delegates to it; main loop uses it as the lite path). A **`karpathy-guidelines` guardrail skill** is vendored in. Four `code-simplifier` **hooks** implement an exit-2-on-Stop "did you simplify after editing?" gate.

### Thread B: Researched the Karpathy skills

- Source: community repo `andrej-karpathy-skills` (Forrest Chang; ~177k stars) — NOT authored by Karpathy, distilled from his viral post on LLM coding failure modes.
- Despite the plural, it's **one skill = four principles**: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution. ~65 lines, pure behavioral guardrail (no new capability). Most additive piece for Vince: the "Think Before Coding" clause.

### Thread C: Distilled both into deliverables

- **Vault entry** `vault/2026-06-17-in-repo-coding-harness.md` (seed, single-source) — the five-component in-repo coding harness, scrubbed of the work-repo identity. INDEX row added.
- **Inception scaffolding spec** `output/20260617-coding-harness-scaffolding-spec.md` (gitignored) — implementation-grade, parameterizable, with the load-bearing caveat: ship the conventions skill as a *skeleton*, not fully-formed (pre-writing it = anticipated reuse). For Vince's Inception project to productize into reusable bootstrap scaffolding.

### Thread D: Created the project's Atlas overview

- Ran `/atlas-overview`. Pre-flight clean (no existing overview; remote matches; commits present; CLAUDE.md present). Created `.atlas/overview.md` (gitignored — always internal) and added `.atlas/` to `.gitignore`. Created `~/.atlas-host` = `mac-m4-pro` (Vince corrected from the derived `work-m4-pro`). Identity values (slug `ice-age-agent`, visibility `internal`, role_fit `ai-lead`) confirmed with Vince. Resolved all [NEEDS] markers. At session-end, refreshed it lean per Vince's instruction: **deliverables + key todos only, no minutia.**

### Thread E: Shared work skills repo confirmed LIVE

- The shared work skills repo is live with three skills: `access-openstack`, `handoff`, and `share-skill-with-team` (a meta-skill for creating team skills). `handoff` and `access-openstack` trace to Sage's vault work. Phase 1 of the skill-sharing initiative is realized. Repo name + remaining upload candidates kept in CLAUDE.local.md (employer-identifying); CLAUDE.md updated generically.

### Thread F: Fable 5 pulled → standardized on Opus 4.8

- Vince reported **Fable 5 was withdrawn at US-Government direction** — **restricted, not retired** (his framing: "restricted for now until Anthropic can secure it better"). He reverted ALL projects (personal + work) to **Opus 4.8 (1M context)** and removed the `claude-fable-5` pin from this project's `.claude/settings.local.json`. Sage now runs on Opus 4.8. This supersedes the entire "Fable 5 bootstrap window / free-through-June-22 / model-pin-revert" thread — those project priorities still stand, only the model changed. The `vault/2026-06-10-claude-fable-5-capability-profile.md` entry + INDEX row were marked **RESTRICTED/historical** this session (not obsolete).

### Thread G: Inception (scaffolding-builder) stalled — decomposition plan set

- Vince is applying `grill-with-docs` to his **inception** project (the one meant to productize the harness into reusable bootstrap scaffolding) but is struggling — feels it's "too advanced and needed Fable," unsure how to proceed.
- Sage's reframe + recommendation, which **Vince accepted**: the "needed Fable" read is a misdiagnosis (Opus 4.8 is top-tier; the blocker is under-decomposition of a meta/abstract project). Don't design the generator top-down. Build ONE concrete harness BY HAND — the `tvo-skyline` harness (confirmed top priority) — using the scaffolding spec as a checklist, then have inception **extract** the reusable scaffolding from that worked example. This is Vince's own extracted-not-anticipated principle applied to the project itself, and it reorders the dependency so inception follows tvo-skyline rather than blocking it.
- Vince will **share the harness framework with inception positioned exactly this way.** Banked as memory.

### Decisions Vince articulated this session (durable)

- **Atlas overview stays lean** — only useful deliverables and key todos; no minutia. (Drove the session-end trim from 7 todos to 3.)
- **Fable 5 is restricted (not retired); Opus 4.8 (1M) is the portfolio-wide standard.** "Needs Fable" is not a valid blocker. Banked as memory.
- **Inception decomposes from a concrete worked example, not top-down.** Build tvo-skyline's harness by hand first; inception extracts from it. Accepted by Vince. Banked as memory.
- Confidentiality split reaffirmed: work-org repo names (`trilioData/...`) and employer-named skills (`trilio-jira-...`) stay in gitignored files (CLAUDE.local.md, `.atlas/`); committed content (CLAUDE.md, vault, this file) uses generic phrasing.

### New memories banked this session

- `project_fable5_pulled_opus48_standard` (later reframed restricted-not-retired).
- `project_inception_stalled_decompose_via_tvo_skyline`.

### Commits pushed this session

- `1036b91` — vault: in-repo coding harness entry + INDEX; CLAUDE.md session-state (shared skills repo live).
- `3a69c01` — gitignore the `.atlas/` overview directory.
- Session-close refresh — CLAUDE.md + docs/session-state.md (Fable→restricted, inception plan, next-session pickup) + Fable vault/INDEX restricted marking.

### Inbox saved this session

None new.

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
