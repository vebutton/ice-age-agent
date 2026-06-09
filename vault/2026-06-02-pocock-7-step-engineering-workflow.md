# Pocock's 7-step engineering workflow — a lightweight alternative to spec-first

**Pattern:** A 7-step pipeline for engineering work with Claude Code (or any coding agent), explicitly pitched against "Specs Over Code" / BMAD-style upfront-spec approaches. The author calls those "waterfall by another name." The workflow treats software engineering as people problems first (research, understanding, planning, coordination, iteration, mistake catching, feedback loops) and uses the agent as a collaborator inside that, not as a code-generator fed a finished spec.

Each step is a separable phase, several are explicitly optional, and the steps connect — *"the magic isn't in any single step, it's in how they all connect."* Same shape claimed to scale from bug fixes to greenfield apps.

**Source:** Matt Pocock, *"HOW do you engineer with AI?"* (newsletter for his "AI Coding for Real Engineers" cohort, June 2026). Local copy: `collateral/HOW do you engineer with AI_.eml`. Companion skills repo: <https://github.com/mattpocock/skills> (`mattpocock/skills`, referenced in CLAUDE.md). 5 of the 7 steps are directly covered by skills in that repo; Step 6 (execution) is the workflow's residual *pattern* value, not a packaged skill.

---

## The 7 steps

| # | Step | Required? | Pocock's skill | Job to be done |
|---|---|---|---|---|
| 1 | **Idea** | Yes | `grill-with-docs` | Asks YOU questions to drag the idea out of your head. Same shape whether new app, new feature, or refactor. |
| 2 | **Research** | Optional | — | Caches external-API / unfamiliar-territory research *into the repo* so the agent doesn't re-explore every session. Use when integrating something new. |
| 3 | **Prototype** | Optional | `prototype` | Quickly prototype the design/UX in code before committing. "Impose your taste on the outcome." Clarifies decisions for the remaining steps. |
| 4 | **PRD** | Yes | `to-prd` | Translate the shared understanding into a Product Requirements Doc — the "shared north star" for what users see and how it behaves. Skill walks through every decision point. (Email calls it `/write-a-prd`; repo name is `to-prd`.) |
| 5 | **Kanban** | Yes | `to-issues` | Decompose the PRD into discrete, *parallelizable* tickets with explicit blocking relationships. (Email calls it `/prd-to-issues`; repo name is `to-issues`.) |
| 6 | **Execution** | Yes | — *(pattern only)* | Spin up a coding agent (Pocock uses Ralph loops) to churn through the Kanban. Walk away if sandbox is set up properly. |
| 7 | **QA** | Yes | — *(adjacent: `tdd`)* | Separate Claude instance creates a QA plan, reviews the work, files new tickets, loops back to execution. Repeat until polished. Repo's `tdd` skill is adjacent but not the same shape. |

**Install entrypoint:** `skills/engineering/setup-matt-pocock-skills` in the repo is Pocock's own onboarding skill — run it to install the others. Verified live 2026-06-02 against `github.com/mattpocock/skills` (default branch `main`).

---

## Why it matters

- **Anti-spec-first stance has teeth.** Pocock tried full upfront specs ("Specs Over Code") and it didn't work for him. The 7-step keeps the *artifact* idea (research cached in repo, PRD as north star, Kanban as plan) but rejects the *philosophy* that you can mechanically transcribe a complete design into code. See also: caveat about Spec-Kit philosophical framing in `vault/2026-05-24-agent-mediated-structured-writeup.md` and `local/skill-watchlist.md`.
- **Optional steps make it scale down.** Research, Prototype, and even the Kanban-driven execution can be skipped for small work. The 4-step skeleton — *Idea → PRD → Execution → QA* — is the irreducible core; everything else is leverage for harder problems.
- **Pairs naturally with a "Research, Plan, Code, Test" mental model.** Research = Step 2, Plan = Step 4 (PRD), Code = Step 6 (Execution), Test = Step 7 (QA). Pocock's extras (Idea/Prototype/Kanban) are the parts worth keeping for a *real* project versus a quick experiment.
- **Each phase produces a durable artifact** the next phase consumes. Research caches in the repo, PRD becomes the north star, Kanban becomes the parallelizable work list. Counter-pattern to "all the context lives in the chat session."

---

## When this fits

- New project where the inputs are fuzzy — Step 1 (`grill-with-docs`) is the differentiator. If the idea isn't crisp, jumping to a PRD wastes a PRD.
- Multi-feature work that benefits from parallelization (Step 5).
- Solo or small-team work where one human is the bottleneck on direction-setting but the agent can absorb execution load.

## When this doesn't fit

- The task is one-off and small. The 4-step skeleton may still apply, but the full 7-step has overhead that won't pay back.
- The bottleneck is *discovery*, not *implementation* — agents accelerate execution, not research that requires real-world investigation, customer conversations, or judgment the author hasn't formed yet.
- The work is heavily creative / exploratory and a PRD would over-commit too early.

---

## Status & open questions

- **Tier:** *observed (single-source)* per [[feedback-pattern-domain-specificity]]. Only Pocock as inventor so far. Do NOT elevate to universal until at least one independent author lands the same shape.
- **First executed worked example (2026-06-05/06):** Step 1 (`grill-with-docs`) applied to a personal-track project. Vince's read: "worked incredibly well." First positive field-test of the methodology in Vince's own portfolio. Not an independent inventor — adoption with positive outcome. Next planned application: a second personal-track project. Open question of "how 5/7 mapped skills behave against real project" now has its first data point from Step 1; later steps still untested by Vince.
- **Open: how the 5/7 mapped skills behave against Vince's actual project.** Worth capturing what Steps 1, 4, 5 produce in practice — first-draft quality, how much editing is needed, where each skill leaks domain assumptions.
- **Open: residual execution-pattern value (Step 6).** Pocock uses Ralph loops; not packaged as a skill. Worth a separate vault entry once Vince has tried a loop on real work.
- **Open: relationship to `vault/2026-05-23-handoff-pattern-...`.** Both Pocock-inflected. Handoff is *within* a long-running project; 7-step is *the shape of* a project. May compose: a handoff doc at the end of Step 6 hands execution state to the next session.

---

## Related

- `vault/2026-05-23-handoff-pattern-structured-context-transfer.md` — handoff pattern, Pocock as inventor #2.
- `vault/2026-05-24-agent-mediated-structured-writeup.md` — calls out Spec-Kit's waterfall-rebranded philosophy, same anti-stance Pocock takes.
- `vault/2026-05-20-claude-md-stays-lean-lazy-loading.md` — same source author for the "don't over-load context" instinct.
- `collateral/HOW do you engineer with AI_.eml` — primary source.
- `output/20260524-ai-notebook-synthesis.md` note 6 — Vince's pre-existing intent to use `grill-with-doc` + `to-prd` on a personal-track project.
