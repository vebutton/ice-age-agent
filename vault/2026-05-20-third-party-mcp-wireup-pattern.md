# Wiring a third-party MCP into Claude Code — env-var token, version pin, audit-then-trust

**Pattern:** When wiring a community / third-party MCP server into a Claude Code project, follow a four-part discipline that keeps secrets out of the repo and the trust surface narrow:

1. **Token in `.env.local` (gitignored), referenced from `.mcp.json` via `${VAR}`.** Never inline secrets in `.mcp.json` — that file is committed. `.mcp.json` only ever sees the variable name; the value lives outside the repo.
2. **Commit a `.env.local.example` template** with placeholder values, so a fresh clone can recreate the setup without spelunking history.
3. **Launch with `source .env.local && claude`** from the project root. The `source` is required — `.env.local` isn't auto-loaded, and without it the MCP subprocess gets no token and silently fails (often surfacing as a generic "failed to connect").
4. **Pin the MCP package version explicitly in `.mcp.json` — never `@latest`.** And do a one-time audit before granting the MCP a real credential.

**Why it matters:**

- **Token hygiene.** A bot/API token committed to a public repo is game-over even on a private repo where the host is compromised. The `${VAR}` indirection plus gitignored `.env.local` makes this a non-event: nothing sensitive ever enters git history.
- **Reproducibility for collaborators / future-you.** The `.env.local.example` template is the durable form of the setup instructions. Without it, every fresh checkout becomes an archaeology project.
- **Version pinning is the difference between trust and continuous trust.** `@latest` means npm pulls the newest published version every launch — any maintainer compromise or malicious release lands in your process immediately. A pinned version is sticky (npm doesn't allow same-version republish after 72h), so a one-time audit retains its validity until *you* choose to upgrade.
- **Audit-then-trust** because a Slack-or-equivalent token in the wrong process can read months of company conversation. The bar isn't "is this maintainer well-known" — it's "have I checked what this code actually does with my token."

**The four-step audit (do once per pinned version):**

1. **Token-handling review:** Find every place the env-var token is read. Confirm it goes only to the intended API (Slack, GitHub, etc.), never to disk, never to a log line in plain text, never to any other URL.
2. **Outbound endpoints:** List every network host the package contacts. Anything beyond the intended API (analytics, telemetry, update-checkers) is a red flag.
3. **Dependencies + scripts:** Skim `package.json` for unrelated deps (random HTTP clients, crypto, scrapers) and **any `preinstall` / `postinstall` scripts** — these run on every install before any code review can intervene.
4. **Maintainer signal:** Solo abandoned vs. active community matters less than maintainer responsiveness — recent releases, dependabot wired up, multiple recent contributors are positive signals.

**Sharp caveat — source audit only matters if binary == source:**

Many MCP packages ship a *compiled artifact* (a Go binary, a bundled JS blob, a Rust executable) downloaded as a platform-specific optional dependency, with the public source on GitHub being separate. In that case the source review only validates the artifact if you can verify the published binary was built from that source — usually requires SLASA provenance, signed releases, or reproducible builds, which most community MCPs **don't have**. Mitigations when verification isn't available:

- Pin the version (catches future maintainer compromise via npm's same-version-republish lockout).
- Record the package integrity hash (`shasum`, `sha512`) at audit time so a republish-without-version-bump would be detectable.
- Treat the binary as semi-trusted, not fully trusted. Run with reduced privileges if practical (no filesystem write outside an explicit cache dir; no network beyond expected endpoints — caught at the network-observation step).
- One-time network observation (`sudo lsof -i -P | grep -i <process>`, Little Snitch, or equivalent) during a normal session confirms the running binary matches the audited source's behavior at the network layer.

**Worked example — Slack MCP (`korotovsky/slack-mcp-server`):**

- Version pinned to `1.3.0` in `.mcp.json` (audited 2026-05-20). npm tarball sha512 recorded with the audit notes.
- Token (`SLACK_MCP_XOXB_TOKEN`, an `xoxb-...` bot token) lives in gitignored `.env.local`, referenced from `.mcp.json` as `${SLACK_MCP_XOXB_TOKEN}`.
- `.env.local.example` committed with `xoxb-REPLACE-ME` placeholder.
- Audit findings: single env-var read; token transmitted only to `*.slack.com/api/` and `edgeapi.slack.com/cache/`; no analytics; no postinstall scripts at the npm-package level; active maintainer (~60 contributors over the last year, recent releases). `utls` (TLS-fingerprint-spoofing) dep present but opt-in only via `SLACK_MCP_CUSTOM_TLS` env var — off by default.
- Caveats: npm package is a Node launcher around a downloaded Go binary; no SLSA provenance, so source review's validity depends on the binary actually being built from the audited source. Mitigations above apply.
- Slack-specific gotcha worth recording: even when targeting a single public channel, all five `*:read` scopes (`channels:history`, `channels:read`, `groups:read`, `im:read`, `mpim:read`) must be granted to the bot — the MCP server enumerates all four channel types at boot, and any missing scope fails the whole startup with `missing_scope`. Granting the scopes does not widen what the bot can read (still gated by per-channel invite); it just lets the boot-time enumeration succeed.
- Scope discipline: bot is invited to exactly one channel. Adding channels is a deliberate per-channel action, never a default.

**Verification log — Slack MCP, audited 2026-05-20:**

- **Source review** (full report in the session that captured this entry): clean for our use case — single env-var read of `SLACK_MCP_XOXB_TOKEN`, token never written to disk, transmitted only as `Authorization: Bearer` to Slack; no analytics / telemetry / update-check beacons; no postinstall scripts; active maintainer (~60 contributors in the last year, recent release cadence). `utls` (TLS-fingerprint-spoofing) dependency present but opt-in only via `SLACK_MCP_CUSTOM_TLS` env var — off by default.
- **Binary identification:** the npm package is a Node launcher that downloads a platform-specific Go binary as an optional dependency. On macOS arm64 the binary lives at `~/.npm/_npx/<hash>/node_modules/slack-mcp-server-darwin-arm64/bin/slack-mcp-server-darwin-arm64` (16 MB Mach-O arm64).
- **Binary integrity (capture-time hash):** `sha256 = 8f0ba1fff09d61d51a090ef447995f778c0fd7234d6f1d6c568cb2fba0e7da83`. If this changes without a version bump on `.mcp.json`, that's a republish-without-version-change attempt and warrants re-audit before next launch.
- **Network observation (live, during an actual `mcp__slack__conversations_history` call from a Claude Code session):** the Go binary (PID confirmed via `pgrep -fl slack-mcp-server`; net traffic *only* on the binary PID, never the Node wrapper) opened a single short-lived HTTPS connection to `18.169.61.189:443` (AWS `eu-west-2`). Verification that this is Slack-owned:
  1. TLS handshake against the IP with `SNI=edgeapi.slack.com` returned a Let's Encrypt-issued cert with `CN=slack.com`, SAN `*.slack.com, slack.com`. TLS handshake completion requires the private key, which Let's Encrypt only issues to a controller of `slack.com` DNS — so the IP is operated by Slack.
  2. Forward DNS: `slack-files.com` resolves to `18.169.61.189` (Slack publishes this exact IP).
- **Slack runs on AWS**, so legitimate Slack traffic always shows up in `lsof` / `nettop` reverse-DNS as `*.compute.amazonaws.com`. The SNI/Host the binary requested is the meaningful identifier, not the PTR record. Don't be alarmed by an AWS-looking PTR; *do* verify with a TLS-cert check (above) if you want certainty.
- **Footnote — the binary's network surface is broader than the audited source enumerated.** The source review said endpoints would be `*.slack.com/api/` and `edgeapi.slack.com/cache/`. The captured connection looked more like a `slack-files.com` / shared-edge endpoint. Stayed inside Slack, but illustrates that empirical network observation catches gaps a source review alone misses — exactly why this step is part of the audit, not optional.

**Where this came from:**

- The how-to source `output/howto-claude-code-slack-readonly.md` (Sage session, 2026-05-19) is the step-by-step form of this for the Slack MCP specifically.
- The audit discipline + binary-vs-source caveat came from this session's hardening pass on the Slack MCP (2026-05-20), prompted by [[project-mcp-build-vs-buy-stance]] — once we'd decided to lean on community MCPs for 3rd-party apps, the question became *how* to do that responsibly. The audit was run by an agent against `korotovsky/slack-mcp-server@1.3.0` and surfaced the binary-distribution caveat that the source review alone wouldn't catch.

**How to apply:**

- **In this repo:** the discipline is now applied to the Slack MCP. Re-run the four-step audit when bumping to a future pinned version (don't just bump and pray).
- **In kickstart packs Sage produces:** seed the same pattern in the new repo's `.mcp.json` + `.env.local.example` skeleton and include a short "audit before trusting" note in the seeded `CLAUDE.md`. Don't hand new projects an `@latest` shape — that bakes the wrong default in from day one.
- **Template-upgrade candidate:** add `.env.local.example` + `${VAR}`-referenced `.mcp.json` + the audit-checklist note to `claude-agent-template`, so every child repo bootstraps with the right defaults.
- **General principle:** any time the project takes a credential dependency on third-party code, the steps are: env-var the secret, gitignore the values, pin the dependency version, do the four-step audit, record the integrity hash. Cost is ~30 minutes; the alternative is unbounded trust in a moving artifact that holds your token.

**Status:** seed (will promote to `promoted-to-template` once the template-upgrade PR lands)

Related: [[project-mcp-build-vs-buy-stance]] (which MCPs to build vs. buy in the first place — this entry is *how to wire the ones you buy*). [[2026-05-20-claude-md-stays-lean-lazy-loading]] (CLAUDE.md should *not* enumerate MCP tools — let the harness lazy-load — but it *should* note runtime gotchas like the `source .env.local && claude` launch requirement).
