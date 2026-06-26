# In-repo coding harness — subagent + runtime skill + on-demand memory + guardrail + enforcement hooks

**Pattern:** Carry the machinery that makes an AI coding agent behave like a disciplined
senior engineer *inside the repo*, not in the operator's head or global config. Five
composable pieces — a domain coding subagent, a companion conventions/runtime skill,
project-scoped agent memory loaded on demand, a behavioral-guardrail skill, and
enforcement hooks — each useful alone, strongest together. Lay it down *before* the first
line of code; it's how a greenfield repo starts disciplined instead of accreting
discipline after the mess.

This is the in-repo cousin of the project-level [[2026-06-09-on-demand-context-routing]]:
the same lean-index-plus-lazy-detail discipline, applied one level down to the *coding*
loop (subagent prompts, conventions, coding memory) rather than project context.

## The shape

Five components. The load-bearing design rule across all of them: **each layer stays
thin and delegates depth to the next — nothing is restated twice.** Subagent prompt →
delegates standards to the skill → delegates depth to agent-memory.

1. **Domain coding subagent** — `.claude/agents/<domain>-engineer.md`. Markdown +
   frontmatter (`name`, `description` with `<example>` blocks, `model`, `memory:
   project`). Launched by the **standard Agent/Task tool with `subagent_type`** — no
   slash command, no orchestration script. The "when to delegate vs. handle inline"
   logic lives entirely in the `description`/`<example>` blocks: the main loop reads them
   and decides (multi-file domain work → subagent; small edit → main loop + skill). The
   prompt body stays thin — it names scope, points at the skill, and states the memory
   contract; it does **not** restate standards.

2. **Conventions / runtime skill** — `.claude/skills/<domain>-runtime/SKILL.md`. The
   procedural heavy-lifter: library-reuse search order, framework idioms, project "hard
   stops," a self-review checklist, a mandatory output format, and cross-links into
   agent-memory. The subagent delegates *all* standards here; the main loop invokes the
   same skill for small edits (the "lite path"). One source of truth for conventions.

3. **Agent memory, on-demand routed** — `.claude/agent-memory/<agent>/`. Project-scoped,
   committed, team-shared coding memory. **Loading is prompt/frontmatter-driven — no hook
   injects it.** The `memory: project` frontmatter binds the dir; the agent's own prompt
   declares the contract: one fact per file, and **`MEMORY.md` is an index, not memory**
   (one-line pointers, no frontmatter, under ~200 lines). Read trigger: only "when
   memories seem relevant, the user references prior work, or recall is explicitly
   requested." Plus a **mandatory staleness check before acting on recall** — verify a
   named path exists / grep a named symbol; if recall conflicts with the code, trust the
   code and update the memory.

4. **Behavioral-guardrail skill** — the community `karpathy-guidelines` skill (one skill,
   four principles: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven
   Execution). ~65 lines, MIT, distilled from Karpathy's post on LLM coding failure
   modes (*not* authored by Karpathy). Adds no capability — pure behavioral guardrail —
   but high-leverage on greenfield code where over-engineering risk peaks, and identical
   across every project.

5. **Enforcement hooks** — `.claude/settings.json` + `.claude/hooks/*.sh`. Deterministic
   gates the *harness* enforces, not the model's goodwill. Reusable shape: a per-prompt
   marker + Stop-gate. `UserPromptSubmit` resets markers; `PostToolUse[Edit|Write]`
   marks "domain file changed"; `PostToolUse[Task]` marks "gate satisfied"; `Stop`
   **exits 2 with a stderr instruction** if the file changed but the gate wasn't run —
   turning a soft reminder into a hard gate. The exit-2-on-Stop is the load-bearing
   trick.

## The one place NOT to copy literally

