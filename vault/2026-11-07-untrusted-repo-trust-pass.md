# Trust pass for an untrusted repo: read it from a clean room, diff against upstream, then check your own config

**Pattern:** Any folder an agent reads that you didn't write (a fork, a clone, a template, a skill, an MCP server, a download) can carry instructions the agent obeys. Instruction files (CLAUDE.md, AGENTS.md and friends) can only *persuade*; settings files (hooks, MCP config, base-URL overrides) *execute and persist*. The attack chains them: the text talks the agent into editing the settings. So the pass reads both kinds of file, reads them from somewhere with nothing worth stealing, and ends by checking that your own config didn't move.

## Why now

- **Incident (2026-10-25/26, Forever Security):** a malicious AGENTS.md in a customer's `~/Downloads` tried to make Codex install a known-malicious npm package. It was blocked before anything ran. A web page can drop such a file into Downloads quietly; the victim then has to launch an agent there.
- **Controlled tests:** Codex ran the payload in about 90% of attempts. In Claude Code, a CLAUDE.md redirected API traffic to a researcher's server about half the time, and the captured OAuth token replayed against the real API, so the result is account takeover, not one bad session.
- **CSA research note (2026-10-27), "Markdown Is the New Executable":** the instruction-file vs settings-file split above.
- **Loading behaviour that matters:** Claude Code loads CLAUDE.md from every directory *above* the working directory at launch, and from subdirectories on demand when it touches files there. OpenCode reads AGENTS.md. A `-maxdepth 3` check misses deep files and misses the settings files entirely.

## The clean room (set up once)

- An empty directory, **not** under any folder holding an untrusted CLAUDE.md. Launch Claude Code there; give it the target repo as a path. **Never launch an agent with the target repo as its working directory** until the pass is done.
- No `.env.local`, no `.mcp.json`, launched without `source .env.local`. No Slack, AgentMail, Playwright or GitHub token in reach of the session.
- Honest limit: on the same macOS user, the Claude Code login and the `gh` keyring login are still on the machine. For anything you will *run* (not just read), use a container or VM (the OpenCode `:ro,Z` container shape) or a separate macOS user.

## The pass

1. **Provenance.** `git remote -v`, the upstream URL, the fork point. Who added each instruction and settings file: `git log --follow --format='%h %an %ad %s' -- <file>`.
2. **Inventory, no depth limit.** Instruction files: `CLAUDE.md`, `CLAUDE.local.md`, `AGENTS.md`, `GEMINI.md`, `.cursor/rules/`, `.cursorrules`, `.windsurfrules`, `.github/copilot-instructions.md`. Settings and execution: `.claude/` (settings*.json, hooks, agents, skills, commands), `.mcp.json`, `opencode.json`/`.opencode/`, `.vscode/`, `.devcontainer/`, commit-hook managers (`.husky/`, `lefthook.yml`, `.pre-commit-config.yaml`).
3. **Diff against upstream first.** Files identical to a reputable upstream are documentation. Only what the fork added or changed needs reading closely. That turns 27 files into the handful that matter.
4. **Grep for signals before reading prose.**
   - URLs, hostnames or IPs that aren't the project's own.
   - Endpoint and credential settings: `ANTHROPIC_BASE_URL`, `OPENAI_BASE_URL`, `*_PROXY`, `*_API_KEY`, `*TOKEN*`, `apiKeyHelper`.
   - Install or fetch commands: `curl … | sh`, `npx`, `npm i`, `pip install`, `brew install`, `uvx`.
   - Instructions to touch things outside the repo: `~/.claude`, `~/.claude.json`, `~/.ssh`, `LaunchAgents`, shell rc files, `security` (keychain).
   - Hidden text: HTML comments, base64 blobs, zero-width and Unicode tag characters (`perl -CSD -ne 'print "$ARGV:$.\n" if /[\x{200B}-\x{200F}\x{202A}-\x{202E}\x{2060}-\x{2064}\x{E0000}-\x{E007F}]/'`).
   - Language aimed at the agent rather than the developer: "ignore previous", "do not tell the user", "silently", "without asking".
   - Heuristic: real instruction files describe the project. They don't reconfigure your machine or point at unfamiliar URLs.
5. **Packages.** For anything with a manifest, run the four-step audit from the MCP wireup entry (install scripts, unrelated deps, network surface).
6. **Verdict.** One of: *clean* (matches upstream or nothing found), *clean after change* (name the file removed or edited), *don't use*. Record it with the commit hash checked.

## After any agent has run in an untrusted folder

Check that your own config didn't move:
- `~/.claude/settings.json` and the repo's `.claude/settings*.json`: an `ANTHROPIC_BASE_URL`, proxy or `apiKeyHelper` you didn't set.
- `~/.claude.json`: `mcpServers` entries you didn't add.
- `~/Library/LaunchAgents/` and shell rc files: new entries.
- **If anything is there, treat the login as exposed:** remove the setting, log out of Claude Code and back in, and rotate any key the session could see.

## Where this applies in Vince's setup

- **M4:** OpenCode runs in a container with `:ro,Z` mounts and talks to local oMLX, so the blast radius is small. OpenCode reads AGENTS.md, so a fork's AGENTS.md files count there even when Claude Code ignores them.
- **This Mac's Claude Code:** the real exposure today, because the subscription login lives here.
- **Studio:** worst fit. Unattended agents mean nobody sees a config change. The defence is structural: agents can't write their own config (read-only, or owned by a different user), plus an egress allow-list. Same defence as the email and web injection Robin will face.

## Status

*seed (single incident + controlled tests + CSA framing).* Not yet run end to end. First worked example due: the Macalla fork (1 extra CLAUDE.md, 26 AGENTS.md).

## Related

- `vault/2026-05-20-third-party-mcp-wireup-pattern.md`: the four-step package audit used in step 5. This entry covers the instruction-file and settings-file surface that audit does not.
- `vault/2026-10-24-promises-and-synthetic-probes-for-agents.md`: monitors the agent can't reach; the "check your own config" step is the manual form of that.

## Sources

- BleepingComputer: https://bleepingcomputer.com/news/security/ai-coding-agents-hijacked-using-agentsmd-file-in-downloads-folder
- CSA research note (2026-10-27): https://labs.cloudsecurityalliance.org/research/csa-research-note-agent-instruction-file-poisoning-20261027/
- CSO Online: https://www.csoonline.com/article/4240222/malicious-downloads-can-trick-ai-coding-agents-into-compromising-themselves.html
