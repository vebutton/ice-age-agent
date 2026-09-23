# Decomposing a decision into narrow typed questions beats reaching for a better model

**Pattern:** When a task is a *decision* rather than a piece of writing — triage this alert, route this ticket, grade this trace, approve or hold this invoice — do not hand the whole problem to a model in one prompt. Break it into a set of **narrow, independently-answered questions with typed answers** (yes/no, pick-one-of-N, score on a scale), defer to plain code wherever code can decide, and assemble the final decision from the parts.

The size of the effect is the point. On identical tasks, with the identical model, decomposition moved Claude Haiku 4.5 from **17.1% to 58.8%** accuracy on security-alert triage and from **6.0% to 42.9%** on invoice processing — while *also* costing less and running faster, because each narrow question is a shorter prompt than the monolithic one.

## Why it matters

- **It is the cheapest accuracy you can buy.** The reflex when a model underperforms is to move up a tier — Haiku to Sonnet, Sonnet to Opus — which multiplies cost by 5–50x. Decomposition is free. On the published numbers, decomposed-Haiku got within ~7 points of one-shot-Opus on security triage at roughly 1/13 the cost.
- **The weaker the model, the bigger the gain.** Measured deltas from one-shot to decomposed: Haiku +28 to +42 points; Sonnet +9 to +10; Opus +3 to +12. This is the load-bearing asymmetry — **decomposition is how you make a cheap model viable**, and it is exactly the lever for routing cheap models at bulk work. It also means benchmarking a small model with one-shot prompts will systematically understate what it can do for you.
- **It converts an opaque judgement into an auditable one.** A monolithic answer is one verdict you either trust or don't. A decomposed answer is a set of sub-answers you can inspect, unit-test, cache, and selectively escalate. When the decision is wrong you can see *which sub-question* was wrong.
- **Cost and latency move the right way too.** This is unusual. Most accuracy interventions (more thinking, bigger model, self-consistency sampling) cost more. This one was cheaper and faster in every published pair, because the monolithic prompt does more redundant reasoning than the sum of the narrow ones.
- **It is the precondition for using a classifier model at all.** Typed-decision models (see the Jev note) only accept work in this shape. Doing the decomposition is what makes that class of model available to you later — and if you never adopt one, you keep the accuracy gain anyway. There is no wasted move.

## Where the evidence came from — and its limits

Source is TypeSafe AI's public eval for their Jev model (`evals.typesafe.ai`, read 2026-09-22), across four workflows: security-incident triage, agent-trace observability, invoice processing, customer service. Every competing model was run in two arms — **"prompt"** (one-shot) and **"workflow"** (decomposed into narrow typed questions, deferring to code where possible). The decomposition delta is the gap between those two arms.

Honest caveats, and they matter:

- **Vendor-published, not independently reproduced.** TypeSafe chose the tasks and authored both arms. No public dataset, no code repo. A vendor with an interest in the decomposed shape looking good authored the decomposed arm.
- **Mitigating the above:** this particular finding runs *against* the vendor's commercial interest. It shows a free technique on an existing cheap model captures most of the gap to their paid product. That is not the number you fabricate.
- **"Accuracy" means agreement with a frontier consensus** (averaged GPT-6 Astra + Claude Fable 5.1 at high thinking), not human ground truth. So the finding is really *decomposition makes a cheap model imitate a frontier panel far better*, which is the right framing for routing and the wrong framing for tasks where being correct matters on its own terms.
- **No absolute score exceeded 78.4%** in the entire suite. These are hard or ambiguous tasks; read the deltas, not the levels.
- **One evidence source.** Status below reflects this.

## Worked numbers

Claude Haiku 4.5, one-shot prompt vs. decomposed workflow, same tasks:

| Task | One-shot | Decomposed | Delta |
|---|---|---|---|
| Security-incident triage | 17.1% | 58.8% | +41.7 pts |
| Agent-trace observability | 29.3% | 57.2% | +27.9 pts |
| Invoice processing | 6.0% | 42.9% | +36.9 pts (7.2x) |
| Customer service | 19.9% | 55.4% | +35.5 pts |

Cost and latency fell in every row alongside the accuracy gain (e.g. security triage: $0.0068 → $0.0047 per case, 4.6s → 3.4s).

Frontier models show the same direction at smaller magnitude — Opus 5 on invoice processing went 66.9% → 78.4% while cost dropped from $0.7699 to $0.4856 per case and latency from 188.7s to 92.1s.

## How to apply

1. **Recognize the shape.** This applies to *decisions*, not generation. If the output is a verdict, a category, a score, or a routing choice, it decomposes. If the output is prose, it does not.
2. **Write the sub-questions as typed primitives.** Yes/no with a probability, pick-one-of-N with a distribution, or a bounded score. Resist sub-questions that want a sentence back — a free-text sub-answer just relocates the monolithic problem.
3. **Push everything you can into code.** "Is the amount over the approval threshold?" is arithmetic, not inference. The model should only be asked what genuinely needs judgement; every question you move to code is free, deterministic and testable.
4. **Assemble in code, not in a prompt.** The composition step is where the audit trail lives.
5. **Re-benchmark your cheap model after decomposing, before escalating a tier.** The escalation may be unnecessary — and the one-shot benchmark that motivated it was measuring the wrong thing.
6. **Keep the sub-answers.** They are the debugging surface, the cache key, and the escalation trigger (low confidence on one sub-question → escalate that question, not the whole task).

**Where this lands in current work:**

- **Inception's harness** ([[2026-06-17-in-repo-coding-harness]]) — the review/gating steps in a coding harness are decisions in exactly this shape. This gives the decompose-first structure a published number instead of an intuition.
- **Sage's AgentMail triage** — the obvious first personal worked example. Also the natural A/B against a typed-decision model.
- **Routing for the December local-inference host** ([[project-local-inference-track]]) — "the weaker the model, the bigger the gain" is directly load-bearing for a plan built on running smaller models locally. Decomposition is what makes the local box's model tier viable for decision work; without it the local-routing hypothesis is much weaker than it looks.

**Open question:** this entry currently documents the *effect* without a second independent source. A worked example on Vince's own data (AgentMail triage is the cheapest one) would move it toward multi-source. Until then do not present the numbers to the work team as settled — present the technique, which stands on its own logic, and cite the numbers as vendor-published.

**Status:** seed (single-source, vendor-published — worked example on own data pending)

Related: [[2026-06-17-in-repo-coding-harness]] (harness gating steps are decisions of this shape). [[2026-06-02-pocock-7-step-engineering-workflow]] (same anti-monolith instinct applied to engineering process rather than to a single model call). Origin note with full model-by-model tables and the Jev assessment: `inbox/20260922-typesafe-jev.md` (gitignored).