A scaffolding generator will want to ship a *complete* conventions skill so the project
"starts strong." Don't. A fat conventions skill is only earned *after* the idioms are
demonstrated in real code. A greenfield project has no idioms yet, so a pre-written one
is **anticipated reuse — the exact anti-pattern** from
[[2026-06-09-skills-extracted-not-anticipated]]. Ship the conventions skill as a thin
*skeleton* (reuse-order stub, empty checklist, output-format headers) and let the body
accrete. The guardrail skill (#4) is the opposite case — it encodes universal LLM
failure modes, not project idioms, so it ships complete.

What's safe fully-formed at bootstrap: the agent-memory mechanism, `karpathy-guidelines`,
the hook template (disabled until a gate exists), the subagent template. What must grow:
the conventions-skill body, the memory contents.

## Bootstrapping order (lay the harness before the code)

1. Lean CLAUDE.md with on-demand routing → pointers to `docs/` (requirements,
   architecture). 2. `karpathy-guidelines` from day one. 3. Empty `MEMORY.md` index,
   committed. 4. Domain subagent(s) with delegate/handle-inline examples — hardest domain
   first. 5. Thin conventions-skill skeleton per domain. 6. Enforcement hook **last**,
   once there's a build/test loop to gate. (Existing projects: same order, but step 5 can
   seed from existing conventions and step 6 can go in immediately.)

## Worked example

One internal Go / Kubernetes-controller repo I reviewed (feature branch, prototype-stage
— one engineer's in-flight experiment, not merged to the default branch). On that branch:
a `<domain>-controller-engineer` subagent (`model: opus`, `memory: project`, delegate-vs-inline
examples in its description) → delegates standards to a `<domain>-controller-runtime`
skill → which cross-links project-scoped agent-memory using the exact MEMORY.md-index
on-demand shape above. Plus the `karpathy-guidelines` guardrail skill vendored in, and a
four-hook "did you run the simplifier after editing?" Stop-gate. The default branch still
runs a monolithic CLAUDE.md + single skill — so this is bleeding-edge, not a blessed team
standard. The *structure* ports cleanly; the *contents* are stack-specific.

Second (intended) instantiation: a personal POC adding backup to an OpenStack dashboard
(Python apiserver + React/TS console + a workload-manager API), requirements done, coding
not started — the ideal moment to lay the harness. Domains: a Python backend engineer
first, a TS console engineer later; conventions skills shipped as skeletons; guardrail
day one; enforcement hook deferred until a pytest/lint loop exists. An existing
OpenStack-access skill bundling the workload-manager client plugs straight into the
backend subagent's integration path.

## When it fits

- A coding project (vs. a knowledge/research project) substantial enough to warrant
  isolated context windows and persistent coding memory.
- Multiple domains (backend/frontend/infra) that each have their own idioms.
- Greenfield or early-stage, where laying discipline up front beats retrofitting it.
- A team that benefits from versioned, shared coding memory and uniform review output.

## When it doesn't

- Tiny / throwaway scripts — the harness is overhead. The guardrail skill alone may be
  all that's warranted.
- A bounded-scope repo with one coherent context (same call as the monolithic-CLAUDE.md
  counter-example in [[2026-06-09-on-demand-context-routing]]).
- Before any code exists, don't over-build the conventions skill (see the "one place not
  to copy" section). The *skeleton* is fine; a fat body is premature.

## Status

*seed (single-source)* — one worked example, and it's on an unmerged feature branch
(prototype-stage), so even within that repo it's not load-bearing yet. The individual
components are better-evidenced (on-demand routing is multi-source observed; the guardrail
skill is widely adopted community work; subagent-via-Task delegation is the standard
harness mechanism), but the *composite harness as a deliberate pattern* rests on this one
instance plus Vince's intended build. Promote toward *confirmed* when a second
independent project assembles the same composite — Vince's own scaffolding build
(Inception) will be the second instance, which adds shape diversity but not an independent
inventor.

## Related

- [[2026-06-09-on-demand-context-routing]] — the project-level parent pattern; this is
  the same discipline applied to the coding loop. The MEMORY.md-index mechanism is
  identical, one level down.
- [[2026-06-09-skills-extracted-not-anticipated]] — governs the "ship the conventions
  skill as a skeleton, not fully-formed" rule. The single most important caveat here.
- [[2026-05-20-claude-md-stays-lean-lazy-loading]] — sibling anti-bloat discipline for
  the always-loaded layer.
- [[2026-06-02-pocock-7-step-engineering-workflow]] — the "grill the docs to lock
  understanding" front-end that feeds a freshly-laid harness its shared understanding.
- [[2026-06-10-claude-fable-5-capability-profile]] — the file-based-memory and
  long-horizon-coding strengths that make the on-demand-memory + subagent components pay
  off.
- [[feedback-pattern-domain-specificity]] — why this is logged *seed (single-source)*
  and not elevated despite being compelling.
