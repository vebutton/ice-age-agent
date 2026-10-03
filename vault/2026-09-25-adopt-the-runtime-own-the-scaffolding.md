# Adopt the runtime, own the scaffolding — "build your own harness" is three decisions, not one

**Pattern:** When an agent platform becomes a funded ecosystem, "should I build my own harness?" stops being answerable, because *harness* names three separable layers with opposite answers:

1. **The runtime** — gateway, message bus, plugin system, model catalog, profile and credential isolation, update machinery. **Adopt. Never build.** This layer has enormous outside investment and no domain specificity; a solo effort cannot keep pace and gains nothing by matching it.
2. **Orchestration patterns** — how many agents relate, who delegates to whom, where durable state lives, what happens when a child dies. **Copy from practitioners.** This is configuration and operating discipline, not code, so it transfers between runtimes and costs nothing to adopt.
3. **Domain scaffolding** — the subagent, conventions, project memory, guardrails and enforcement hooks wired to *your* repositories and *your* standards. **Build this, and only this.** Nobody else will ever build it, because it encodes knowledge that exists only in your work.

The failure mode is answering all three with one word. "Build my own harness" usually means (3) and gets heard as (1); "just use the community tool" usually means (1) and gets heard as denying (3).

## Why it matters

- **The runtime race is unwinnable and not worth entering.** Observed 2026-09-25: the Hermes Agent project shipped **2,200 merged PRs across three releases in ten days** (v0.21.3 → v0.21.5), with a separate report of 126 PRs merged in a single 24-hour window. OpenClaw passed 250k GitHub stars within ~60 days of its rename. Whatever a single practitioner builds at this layer is obsolete on arrival.
- **But adopting a runtime is not the same as being served by it.** An independent survey of open-source agent harnesses (2026-09-18) found that **none of them documents multi-agent fleet coordination**, and that all the coding-shaped ones assume session freshness rather than always-on operation. The funded layer is layer 1. Layers 2 and 3 are where the user is still on their own.
- **Layer 3 is where measured performance actually comes from.** A study of 254 SWE-bench submissions found differences between top coding agents frequently not statistically significant, and that **changing an agent's scaffolding affects performance more than swapping between top-ranked models**. The same direction appears in a separate vendor eval where decomposition lifted a cheap model by 28–42 points ([[2026-09-22-decompose-decisions-into-typed-questions]]). Neither result says "build a runtime". Both say the wiring around the model is what pays.
- **Velocity is a reason to adopt *and* a reason to pin.** The same release note documenting 2,200 PRs advises readers *not* to update unless they need the new model catalog: the repair tool "has been known to silently drop config fields it doesn't recognize", plugin tools began propagating into every session at 500–1,000 tokens of schema each, and unauthorized-message handling changed behaviour. Community migration stories in the same period were driven by **update breakage**, not by capability gaps. Depending on a fast-moving upstream is correct for a runtime and requires the version-pinning discipline from [[2026-05-20-third-party-mcp-wireup-pattern]].
- **Layer 2 is available for free and is usually skipped.** Working fleet designs are published by practitioners rather than maintainers — a control-plane agent with domain managers and specialists on a shared message bus; **durable work living on a board rather than in a subagent's context, on the assumption that subagents will die**; stall monitors that interrupt an idle child after a fixed timeout and emit an explicit *stalled* event so the parent hears an outcome instead of silence. None of that requires building anything.

## What was ruled out, and why

Recording the negative space deliberately ([[2026-06-10-negative-space-documentation]]):

- **Building an agent runtime** — ruled out on ecosystem velocity, above. Not on capability, and not permanently-in-principle; if the ecosystem consolidates or a runtime becomes hostile to the use case, this reopens.
- **Building a fleet orchestration layer from scratch** — ruled out because working designs are already published; the cost is reading, not engineering.
- **Migrating between runtimes to acquire a feature** — ruled out as the default move. A practitioner who ran six months on one runtime, a month on the other, then **harvested the three features they wanted (memory, self-learning, self-skill-development) back into the original and deleted the second** demonstrates the cheaper path. Migration costs the operational knowledge already accumulated; feature-harvesting does not.

## How to apply

1. **Name the layer before answering the question.** "Should I build this?" is unanswerable until you say whether *this* is a runtime, a pattern, or domain scaffolding.
2. **Pin the runtime and read release notes.** Treat a fast-moving upstream the way third-party MCP servers are treated: explicit version, deliberate upgrades, back up config first, never blind-update something load-bearing.
3. **Harvest features, don't migrate platforms.** When a competing runtime has something you want, take the idea into the one whose failure modes you already know.
4. **Spend your build budget entirely on layer 3.** Domain subagent, conventions skill, on-demand project memory, guardrail skill, enforcement hooks — the five-piece shape in [[2026-06-17-in-repo-coding-harness]]. This is the only layer where effort compounds rather than being overtaken.
5. **Steal layer 2 from people operating at scale**, not from vendor documentation. The maintainers document the runtime; the operators document the fleet.

## Where this came from

Articulated 2026-09-25 when Vince challenged the standing "own the harness" goal directly: millions of dollars of tokens are being spent improving OpenClaw, Hermes and other community harnesses, so building one makes no sense. The challenge was correct at layer 1 and the goal was restated rather than defended.

The distinction is the same shape as [[project-mcp-build-vs-buy-stance]] one level up — build what is specific to you, buy what is generic — applied to agent runtimes instead of MCP servers. That earlier stance is the second instance of this reasoning, which is why this entry is tiered as observed rather than single-source.

**Worked example:** an in-repo coding harness built for one practitioner's projects and validated on a second ([[2026-06-17-in-repo-coding-harness]]) is layer 3 and remains the right deliverable. The ambition to generalise it into a *runtime* is the part that was dropped.

**Open question:** layer 2 is currently sourced from a handful of community posts by people running fleets of ~20 agents. Those designs are unvalidated here. The first real fleet run is what would move this entry's layer-2 claims from *reported* to *observed*.

**Status:** seed (multi-source observed — one practitioner's portfolio; layer-2 claims still second-hand)

Related: [[2026-06-17-in-repo-coding-harness]] (the layer-3 deliverable). [[project-mcp-build-vs-buy-stance]] (same build-vs-buy reasoning at the MCP layer). [[2026-05-20-third-party-mcp-wireup-pattern]] (how to depend on fast-moving third-party code). [[2026-09-22-decompose-decisions-into-typed-questions]] (scaffolding over model choice, measured). [[2026-06-10-negative-space-documentation]] (why the ruled-out section above exists).
