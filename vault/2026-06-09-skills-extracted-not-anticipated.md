# Skills are extracted from demonstrated reuse, not anticipated reuse

**Pattern:** Build the capability concretely *in the project that needs it*. Recognize the skill-shape *afterwards*, only when a second instance of the same need arrives. The second instance can be **cross-project** (another project needs the same capability) OR **same-project temporal** (the same project keeps re-paying the same lesson across sessions because `CLAUDE.md`/memory fail to surface it reliably). In both cases the skill is extracted at instance #2, never before. Anticipating reuse produces skills nobody uses and abstractions that don't fit when the second use case actually arrives.

## The flow — cross-project axis

1. **Build the capability in service of a real need.** No skill intent at this stage. Solve the project's problem with whatever shape is right for that project.
2. **Wait for the reuse signal.** A second project encounters the same need. This is the trigger — without it, skill creation is speculative.
3. **Extract the skill at that point.** The second project becomes the validation environment (and often the one you author the skill *in*, because it's the project where the abstraction needs to fit next).
4. **Publish only after the second-project use confirms the skill shape works.** Broader reuse is now defensible because you have two concrete data points, not one.

## The flow — same-project axis

Articulated by Vince 2026-06-09: not all reuse signal is cross-project. Sometimes the same project re-pays the same lesson many times because the always-on persistence layers (memory line, `CLAUDE.md`) fail to surface it reliably.

1. **Notice the lesson keeps re-paying.** A specific gotcha or workflow keeps having to be re-taught — typically because context overflow buried it, restart dropped it, or the memory/`CLAUDE.md` directive isn't winning at the moment of need.
2. **Recognize that the persistence layer is wrong.** Always-on layers (memory, `CLAUDE.md`) compete for the same context slot every turn. A skill loads only when its trigger fires, so it can carry richer workflow content without paying the always-on cost.
3. **Extract into a skill.** The same project IS the validation environment — the next session that doesn't re-pay the lesson is the test. Skills shine here precisely because they sidestep the failure modes (overflow, restart) that broke the always-on layer.

The cross-project flow says "wait for spatial reuse signal." The same-project flow says "wait for temporal reuse signal." Both are forms of demonstrated reuse — just on different axes.

## Why this matters

- **Skills built without reuse demand drift.** They encode a guess at what's portable. When a real second use case arrives, the guess is usually wrong — the abstraction is over- or under-specified for the actual need.
- **Concrete-first beats abstract-first.** The capability in Project A is fully informed by Project A's constraints. Trying to abstract during Project A means designing for hypothetical Project B without knowing what Project B will care about.
- **It respects the rule of three.** Two concrete uses justify a skill; one does not. Three would be even better.
- **It saves work.** Skills that don't get reused are pure cost — time to build, document, package, and maintain them is unrecovered. Resist "this seems reusable" until it actually is.

## Anti-pattern

*"I just built this thing, let me skill-ify it before moving on."* The skill-shape is speculative at this point. Park the capability, note it, and revisit only when a second project triggers the need.

A subtler anti-pattern: building skills *because skills are a thing now*. Skill infrastructure (skill-creator, marketplaces, plugin systems) creates incentive to produce skills as evidence of activity. Resist this — the value of a skill is reuse, not authorship. A team with five well-used skills is in a better position than one with fifty unused ones.

## Worked examples (Vince's own portfolio)

**Cross-project axis:**

- **`handoff` skill.** Built informally across multiple projects (an infra/ops project, then independently surfaced by Pocock and Steinberger). Skill captured only after the pattern emerged through repetition. Confirmed as a multi-source pattern in `vault/2026-05-23-handoff-pattern-structured-context-transfer.md`.
- **`access-openstack` skill (2026-06).** Capability built in service of one project that needed OpenStack access. When a second project needed the same capability, recognition triggered: this is reusable. Skill extracted, validated in the second project where the need was active, then published to the team-skills repo. The second project was both the trigger AND the validation environment.

**Same-project axis:**

- *No worked example yet.* The previously named candidate (`git-commit-from-file`, per `vault/2026-05-18-git-commit-skill-needed.md`) was **deliberately NOT skill-ified** on 2026-06-10 — Vince's call: a single tweak, however valuable, lacks the capability mass to carry a skill; the rule went to global `~/.claude/CLAUDE.md` instead, and the skill waits for a fuller `git-ops` bundle. That decision is itself a demonstration of this entry's anti-pattern discipline (declining to skill-ify for the sake of skill-ifying), and it surfaces a **placement spectrum** this entry should name: *one-line universal rule → global CLAUDE.md; project-specific rule → project CLAUDE.md; multi-step workflow with gotchas → skill.* Demonstrated reuse is necessary but not sufficient — the artifact also needs enough body to justify the trigger machinery. First same-project worked example still pending; the future `git-ops` bundle remains a candidate.

## When to violate this

- **Skills that wrap a single critical workflow built from earned scars across multiple prior instances.** E.g., the `setup-github-pat` skill (2026-06-05) was authored at the point of first formal capture, but the underlying earned scars came from at least two prior real PAT setups. The "two real uses" threshold was met externally to the skill; the skill is encoding the gotchas, not anticipating reuse.
- **External patterns with strong cross-source corroboration.** Pocock's 7-step engineering workflow (`vault/2026-06-02-pocock-7-step-engineering-workflow.md`) was adopted before personal reuse demand because the cross-author corroboration (Pocock's published work + companion skills repo) served as the demand signal externally.
- **Research/lab work explicitly scoped as such.** "I'm evaluating what skills look like" is legitimate as lab work — but then don't ship those samples to a shared repo as if they were earned skills.

## Status

*seed (single-source)* — articulated by Vince 2026-06-09 (cross-project axis) and extended same day with the same-project axis. Two cross-project worked examples from his portfolio; same-project axis is still pre-worked-example (commit-from-file candidate identified). Per [[feedback-pattern-domain-specificity]], do not elevate to universal until at least one other practitioner lands the same shape independently.

## Related

- `vault/2026-05-23-handoff-pattern-structured-context-transfer.md` — worked example 1.
- `vault/2026-06-02-pocock-7-step-engineering-workflow.md` — adjacent; cross-author corroboration as substitute for personal reuse demand.
- `vault/2026-05-24-agent-mediated-structured-writeup.md` — adjacent anti-anticipation stance ("specs-over-code is waterfall by another name").
- [[feedback-pattern-domain-specificity]] — same discipline applied to vault patterns rather than skills.
- [[project-sage-is-skill-factory]] — Sage's outputs are knowledge-shaped; same anti-anticipation principle (vault entries earned from real cross-project evidence, not anticipated portability).
