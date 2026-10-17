# Frontier scaffolds, local fills: what a weak local executor must be handed

**Pattern:** When a small local model is the executor and a frontier model is the planner, the planner's job isn't done when the task is described. A weak executor reliably finishes a unit of code only when it is handed **four things**, each of which it can't produce for itself:

1. **Acceptance tests as the spec**, written by the planner from real captured data, before any code exists.
2. **A lock on the spec enforced by the operating system, not by the prompt.** Read-only mounts of the tests and fixtures.
3. **A skeleton** for any unit that has to plug into a framework: construction, wiring and helpers already written, with one stub method and a step-list docstring for the logic.
4. **Enough context window that the work never compacts away**, plus the harness re-stating the rules if it does.

The executor writes the logic. The planner writes everything that decides *where* the logic goes.

## Why it matters

Observed across two tasks on one 9B model (qwen3.5:9b, thinking on, OpenCode, rootless container sandbox, 16GB Apple Silicon), October 2026:

- **Task 1: one pure function (a log parser), 14 tests.** It passed with tests and a read-only lock alone. This is the easy case. It's what makes "local models can code" demos look convincing.
- **Task 2: a stdlib streaming HTTP metrics proxy, 24 tests in four parts.** In one shot: 9/24. Decomposed into one test group per session, the stateless parts passed first time. The rest:

| Unit | Shape | Without the missing piece | With it |
|---|---|---|---|
| Stream filter | state carried across calls, byte-exact output | **4/6 at 32K**: 3 compactions, a stall after each, rules lost from every summary | **6/6 at 48K**: zero compactions, nothing else changed |
| Threaded server | framework wiring + upstream streaming | **0/7** after ~2h50 at 48K: hangs, then wrong signatures wrapping the stdlib server | **7/7 with a skeleton**, and it stayed inside the one stub method |

Each "with it" column changes **one thing**. The 0→3/7 gain on the server came before any permission change, so it's down to the skeleton.

**Why the skeleton works.** The server failed for two reasons the executor can't fix by trying harder:
- **Inversion of control.** The framework constructs the handler for every request, so configuration has to reach it indirectly. The model's instinct is to pass it in directly, which leads to subclassing the framework with mismatched signatures.
- **Failures that are silent.** Wrong framing in a streaming HTTP/1.1 proxy doesn't raise an error. The client just waits. A small model learns from error text, and a 120-second timeout gives it none. The skeleton removes both by owning construction and using a connection-close framing that can't hang. What's left is logic that fails *loudly*, with a traceback pointing at the stub.

**Why context is a hard floor, not a dial.** At 32K, compaction didn't just lose tokens. It lost *why the last attempt failed* (so rejected approaches came back), lost the standing rules (so forbidden write paths came back), and ended the turn on the summary (so it stalled every time). Same model, same everything at 48K: no compaction, and with headroom it delegated bulky fixture reading to **subagents on its own**, keeping its main context for the code.

**Consistent with outside evidence.** A 51-run community measurement of a local 27B executor under a frontier planner: 42/42 on full-code handoff, 9/9 on seeded-bug debugging, **0/3 given only an interface and rules**. The less the planner hands over, the faster a local executor falls off. This entry names what "hands over" has to include.

## What was ruled out, and why

Recording the negative space deliberately ([[2026-06-10-negative-space-documentation]]):

- **Prompt-level decomposition alone.** "Implement only part X" scoped which tests it ran, not what it wrote. Its first edit sketched the entire interface from the test docstring. Scope the *spec* the executor can see, not the instruction.
- **Prompt- or permission-level spec locks.** An edit-deny on the test file was routed around in one step with `echo >`. Removing a bash allow-rule moved file writes to `/tmp/fix.py`. Only the read-only mount held, every time.
- **Treating bash file-writes as defiance.** Exports show exact-match edits failing (whitespace, a malformed match), then a fallback to Python `str.replace`, `sed -i` or whole-file heredocs. And once given a sanctioned path, it used the proper write tool immediately. These are workarounds for a failing or forbidden tool, so the fix is a forgiving write path plus git as the undo, not more denial.
- **Trusting the executor's progress signal.** `pytest -x | head` showed "1 failed, 8 passed", which meant *stopped at test 9*, and both the human and the compaction summaries read it as nearly done. Ban `-x`, and run the full suite after each unit.
- **"Just use a bigger window" as the whole answer.** 48K fixed the stateful filter and did nothing for the server. Context and scaffolding fix different failures.

## How to apply

1. **Planner writes the tests first**, from real captures, and the locked spec is mounted read-only.
2. **Classify each unit before handing it over.** Stateless transform → tests suffice. State across calls → tests + adequate context. **Framework or concurrent-I/O wiring → planner writes the skeleton**, and the executor gets a stub with a step-list docstring and helpers that make failures loud and fast.
3. **Hand over one unit's spec at a time.** Hide out-of-scope tests or expect drift toward any failure it can see.
4. **Size the window to the executor and the task class.** For this 9B on multi-part code, 48K was the floor. Re-state the rules after every compaction, and auto-continue the turn that ends on a summary.
5. **Give it an undo** (git with a starting commit, named in the prompt) and **a forgiving write path**. Let it run read-only probes; byte-exact specs need byte views.
6. **Measure honestly:** no `-x`, full suite per unit, and record nudges and interventions separately from passes.

## Where this came from

Vince's M4 trial (sittings 2 and 3, 2026-10-11 → 10-16), run as acceptance-test-driven development: Claude drafted tests in a separate context from real Ollama captures, and the local model implemented them. The skeleton step was Vince's call after the unscaffolded server run failed. The full run log and a coding-subagent handoff for the harness projects are in gitignored local notes.

Builds on [[2026-09-25-adopt-the-runtime-own-the-scaffolding]]: this is what "own the scaffolding" means *per task* when the executor is weak. The orchestrator writes the skeleton, not just the plan. It's also the same lever as [[2026-09-22-decompose-decisions-into-typed-questions]]: the weaker the model, the more the structure around it carries.

**Status: seed (observed).** One executor model, one harness, two tasks. The specific thresholds (48K, which units need skeletons) are likely model- and harness-specific. The four-handed-things shape is the claim. Needs a second executor or a second harness before it's promoted.
