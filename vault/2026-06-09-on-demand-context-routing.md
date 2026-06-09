# On-demand context routing — front-loaded summary, lazy-loaded detail

**Pattern:** When a project accumulates multiple deep-context areas (architecture, workflows, decisions, session state, watchlists, etc.), don't load all of them at every interaction. Keep an always-loaded layer carrying *summary + routing logic*, and put the *full detail* in lazy-loaded files that the agent consults on demand. The always-loaded layer tells the agent **when** to drop into the detail.

## The shape

Two layers and a routing logic:

1. **Always-loaded layer** — small, dense, primarily a routing index.
   - Examples: project root `CLAUDE.md`; `CLAUDE.local.md` watchlist pointer section.
   - Carries: project-wide rules, headline summaries, when-to-load triggers for the detail files.
   - Sized for always-on cost (the harness loads it every interaction).

2. **Lazy-loaded layer** — full content.
   - Examples: `docs/architecture.md`, `docs/session-state.md`, `local/skill-watchlist.md`, `local/mcp-watchlist.md`.
   - Carries: thread-level detail, full decision archaeology, exhaustive context.
   - Loaded only when the always-loaded layer's routing logic says "go here."

3. **Routing logic** — the connection.
   - Lives in the always-loaded layer as either explicit ("when X comes up, load Y") or implicit through summary + pointer ("brief = headlines; full = `docs/Y.md`").
   - Without this, the lazy-loaded files become unreachable. The summary's job is *to know what's in the detail*, not to substitute for it.

## The routing decision: forward-looking vs backward-looking

This is the load-bearing question for *every* piece of content. Don't treat the split as "summary vs detail" — that framing fails. Ask instead:

> *Does the next-interaction agent need this AT cold start to behave correctly?*

- **Yes → always-loaded layer.** Forward-looking content: open items, next-session pickup steps, continuity reminders, task state, behavior-affecting decisions. The agent needs to know it's there before it acts.
- **No, only useful when prior context is needed → lazy-loaded layer.** Backward-looking archaeology: thread-by-thread accomplishments, decision reasoning, ruled-out paths, historical detail.

A common failure mode is treating "routing" as a summary-vs-detail question. A summary in the always-loaded layer that misses key forward-looking content fails its job — the on-demand layer doesn't get consulted reliably, and the agent operates without the operating context it needs. This was the bug in this entry's own first implementation; the correction is the refinement above.

The discipline at each session-end: walk through every section and ask the forward/backward question. Resist the urge to relocate forward-looking content to make the brief shorter. The brief's job is *operating context*, not *minimal headline*.

## Worked examples

- **Multi-file project context split** — a primary K8s-operator work repo I reviewed has its `CLAUDE.md` at ~3.5KB and routes deep context to `architecture.md`, `crds.md`, `flows.md`, `webhooks.md`. Each file is loaded only when its domain comes up in conversation. Inventor: an independent practitioner outside Vince's portfolio (the repo's AI context layer was set up by another engineer).
- **`local/` watchlist scheme (Vince's own dog-fooding)** — `CLAUDE.local.md` carries one-line pointers to `local/skill-watchlist.md` and `local/mcp-watchlist.md`. Pointers include time-sensitive callouts (e.g., "iOS skill review window: June 2026") that need cold-start visibility; the full catalogs live in the lazy-loaded files.
- **Session State split (this commit)** — project root `CLAUDE.md` keeps a ~30–50 line brief carrying forward-looking content: last-session headline, open items, task state, next-session pickup steps, continuity reminder, pointer. Full thread-level archaeology lives in `docs/session-state.md` and is consulted only when prior-thread depth is needed. Promoted to a global rule via `~/.claude/CLAUDE.md` so the pattern propagates to future projects.

## Counter-example

A bounded-scope work repo I reviewed (~12.7KB CLAUDE.md, monolithic, disciplined) didn't bother with on-demand routing — and that was the right call for its scope. The pattern pays off when there are *multiple* deep-context areas. A single coherent file is better when the total context fits in a single coherent file. Don't apply on-demand routing for the sake of applying it.

## When this pattern fits

- Project has multiple deep-context areas (architecture + workflows + decisions + session state + watchlists).
- Always-on context budget would be exceeded if all areas were loaded together.
- Some context is rarely needed but needs to be accessible.
- The "headline shape" of each area is useful at cold start, but the detail is not always needed.

## When it doesn't fit

- Bounded-scope project where total context fits in a single coherent file.
- The "summary" of a section would be longer than the detail itself (overhead exceeds gain).
- The detail file's content is needed every interaction anyway (e.g., persistent rules that fire constantly belong in the always-loaded layer).

## Common failure modes

- **Forward-looking content leaks to the on-demand layer.** "Pickup steps for next session" or "continuity reminder" placed in the lazy-loaded file because they felt like detail. The agent doesn't load on-demand files at cold start, so it misses these — and operates without the context it needed. This is the single biggest pitfall; see the routing decision above.
- **Summary loses the pointer.** The brief is in CLAUDE.md but doesn't say "see `docs/X.md` for detail." The detail file becomes unreachable.
- **Detail leaks back into the summary.** A "brief" Session State that grew to 100 lines defeats the purpose. Recheck the brief's length each session.
- **Routing logic is implicit only.** The summary just is the summary — no explicit cue. The agent has to guess when to drop into the detail. Better: include explicit triggers ("if the user asks about prior decisions, see `docs/session-state.md`").

## Status

*seed (multi-source observed)* — at least two independent inventors:
- A work-repo author (independent practitioner outside Vince's portfolio) who structured a primary K8s-operator repo this way.
- Vince's own application of the pattern across his `local/` watchlist scheme and (in this commit) Session State.

Third worked example (this commit's Session State split) doesn't add an independent inventor — it's Vince's third application — but it adds project-shape diversity, which strengthens the multi-source signal.

Not yet *confirmed* (would need a third independent inventor). Multi-source observed is solid enough to act on.

## Related

- `vault/2026-05-20-claude-md-stays-lean-lazy-loading.md` — sibling: don't enumerate MCP/skill inventories in CLAUDE.md (the harness lazy-loads them already). Same family of "keep the always-loaded layer lean."
- `vault/2026-05-23-handoff-pattern-structured-context-transfer.md` — handoff doc is a closely related shape: a structured summary that lets the next agent decide what to dig into. Session State as routed-summary is a special case of the handoff pattern applied continuously rather than at transitions.
- `vault/2026-06-09-skills-extracted-not-anticipated.md` — same anti-bloat discipline applied at the skill-creation layer (don't ship skills you haven't earned the reuse demand for).
- [[feedback-session-state-forward-vs-backward]] — the operating rule for applying this pattern to session state specifically.
- [[feedback-pattern-domain-specificity]] — the "don't elevate to universal on thin evidence" discipline applies here too: bounded-scope monolithic CLAUDE.md is a valid choice, not a failure mode.
