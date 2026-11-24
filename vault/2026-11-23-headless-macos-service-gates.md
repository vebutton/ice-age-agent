# Headless macOS service that reads an external drive: the gates a reboot with nobody logged in exposes

**Pattern:** A service that works fine while you're logged in can fail silently after an unattended reboot, because macOS ties several things to a login session. Test the real condition (reboot, unlock, **nobody logs in**, probe from another machine) and expect to clear each gate below in turn. Each one presents as the same symptom: the server is up, but it serves nothing.

## The gates, in the order they show up

1. **FileVault: nothing on the data volume runs until the disk is unlocked.** System daemons, Tailscale included, start only after the unlock. On macOS 26 the pre-boot screen accepts SSH (Remote Login must be on *before* the reboot); it takes the login password, unlocks, and drops the connection. Plan remote reach around that: the unlock is LAN-only unless a second device (KVM, subnet router) sits on the LAN.
2. **Per-user agents only start at a GUI login.** `brew services start` (without sudo) installs a LaunchAgent that loads when the user logs in to the desktop. An SSH login doesn't count. Symptom: the reverse proxy in front answers 502. Fix: a system LaunchDaemon in `/Library/LaunchDaemons` with `UserName` set (and `HOME` in `EnvironmentVariables`), copying the agent's `ProgramArguments` and environment. Remove the agent first (`brew services stop`) so it doesn't run twice. From then on, restart with `sudo launchctl kickstart -k system/<label>`, including after package upgrades.
3. **External drives mount only at login, by default.** With nobody logged in the volume simply isn't there. Fix: `sudo defaults write /Library/Preferences/SystemConfiguration/autodiskmount AutomountDisksWithoutUserLogin -bool true`, then reboot.
4. **Privacy (TCC) refuses removable-volume access in a background session.** There's no screen to show the permission prompt, so the request is refused: the log says `Refusing TCCAccessRequest for service kTCCServiceSystemPolicyRemovableVolumes … in background session`. A prompt you accepted earlier, in a logged-in session, doesn't carry over. Fix: grant Full Disk Access (it can be added by hand, unlike the per-volume permission) to **the binary the TCC log names as responsible**.
5. **TCC attributes access to the interpreter path, not the app bundle you'd guess.** For Homebrew Python, `lsof` shows the running executable as `…/Python.framework/Versions/3.11/Resources/Python.app/Contents/MacOS/Python`, but TCC's responsible identity was `…/Versions/3.11/bin/python3.11`. Granting the `Python.app` bundle did nothing headless; granting `bin/python3.11` fixed it. Don't infer the identity: read it from the log.

## Diagnose fast

- **errno tells you which layer:** `[Errno 13] Permission denied` is ordinary file permissions; `[Errno 1] Operation not permitted` on a path you own is the privacy layer.
- **Read the TCC log:** `/usr/bin/log show --last 15m --style compact --predicate 'subsystem == "com.apple.TCC"' | grep -i -E 'deny|refus|<binary>'`. Use the full path: over SSH into zsh, a bare `log` hits a zsh builtin and fails with "too many arguments".
- **Find a renamed process's binary:** services often rename themselves (`ps` shows only the new name). `sudo lsof -a -p <pid> -d txt -Fn` gives the executable. That's the path the **firewall** wants. As item 5 shows, it may not be the path **TCC** wants.

## The maintenance cost to write down

- **Homebrew paths are versioned** (`Cellar/python@3.11/3.11.18/…`). Both the application-firewall allow and the Full Disk Access grant pin that path, so a `python@3.11` upgrade silently breaks remote reach (the firewall) and drive access (TCC). Re-grant both after any interpreter upgrade, and put that in the runbook beside the restart command.
- **Both grants cover every script run by that interpreter,** not only the service. Acceptable on a box whose other tenants live in containers; worth knowing anywhere else.
- **Keep shared caches on the internal disk** even when the models live on the external one: a reported freeze with cache and models on an external Thunderbolt drive turned out to be the drive's throughput.

## Test protocol

Reboot → unlock over SSH → **don't log in, by SSH or VNC** → from another machine, call the service's health or model-list endpoint and retry for a minute. Pass means the full list comes back, not just an HTTP 200: an empty list with a 200 is exactly what gates 3–5 look like.

## Sources

- Own setup, 2026-11-23: an M5 Ultra Mac Studio running oMLX 0.8.3 (Homebrew, python@3.11) as an inference server, with models on an external SSD; macOS 26; each gate hit and cleared live over successive headless reboots, diagnosed from the oMLX and TCC logs.

*Status: seed (single-source, observed first-hand). Promote when a second service (e.g. an agent runtime as a LaunchDaemon) confirms gates 2–5.*
