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
- Honest limit: on the same macOS user, the Claude Code login and the `gh` keyring login are still on the machine. For anything you will *run* (not just read), use a container or VM (the OpenCode `:ro,Z` container shape) or a separate macOS user. **Vince's setup is the container below.**

## Container setup (decided 2026-11-08: podman on the M4, Claude does the reading)

Why Claude rather than local Qwen: judging the intent of prose ("is this instruction aimed at the agent?") is the core of the pass, and a 9B is weak at it. The cost is that a Claude credential enters the container, and that credential is exactly what the measured attack steals. Hence three walls.

**Wall 1: a dedicated podman machine that sees only the drop folder.**
- On macOS the default podman machine mounts all of `/Users` into its VM. Create a separate one that mounts only a drop folder:
  `podman machine init trustpass --volume $HOME/trust-pass:$HOME/trust-pass`
- Check with `podman machine inspect trustpass` that `/Users` is **not** in the mounts (passing `--volume` should replace the default; verify, don't assume).
- Podman on macOS runs one machine at a time: stop the OpenCode machine first, select this one with `--connection trustpass` or `podman system connection default trustpass`.
- **Prepare the repo on the host, before the pass:** clone into `~/trust-pass/<repo>`, add the upstream remote and `git fetch upstream`. The container then needs no GitHub access, and step 3 (diff against upstream) works offline. Fetching doesn't run repo content.

**Wall 2: no reusable credential inside.**
- Don't mount `~/.claude`, `~/.claude.json`, `.env.local`, `~/.ssh` or anything with a `gh` login. Container `HOME` starts empty each run (`--rm`).
- Authenticate with a token from `claude setup-token` on the host (stays on the subscription, not API billing), passed in as `CLAUDE_CODE_OAUTH_TOKEN`. **Revoke it after each pass** (or each batch of passes).
- Set `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1` so the allow-list below can stay at one host.

**Wall 3: outbound traffic to `api.anthropic.com` only.** This is the wall that defeats the measured attack: a hijacked `ANTHROPIC_BASE_URL` has nowhere to send the token.
- Preferred shape: put the Claude container on an **internal** podman network (`podman network create --internal tp-internal`, no route out), plus a small proxy container on both that network and the default one, allowing only `api.anthropic.com:443`. Point the Claude container at it with `HTTPS_PROXY`. Unsetting the proxy variable gets the agent nothing, because there is no other route.
- Weaker alternative: iptables inside the Claude container (Anthropic's reference devcontainer `init-firewall.sh` does default-deny + allowed domains). It needs `NET_ADMIN`, so the container could undo its own rules. Use it only if the proxy shape is too fiddly, and drop the capability after the rules load.
- If Claude Code fails to connect, read the proxy log for the blocked host **before** widening the list, and record any host you add.

**Running the pass inside:**
- Mount the target repo read-only: `-v $HOME/trust-pass/<repo>:/target:ro,Z`. Launch `claude` from an empty working dir (e.g. `/work`), never from `/target`, and give it `/target` as a path.
- Image needs: `git`, `ripgrep`, `perl` (for the hidden-character grep), Node + Claude Code pinned to a known version.
- Inside the box, permission prompts are not the defence; the walls are. Still don't grant it a writable mount.

**After the pass, before the container is discarded:**
- Look at the container's own `~/.claude/settings.json` and `~/.claude.json`. If anything changed there (base URL, `apiKeyHelper`, new `mcpServers`), the repo tried a hijack: that is itself the verdict (*don't use*) and evidence worth keeping.
- Revoke the setup token. Check the proxy log for refused hosts: a refused attempt to reach an unknown host is a finding.

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

*seed (single incident + controlled tests + CSA framing).* Container setup added 2026-11-08; Vince is building it in a separate trust-pass project. Not yet run end to end. First worked example due: the Macalla fork (1 extra CLAUDE.md, 26 AGENTS.md).

## Related

- `vault/2026-05-20-third-party-mcp-wireup-pattern.md`: the four-step package audit used in step 5. This entry covers the instruction-file and settings-file surface that audit does not.
- `vault/2026-10-24-promises-and-synthetic-probes-for-agents.md`: monitors the agent can't reach; the "check your own config" step is the manual form of that.

## Sources

- BleepingComputer: https://bleepingcomputer.com/news/security/ai-coding-agents-hijacked-using-agentsmd-file-in-downloads-folder
- CSA research note (2026-10-27): https://labs.cloudsecurityalliance.org/research/csa-research-note-agent-instruction-file-poisoning-20261027/
- CSO Online: https://www.csoonline.com/article/4240222/malicious-downloads-can-trick-ai-coding-agents-into-compromising-themselves.html
