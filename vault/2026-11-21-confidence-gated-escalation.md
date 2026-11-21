# Confidence-gated escalation: a small model decides, and hands up only what it's unsure of

**Pattern:** Put a small, cheap decision model in front of the expensive path. It answers every
item with a typed answer and a confidence. Items above a threshold are handled (or dropped) on its
say-so; items below go up to a frontier model or a human. The expensive path then sees only the
fraction of traffic that needs it.

Two rules make it work, and the second is the one people skip:

1. **Route on confidence, but pick the threshold on your own labelled data.** A benchmark's
   calibration score does not tell you which model or which threshold works on your traffic.
2. **Gate humans on the cost of undo, not on confidence.** Confidence decides *which model* looks
   at something. Whether a *human* must approve depends on whether the action can be taken back.
   An irreversible action needs a human at any confidence.

## Evidence

**Practitioner reports (two, independent):**
- **Proceda** (AI Tinkerers demo): Jev decides "is this step done / which outcome"; when unsure it
  falls back to the main model. About 30% fewer main-model calls, 360/360 correct. Speaker claim,
  not reproduced.
- **Hightouch** (HN, 603 pts, 2026-11-06): "a Jev that can ask Opus for help gives Opus accuracy at
  4x lower cost." Vendor post.

**Own measurement (M4 Pro 16GB, oMLX/Ollama, 2026-11-19 → 11-21).** Two tests, four open decision
models:

| Model | Typed Decisions acc / ECE | HN filter AUC | HN: skip at 90% recall | HN: skip at 100% |
|---|---|---|---|---|
| d3-mini oQ8e | 0.727 / 0.042 | **0.933** | **84%** | **49%** |
| elephant-1 9B 4-bit | **0.749** / 0.108 | 0.931 | 78% | 33% |
| strands-decider 2b | 0.605 / 0.101 | 0.908 | 72% | 44% |
| clef-flash oQ4e | 0.695 / **0.013** | 0.894 | 72% | 26% |

Typed Decisions = 400 public synthetic cases (gold is a teacher model). HN filter = 1,088 Hacker
News stories over 150 points from three past research sweeps; positive = the sweep mentioned it
(58). "Skip at 90% recall" = the share of stories the expensive path never has to read while still
catching 90% of what the sweeps caught.

**What the numbers say:**
- **The escalation saving is real and large.** With d3-mini at a ~0.35 threshold, the expensive
  path reads about 1 in 6 stories and still gets 9 in 10 of the ones that mattered.
- **Benchmark calibration did not predict real-traffic ranking.** clef-flash was by far the best
  calibrated on Typed Decisions (ECE 0.013) and came last on the HN filter: it parks irrelevant
  items around 0.3 instead of near 0.1, so any useful threshold lets more through.
  strands-decider, last on the benchmark, beat it on HN at 6x the speed. Hence rule 1.
- **The last few percent of recall are expensive.** Going from 90% to 100% recall roughly halves the
  saving for every model. Decide up front what a miss costs, then set the threshold there.
- **Some items get past every model.** All four missed one obviously on-topic story ("Jev meets the
  Trolley Problem"). Escalation by confidence won't catch blind spots shared by every model. A small
  random sample of low-confidence items sent upward anyway is the cheap check.
- **Weakest workflow everywhere: agent-trace observability** (0.45–0.66 accuracy on Typed
  Decisions). Don't gate "is this agent going wrong?" on a small model without testing on real
  traces.

## How to apply

1. **Decompose first** ([[2026-09-22-decompose-decisions-into-typed-questions]]): escalation needs
   a typed answer with a probability, not prose.
2. **Label a few hundred items from your own traffic.** Past outputs from the expensive path work
   as labels (here, what earlier sweeps surfaced). Expect them to be imperfect; they're still better
   than a public benchmark for this choice.
3. **Choose model and threshold from "skip at target recall"**, not from accuracy or ECE alone.
   Pick the target from what a miss costs.
4. **Separate the two gates.** The confidence threshold routes between models. A fixed list of
   irreversible actions (send, delete, pay, deploy, merge) always goes to a human, whatever the score.
5. **Send a small random sample of below-threshold items up anyway** and track how many the
   expensive path would have kept. That is the drift alarm, and it checks evidence, not the small
   model's own opinion of itself ([[2026-10-24-promises-and-synthetic-probes-for-agents]]).

**Where this lands:** Sage's research sweeps (d3-mini as the HN pre-filter on the Studio, oMLX
chosen as the engine 2026-11-20); Robin's service design (undo-priced human gates); the coding
harness review step ([[2026-06-17-in-repo-coding-harness]]).

**Limits:** one person's traffic, labels produced by a frontier model rather than the person,
titles only, no option-order shuffle test, one run each. Practitioner sources are vendor or speaker
claims.

**Status:** seed (multi-source: two practitioner reports + one own measurement)

Related: [[2026-09-22-decompose-decisions-into-typed-questions]] (the input shape this needs).
[[2026-10-24-promises-and-synthetic-probes-for-agents]] (sampling below-threshold items is a probe).
"Harness, Loop, Graph" post: `inbox/20261115-harness-loop-graph.md` (gitignored). Full results:
`local/decision-test/20261120-typed-decisions-results.md` (gitignored).
