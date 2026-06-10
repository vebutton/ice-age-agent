# Negative-space documentation — record what was ruled out, not just what was done

**Pattern:** Knowledge artifacts leak their highest-value content when they record only the positive space (what was done, what exists, how it works) and omit the negative space — what was tried and abandoned, considered and rejected, hypothesized and disproved, or deliberately *not* built. Document the negative space explicitly. "X doesn't work because Y" is often more reusable than "we did Z": the positive content tells you what happened; the negative space tells you what *not* to try next time you face the same problem shape.

## Why this matters more in the agent era

Two compounding reasons:

1. **Fresh contexts re-explore dead ends at full cost.** Every new session, sub-agent, or teammate that lacks the ruled-out list pays the full exploration price again. In a workflow where sessions are never re-entered (Vince's model) and agents spin up cold constantly, undocumented dead ends are a recurring tax, not a one-time loss.
2. **Agents fill gaps with plausible assumptions.** An LLM agent reading a doc that describes what exists will happily infer the existence of the standard adjacent machinery — the conventional endpoint, the usual validation layer, the expected config knob. A doc that says "there is no X; don't look for it / don't add it without reading Y" closes off the wrong turn before it's taken. Positive-only docs are hallucination bait.

## Where the negative space lives, by artifact type

- **Handoff docs:** "What's been ruled out" — hypotheses already disproved, with the evidence. The handoff pattern names this its *highest-value section* (see Related).
- **Debugging notes:** what doesn't reproduce, what was swapped without effect, what was confirmed not to be the cause.
- **Post-mortems:** contributing factors considered and rejected, and why.
- **ADRs:** the alternatives considered and the reason each was rejected outlive the final choice — the choice gets revisited when context changes; the rejection reasons say whether the revisit is warranted.
- **Design docs:** the constraints that ruled out simpler approaches (otherwise every reviewer re-proposes them).
- **Onboarding docs:** "this is not the way to do it" sections that prevent the new-hire failure mode.
- **Standing context docs for agents (CLAUDE.md companions):** what the subsystem deliberately does *not* handle, so an agent doesn't assume the standard-shaped machinery exists.
- **Session archaeology:** ruled-out paths as a first-class section, so the next session doesn't relitigate them.

## Worked examples

- **Handoff "What's been ruled out" section** (2026-05-22, internal infra/ops project; independently named by Matt Pocock's handoff version) — flagged as the highest-value section of the handoff doc, above what-was-done and open hypotheses. This entry is the extraction of that insight, anticipated in the handoff entry itself ("may extract later").
- **Standing context doc in a primary K8s-operator work repo** (`webhooks.md`, observed 2026-05-26) — flagged during the cross-project review as this pattern's first sighting in a *standing* doc rather than a transition artifact: part of the file's value to an agent is bounding its domain, including what the webhook layer doesn't cover. Specifics stay in the (access-gated) repo; the structural observation is what's portable.
- **Sage's own session archaeology** (`docs/session-state.md` schema) — "ruled-out paths" is a first-class section, applied continuously rather than at transitions. E.g., recording a rejected split-into-two-always-loaded-files proposal (2026-06-09) prevents a future session from re-proposing it.

## Counter-signal (why this isn't universal)

Of three independent inventors of the handoff pattern, only two (Vince, Pocock) named negative space as headline value; Steinberger's version omits it. The mechanic (handoffs) converged three ways; the negative-space emphasis didn't. Treat this as a distinctive practice worth advocating, not a universal everyone already does — which is also what makes it worth writing down.

## How to apply

When writing or reviewing any of the artifact types above, run one check: **is the negative space captured?** What was considered and rejected, what didn't work, what deliberately doesn't exist? If absent, the doc is leaking its most durable content. Cheap retrofit: a single "Ruled out / does not exist" section is usually enough; it doesn't need to be long, it needs to be *findable*.

Boundary: don't inventory every absence — the negative space worth recording is the part someone (human or agent) would otherwise *plausibly assume or re-attempt*. "We don't use Kafka" matters only if the problem shape makes Kafka the obvious guess.

## Status

*seed (multi-source observed)* — two independent branches:
- Vince/Pocock handoff branch (named explicitly, 2 of 3 handoff inventors).
- A work-repo author (independent practitioner) applying the shape to a standing agent-context doc.

Plus Sage's own continuous application in session archaeology (project-shape diversity, not an independent inventor). Not yet *confirmed* — the Steinberger omission is exactly the kind of non-convergence [[feedback-pattern-domain-specificity]] warns about elevating past.

## Related

- `vault/2026-05-23-handoff-pattern-structured-context-transfer.md` — origin artifact; this entry extracts and generalizes its "negative-space insight" block.
- `vault/2026-06-09-on-demand-context-routing.md` — sibling discipline on the *structure* side (where content loads); this entry is the *content* side (what must not be omitted). The archaeology layer in that pattern is where ruled-out paths live.
- [[feedback-pattern-domain-specificity]] — status discipline applied above.
