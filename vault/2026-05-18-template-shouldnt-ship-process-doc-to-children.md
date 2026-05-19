# Bootstrap templates shouldn't ship their own process doc to children

**Pattern:** `claude-agent-template` ships `project-bootstrap-process.md` (the doc describing how to bootstrap a new project from the template) into every child repo it produces. That doc is template-process metadata, not project-specific content — it's identical (or near-identical) across every child. Children should link back to the template repo for process docs, not carry their own static copy that drifts the moment the template evolves.

**Why it matters:** Every child repo bootstrapped from the template ends up with a frozen-in-time copy of the process doc. When Sage proposes a template-upgrade PR — exactly the loop she exists to drive — the upgrade lands in the template but does not retroactively update the dozens of children. The bootstrap process doc decays in every child the moment it lands. Worse, anyone reading a child repo's `project-bootstrap-process.md` might assume it reflects current practice when it doesn't. The README's link to `claude-agent-template` gives a breadcrumb to the live source; the static copy actively misleads.

**Where this came from:**
- Vince asked during Sage's bootstrap session 2026-05-18: *"do we need the project-bootstrap md in this ice-age repo?"* Rhetorical-but-not — he was naming the smell.
- Confirmed by checking the template's "What to commit" list, which explicitly includes `project-bootstrap-process.md`. The template prescribes the duplication.

**How to apply:**
- **Now (this repo):** Remove `project-bootstrap-process.md` from `ice-age-agent`. The README's link to `claude-agent-template` is enough.
- **Template-upgrade PR (promote when ready):** Update `claude-agent-template` so that step 13's "What to commit" list excludes `project-bootstrap-process.md`. Update the bootstrap checklist so the cleanup step (or a new one) removes the process doc from the child before the first commit. Update the README template to include the link back to the template repo.
- **General principle:** Audit the template for other files that are template-process metadata vs. project-specific content. Anything in the first category should not propagate.

**Status:** seed (will promote to `promoted-to-template` once the `claude-agent-template` PR lands)
