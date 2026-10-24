# Detect it before the user does: promises and synthetic probes for unattended agents

**Pattern:** Run an unattended agent the way an SRE team runs a service. For each outcome a user depends on, write a **promise** in plain terms ("a ticket gets a response within an hour"). Back every promise with a **synthetic probe**: a fake but realistic task that goes down the real path, on a schedule *and* after every change. **Monitor every step** of the workflow, not just the end result, so a failure shows up at the step where it happened. The goal is that **you find the failure, not the user**.

## Why it matters

Agent failures are mostly **silent**. Nothing raises an error; the work just doesn't happen, or happens wrong, and it looks fine from outside. Every failure that cost time in the October 2026 local-model trial was this kind:

- An unreadable config made the coding harness **fall back to a cloud model** with no rules loaded. The session looked normal; only the model name in the footer gave it away.
- The agent **stalled after a context compaction**. No error, just a turn that ended and never resumed.
- Tests for an unimplemented server **hung**, so the run sat there instead of failing.
- The inference engine, when its memory guard rejects a request mid-stream, **returns HTTP 200 with an empty stream**, which looks like the model chose to say nothing.

Outside evidence points the same way:
- An agent given a real business for four weeks spent eight days generating leads nobody could buy, because it had **no way to mark a prerequisite as blocked**. A self-grading loop wrote its conclusions into the same file as its raw observations, so one wrong call contaminated every later session (AgentNews #30).
- A rogue agent breached a portal and went **unnoticed for three months, until a third-party monitor caught it** (Weekly Claw ep. 31).
- Community agent runtimes churn fast, and the dominant complaint is **upgrades that break things quietly**: a `doctor --fix` that silently drops config fields it doesn't recognise, and a gateway that won't start over SSH on a headless Mac. In the October sweeps of those communities, users find these breakages themselves. **No runtime was seen shipping a canary or health-probe layer.** That is an observed absence, not a proven one.

The SRE framing fixes the gap at the right level. "Is the process up?" is the wrong question: a gateway can be up and answer nothing, and a server can return 200 with an empty body. A promise is stated in **user outcomes**, so its probe has to check the outcome.

## What was ruled out, and why

Recording the negative space deliberately ([[2026-06-10-negative-space-documentation]]):

- **Process-level health checks.** Up isn't working. Every silent failure above passed a liveness check.
- **Status-code checks.** An empty 200 stream passes. Probes have to assert **content**: a known answer, and the expected model ID.
- **The agent's own reports.** The four-week business agent graded itself into a loop. Probe results must live **outside** the agent's memory and notes.
- **Probing on a schedule only.** Upgrades, config edits and model swaps are when things break. Run the probes **after every change**, before the change is trusted.
- **Alerting through the agent's own channel.** If the agent is what broke, its messages don't arrive. Alerts go out of band.

## How to apply

1. **Write three to five promises per agent, in user terms.** For a personal assistant: "replies to a direct message within 2 minutes"; "the morning briefing lands by 07:00"; "new mail is triaged within an hour". Each gets a number.
2. **One synthetic probe per promise.** A canary task with a known correct answer, sent down the real path. Assert the content, the model that answered, and the time taken. Run it on a schedule and **after every upgrade, config change or model swap**. A failed probe blocks the change.
3. **Instrument each step.** Every step of a workflow emits start and end events with a time budget. A step over budget raises a "stalled" event, so the parent hears an outcome either way. A community fleet design already does this: 450 s idle or 1,200 s inside a tool, with a 120 s grace period.
4. **Give the agent a "blocked" state.** A prerequisite it can't satisfy should surface as blocked, not as more work.
5. **Keep probe results separate** from the agent's working memory, and send alerts out of band (a phone push, not the agent's chat).
6. **Pair it with pinning.** Pin the runtime you adopt ([[2026-09-25-adopt-the-runtime-own-the-scaffolding]]) and promote an upgrade only once the canaries pass on it.

## Where this came from

Vince attended an AI SRE event on 2026-10-24. The practice there centred on synthetic testing, continuous monitoring and setting a promise, for example trouble-ticket responses within an hour with every step of the ticket process monitored, so failures are detected internally rather than by the customer. Vince noticed that the community agent runtimes he follows, with their upgrade churn, have no equivalent. Sage connected it to the silent failures logged during the M4 trial and to the agent-ops evidence already on file.

Related: [[2026-10-16-frontier-scaffolds-local-fills]] (the same trial's failures, from the build side).

**Status: seed (single-source practice + observed failures; not yet applied).** First application: the always-on assistant on the incoming Mac Studio. Promote when a probe catches a real failure before a user does.
