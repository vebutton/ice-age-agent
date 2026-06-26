# Claude Fable 5 — capability profile and bootstrap-window note

> **STATUS: RESTRICTED (2026-06-26).** Fable 5 was withdrawn at US-Government
> direction and is **not currently available** — restricted for now until Anthropic
> can secure it better; not permanently obsolete. Vince reverted all projects to
> **Opus 4.8 (1M)** as the standard. The capability profile and bootstrap-window
> deadlines below are **historical** — kept for reference and for when/if Fable is
> restored. See memory [[project-fable5-pulled-opus48-standard]].

**What it is:** Mythos-class Claude model released 2026-06-09. API ID `claude-fable-5`. State-of-the-art on nearly every benchmark Anthropic published. Available everywhere day one, including Claude Code and Cowork.

**Fable 5 vs. Mythos 5:** identical underlying model. Mythos 5 has safeguards lifted for authorized cyber/biomedical users (trusted-access enrollment). For general agentic/coding/knowledge work, **Fable 5 is the relevant one** — no Mythos enrollment needed.

## The time-sensitive bit (deadline)

- **Free on Pro/Max/Team/seat-based Enterprise from June 9 to June 22, 2026.** From **June 23** it shifts to usage-credit-based with gradual restoration.
- Interactive Claude Code is subscription-included, so bootstrapping during this window costs nothing extra (see [[CC subscription vs API cost]] calculus — no headless `claude -p` loops needed to get the benefit).
- **Implication:** front-load any Fable-5 bootstrap work before June 22. After that, the cost calculus changes (usage credits) and per-project monetization flags matter again.
- Post-window API pricing for reference: **$10/M input, $50/M output** (~half of Mythos Preview).

## Capability profile (vs Opus 4.8 unless noted)

Source: Anthropic announcement + published benchmark table (single-source; values are the higher of Fable 5 / Mythos 5, within 1–3 pts of each other).

**Agentic coding — the standout:**
- SWE-Bench Pro **80.3%** (Opus 4.8 69.2%, GPT 5.5 58.6%, Gemini 3.1 Pro 54.2%)
- FrontierCode Diamond **29.3%** (Opus 13.4%) — >2× on hard production-codebase tasks
- Terminal-Bench 2.1 **88.0%** (Opus 82.7%) — directly relevant to Claude Code CLI work
- Anecdote: Stripe migrated a 50M-line Ruby codebase in a day (vs. ~2 months manual)

**Long-horizon autonomy + file-based memory:**
- "Works autonomously for longer than any previous Claude."
- Anthropic's Slay-the-Spire test: persistent **file-based memory improved performance 3× over Opus 4.8.**
- **Why this matters for Sage's own architecture:** independent validation of [[on-demand context routing]] — the `CLAUDE.md` brief + `docs/session-state.md` split and the global memory scheme are exactly the file-based-memory shape Fable 5 gets 3× leverage from. Projects bootstrapped on Fable 5 *with* the routing template should benefit more than Opus did.

**Vision → code:**
- "Rebuild web app source code from screenshots alone." GDP.pdf vision **29.8%** (Opus 22.5%). Makes screenshot-driven UI/Mac-app scaffolding viable.

**Long context:** "Stays focused across millions of tokens in long-running tasks" — relevant to cross-project knowledge-holding, not just one-shot bootstraps.

**Self-validation:** "At the highest effort, reflects on and validates its own work." Worth setting effort high for architecture-decision moments.

## The caveat to know

Fable 5 has classifier safeguards that **auto-fall-back to Opus 4.8** for cybersecurity-exploitation, biology/chemistry, and model-distillation queries — silently. The starred (\*) benchmark rows (ExploitBench 78%, BioMysteryBench, etc.) reflect that fallback, so they don't represent Fable-level capability on those domains. For infra / agents / Mac-app work this won't bite; if a project touches offensive-security tooling, expect Opus-level responses there.

## Status

*Status: reference (single-source — Anthropic announcement + benchmark screenshot, 2026-06-09). Capability claims are vendor-reported; treat as orientation, not independently verified. Re-tier if third-party benchmarks or hands-on results confirm.*

Sources: `collateral/Screenshot 2026-06-09 at 6.28.46[U+202F]PM.jpg` (benchmark table), https://www.anthropic.com/news/claude-fable-5-mythos-5.
