# Handoff documents — structured context transfer when fresh context beats accumulated context

**Pattern:** When an investigation, tangent, or specialist sub-problem would *pollute* the main session's context, don't try to do it in-line. Compact the current state into a structured **handoff document** and spin up a second session in fresh context. The two sessions share the repo, credentials, and tools — they don't share working memory. The handoff doc is the bridge.

**Why it matters:**

- **Context is a finite resource.** Big-context investigations (debugging, performance analysis, schema archaeology) crowd out the main task's working memory. Trying to do both in one session degrades both.
- **Fresh context outperforms accumulated context for many problem types.** A clean session approaches a debugging problem without the bias of "what we were trying to do" — exactly what you want for "why is this slow / broken / unexpected."
- **The structure of the doc is the contract.** A well-shaped handoff lets the fresh session pick up *immediately* without re-discovering what's already known or already ruled out.
- **It generalizes beyond debugging.** Any time you need a focused specialist context for a tangent — schema redesign, security review, performance investigation, learning a new framework — the handoff pattern lets you scope it cleanly without the main session bearing the cost.

**The shape of a good handoff** (from the skill at `~/.claude/skills/handoff/SKILL.md`; section #3 added 2026-05-27 from cross-comparison with steipete/agent-scripts):

11 sections in fixed order. Omit only if genuinely empty:

1. **Goal of the next session** — one or two sentences. If the user passed an argument, that's authoritative.
2. **Context** — repo, branch, current commit SHA, environment specifics (cluster names, endpoints, hostnames). Subject to the Safety rule below.
3. **Live state / running processes** — anything still running from this session that the next agent needs to attach to or be aware of: tmux sessions (with paste-ready `tmux attach -t <name>` commands), dev servers, debuggers, long-running tests, background scripts, attached agents. Most pure-investigation handoffs won't need this. Operationally-flavored handoffs (mid-deploy, mid-build, mid-debug-session, parallel-agent orchestration) will. Borrowed from steipete/agent-scripts.
4. **What's been done** — concrete actions in this session (commands run, files changed, deployments triggered, tickets opened). Reference artifacts by path or URL, don't restate them.
5. **What's been ruled out** ⭐ **The highest-value section.** Hypotheses already disproved with the evidence that disproved them. Include false leads, red herrings, "I thought X but X turned out to be fine because…". *Do not skip this section* — the negative space is where the leverage is.
6. **Open hypotheses** — live theories ranked by likelihood, each with evidence for and the next check that would confirm or kill it.
7. **Reproduction / how to verify** — exact commands the next agent runs first to confirm same problem state.
8. **Relevant files & endpoints** — bullet list of paths, URLs, dashboards, log locations. References only, no content.
9. **Required tools & skills** — skills, MCP connectors, CLI tools the next agent will need. Flag anything that may not be installed.
10. **Out of scope** — explicit "don't do these" instructions ("don't touch the prod keystone config", "don't re-run the 40-minute Ansible playbook").
11. **Bootstrap prompt** — a single paste-ready prompt for the fresh session. E.g., *"Read `./.claude/handoffs/2026-05-22-openstack-slowness.md` and start with the top-ranked open hypothesis."*

**Storage convention:** `./.claude/handoffs/<YYYYMMDD>-<short-slug>.md` inside the repo (so it lives next to the work it describes). Fallback to `~/.claude/handoffs/` if there's no repo. Slug is short kebab-case derived from the focus area (`openstack-slowness`, `flaky-ci`, `auth-refactor`). Collision suffix `-2`, `-3` if needed.

**Safety rule:** never include credentials, API tokens, private keys, or full connection strings in the handoff doc. Reference the env-var name, file path, or secret-store key instead (`$OS_PASSWORD from ~/.config/openstack/clouds.yaml`). The handoff doc may end up in git, in screenshots, in conversations — keep secrets unreachable from it by default.

**Style rule:** terse. Bullets and concrete claims over narrative paragraphs. Don't duplicate content already in PRDs, ADRs, issues, commits, diffs — reference them by path or URL. The next agent has tokens to spend on the *problem*, not on re-reading prose.

---

**The negative-space insight** (folded in from candidate C; **extracted 2026-06-10** to `vault/2026-06-10-negative-space-documentation.md` — that entry is now canonical; this block kept as origin record):

The handoff skill explicitly names **"What's been ruled out"** as the *highest-value* section — above what was done, above open hypotheses, above the reproduction steps. This is a deep claim, and it generalizes far beyond handoffs:

- **Debugging notes:** what doesn't reproduce, what's been swapped without effect, what was tested and confirmed not to be the cause.
- **Post-mortems:** which contributing factors were considered and rejected, and why.
- **ADRs (architecture decision records):** the alternatives considered and the reason each was rejected is more durable than the final choice.
- **Design docs:** the constraints that ruled out simpler approaches.
- **Onboarding docs:** "this is not the way to do it" sections that prevent the new-hire failure mode.

In all of these, *negative space* — what was tried, considered, or hypothesized and ruled out — carries more durable value than the positive content. The positive content tells you what was done; the negative space tells you what *not* to try next time you face the same problem shape. Knowledge of "X doesn't work because Y" is often more reusable than knowledge of "we did Z."

This insight may deserve its own vault entry if it shows up independently in other projects' artifacts (the cross-project sweep is the right validation mechanism — see [[feedback-pattern-domain-specificity]]).

---

**Mechanics in practice** (worked example from an internal infra/ops project, 2026-05-22):

Two terminals open in the same repo. Main session was building Ansible playbooks (big context, lots of state). Cluster slowness emerged as a tangent that needed investigating but would have wrecked the playbook context. Workflow:

1. In the main session: *"Write a handoff for investigating why this OpenStack env is slow."*
2. The skill produced `./.claude/handoffs/20260522-openstack-slowness.md` with all 10 sections filled in, ending with the bootstrap prompt.
3. Second terminal, same repo: `claude` → paste the bootstrap prompt → fresh session opens at the handoff doc and starts on the top hypothesis.
4. Two Claude sessions, same files, same credentials, same MCPs. Main session kept building playbooks; side session diagnosed the cluster. Zero cross-pollination, no degraded context for either.

The output of the side session was three artifacts in the same repo's `notes/` (gitignored): an infra-team writeup, a replication playbook, and a Slack message text. The main session benefited from none of that detail; it kept its focus.

---

**Convergent discovery footnote — worth catching as a meta-signal:**

Vince had been doing this pattern informally for weeks (spinning up a second Claude, hand-briefing it from a paste-buffer) before encountering [Matt Pocock's "handoff is my new favorite skill" video](https://www.youtube.com/watch?v=dtAJ2dOd3ko) on 2026-05-22, which gave the pattern a name and a structured form. Pocock had independently arrived at the same shape. Vince improved on the basic version Pocock published.

**The meta-pattern:** practitioners are independently re-inventing the same context-management primitives in the Claude Code ecosystem right now. *"All of this has happened before, and all of it will happen again."* (Vince's framing in the Slack post.) This is an *active discovery period* — Sage's job in this period is to catch named patterns from inbound sources (newsletters, YouTube, Discord, Slack) and surface them faster than the rediscovery cycle. The handoff pattern is a worked example: by the time Vince saw the named version, he'd already lost ~weeks to informal-version friction.

**Third independent inventor confirmed (2026-05-27):** Peter Steinberger ([steipete](https://github.com/steipete)) published a `/handoff` slash-command prompt as part of his public [steipete/agent-scripts](https://github.com/steipete/agent-scripts/blob/main/docs/slash-commands/handoff.md) repo (the deeper skill lives in a sibling repo `agent-skills` he hasn't published). His checklist has 7 sections vs. the 11 here — leaner and more operationally-flavored. He converges on the core handoff-as-mechanic shape but does **not** share the "What's been ruled out" / "Open hypotheses" emphasis, and omits the bootstrap-prompt closer. His unique contribution is the **Live state / running processes** section (folded into this entry as #3).

**What the partial convergence tells us:** the *handoff-as-mechanic* is the universal pattern (three independent inventors agree on it). The *negative-space-as-headline-value* insight is distinctive to the Vince/Pocock branch — only 2 of 3 inventors named it. That distinctness *raises* the value of capturing it as its own vault entry, separate from handoffs.

---

**Where this came from:**

- The skill itself: `~/.claude/skills/handoff/SKILL.md` (installed globally by Vince, also dropped into a per-project `.claude/skills/handoff/` where it's been used in practice).
- Vince's Slack post to the work AI channel (2026-05-22) externalizing the pattern with full credit to Matt Pocock.
- Direct review of the SKILL.md content during the first ingestion-sweep session (2026-05-23).
- Cross-comparison with steipete/agent-scripts `docs/slash-commands/handoff.md` (2026-05-27 review) — third independent inventor; produced section #3 (Live state) and isolated the negative-space insight as not-universal-after-all.

**How to apply:**

- **Recommend the handoff skill** whenever Vince (or a project Sage is advising) hits a context-pollution risk — investigations, performance tangents, security reviews, third-party-integration deep-dives.
- **Treat the 10-section structure as the contract.** Skipping "What's been ruled out" is the most common failure mode and the most expensive — the section is doing the most work in the doc.
- **Apply the negative-space insight to other knowledge artifacts.** When writing or reviewing post-mortems, ADRs, design docs, or onboarding material, check whether the negative space is captured — what was considered and rejected, what didn't work, what was hypothesized and disproved. If it's not in there, the doc is leaking the highest-value content.
- **Watch for convergent-discovery signals.** If multiple inbound sources (newsletters, videos, Discord posts) name the same pattern within a short window, that's a strong signal the pattern is durable and worth a vault entry sooner rather than later.

**Status:** confirmed (2026-05-27). Promotion basis: three independent inventors of the handoff-as-mechanic pattern — Vince (informal, ongoing), Matt Pocock (named/structured, 2026-05-22 YouTube), Peter Steinberger (steipete/agent-scripts public review 2026-05-27). Note: the negative-space-insight component (`What's been ruled out` as the headline-value section) did *not* converge across all three — only 2 of 3 inventors named it. That asymmetry strengthens the case for promoting negative-space-documentation to its own standalone vault entry (tier-2 candidate already queued in session state).

Related: [[2026-05-20-claude-md-stays-lean-lazy-loading]] (both are context-management patterns — lazy-loading keeps a single session lean; handoffs split work across sessions when leanness within one isn't enough). [[feedback-pattern-domain-specificity]] (the cross-project sweep is the validation mechanism — handoff pattern crossing into more projects is what promotes this to confirmed).
