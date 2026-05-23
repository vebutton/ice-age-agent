# Gitignored repo-local configs need tracked `.sample` companions

**Pattern:** Any file that has to be gitignored because of proprietary / environment-specific content (CLAUDE.md, vars/main.yml, .env, credentials, ansible inventories, anything similar) should ship with a tracked `*.sample` (or `.example`) companion in the repo. Real file gitignored; sample tracked. Fresh clones bootstrap by copying the sample, then filling in the local-specific values.

**Why it matters:**

- **Gitignoring without a tracked template makes fresh clones painful.** Every clone has to manually recreate the file from memory, from another machine, or by asking the previous author. That's archaeology, not bootstrap.
- **The template encodes the *shape* of the file** — section structure, required fields, expected env vars — without leaking the *values*. A teammate (or future-you, or an AI agent reading the repo cold) can see exactly what needs to be filled in.
- **It scales to more than secrets.** `.env.local.example` is the well-known case, but the same pattern applies to any repo-local config: Ansible vars, Terraform tfvars, project CLAUDE.md files where the context is proprietary, IDE settings, deployment configs.
- **It survives the "I don't always commit CLAUDE.md" reality.** Sometimes a project's CLAUDE.md has customer / internal context that doesn't belong in a public repo. Without a template, every clone re-derives the structure from scratch.

**Two ways to handle proprietary content in CLAUDE.md** — pick by fit:

1. **Split** (this repo's approach): `CLAUDE.md` tracked + public, `CLAUDE.local.md` gitignored for proprietary bits. The split is explicit and clean.
2. **Sample-template** (the pattern named here): the whole `CLAUDE.md` is gitignored, `CLAUDE.md.sample` is tracked as the fill-in-the-blanks template.

Decision rule: **can the proprietary content be cleanly factored out into a small companion file?** If yes, split (Approach 1 — fewer manual steps on clone). If the proprietary content is woven through the whole file and factoring it out would gut the document's coherence, sample-template (Approach 2 — single source of truth, one copy step on clone).

**Where this came from:**

Vince surfaced this 2026-05-22 from a separate project (Ansible / infra-shaped, given the `vars/main.yml.sample` mention). In that project the convention was already partially in place — `vars/main.yml` gitignored with `vars/main.yml.sample` tracked — but `CLAUDE.md` was gitignored without a sample companion. Bringing up a new clone for a different environment exposed the gap: the CLAUDE.md edits made in the original clone weren't reachable from the new clone via git, because the file was gitignored entirely. The fix was to add `CLAUDE.md.sample` to match the existing `vars/main.yml.sample` convention.

This generalizes the `.env.local` / `.env.local.example` pattern already documented for third-party MCP wireup (see [[2026-05-20-third-party-mcp-wireup-pattern]]) — the same discipline, applied beyond env-vars to any repo-local config with proprietary content.

**How to apply:**

- **In this repo:** doesn't change anything immediately — Sage uses the split approach (`CLAUDE.md` + `CLAUDE.local.md`), which works for this codebase. But the principle is now codified.
- **In kickstart packs Sage produces:** include the decision rule in the seeded `CLAUDE.md` (or in a one-page "project conventions" doc). For each gitignored config file in the seeded skeleton, ship its `.sample` companion. Never gitignore a file in a fresh skeleton without simultaneously creating its tracked template.
- **Template-upgrade PR against `claude-agent-template`:** the template should bake in this discipline. Two improvements:
  - Document the split-vs-sample-template decision rule in the bootstrap doc.
  - Audit the template's own gitignores: every gitignored file pattern should either have a `.sample` companion or be explicitly tagged as "transient, no template needed" (e.g., `*.pyc`, `__pycache__/`).
- **General principle:** when adding a new file to `.gitignore`, ask "does this file have shape that a future clone needs to recreate?" If yes, add the `.sample` companion in the same commit.

**Status:** seed (will promote to `promoted-to-template` once the `claude-agent-template` PR lands)

Related: [[2026-05-20-third-party-mcp-wireup-pattern]] (specific case for `.env.local` / credentials), [[2026-05-18-template-shouldnt-ship-process-doc-to-children]] (adjacent — both about what propagates from template to children and what doesn't)
