# Git commits need a Claude skill, not per-project HEREDOC rediscovery

**Pattern:** Claude reinvents safe commit-message handling on every new project. Inline `git commit -m "..."` fails on quotes / special chars; the file-based workaround (`git commit -F <file>`) is the durable answer. HEREDOC is a workaround on top of a workaround — it still treats commit creation as ad-hoc shell work that has to be rediscovered each session. This is the textbook "skill, not improvisation" candidate.

**Why it matters:** Wasted tokens, slower velocity, inconsistent commit hygiene (trailer attachment, --amend safety after hook failure, multi-line bodies) across every Claude Code instance Vince runs. The cost compounds across the portfolio. Worse: it's exactly the kind of friction that's *invisible* in any single project but obvious when you see it happen in twenty. Robin can't catch this because she lives in OpenClaw and never sees the commit step.

**Where this came from:**
- Vince named it explicitly in the bootstrap conversation 2026-05-18 (`collateral/Ice-Age-agent.md`): *"Claude is continuously trying to check-in commits and making a commit message in line, and it always fails... I just think that should be part of a Git GitHub skills."*
- Reinforced live in the bootstrap session itself: Claude tried inline HEREDOC for Sage's own first commit, the user interrupted with *"we've already missed the gh skill of creating the commit message as a file to avoid inline text issues."* The agent designed to catch this pattern missed it on her own commit.

**How to apply:**
- **Now:** Use file-based commit messages by default — write the message to a file (e.g. `/tmp/commit-msg.txt`), then `git commit -F <file>` and delete the file. Even when HEREDOC would work, prefer the file approach to build the habit.
- **Soon:** Build a `git-commit` Claude skill that wraps: file-based message authoring, trailer attachment (Co-Authored-By etc.), hook-failure recovery (NEW commit, never --amend), and refusal to run with `--no-verify` / `--no-gpg-sign` unless explicitly requested.
- **Later:** Audit adjacent git/gh operations Claude routinely reinvents — PR creation with body files, conflict resolution flow, force-push safety. This is the seed of a `git-ops` skill bundle.
- **Sage-specific:** When Sage produces Kickstart packs for new projects, the seeded `CLAUDE.md` should reference this skill (or its replacement) so the receiving project starts already wired for it.

**Status:** seed
