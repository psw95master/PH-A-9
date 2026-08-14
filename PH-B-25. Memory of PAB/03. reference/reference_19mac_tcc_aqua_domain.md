---
name: reference-19mac-tcc-aqua-domain
description: "On the 2019 Mac, Full Disk Access only applies in launchd's Aqua domain — SSH runs in Background and is always denied; the cerryde tmux server is a LaunchAgent so its panes inherit Aqua"
metadata: 
  node_type: memory
  type: reference
  originSessionId: d0ac12b3-2afc-4b87-91e2-7467f0106527
  modified: 2026-07-28T02:30:03.746Z
---

On the 2019 Mac (`psw95`, macOS 15.7.7), **TCC permissions only take effect in launchd's `Aqua` domain**. Verified 2026-07-28 by running the identical `node` binary, with the identical Full Disk Access grant, in both domains:

| Domain | `launchctl managername` | iCloud / Google Drive / `~/Library/Safari` |
|---|---|---|
| Aqua — LaunchAgent in `gui/501`, GUI session | `Aqua` | reads fine |
| Background — anything spawned by sshd | `Background` | `EPERM`, always |

Granting FDA to `sshd-session`, `sshd-keygen-wrapper`, `tmux`, or the accessing binary itself makes **no difference** in the Background domain. Don't spend time on it again.

**This is why `cerryde` works:** `~/Library/LaunchAgents/com.psw95.cerryde-tmux.plist` starts the tmux server in `gui/501` at login (session `_keep`, plus `exit-empty off` so the server survives with no sessions). SSH clients attach to that already-running server, and since panes are forked by the *server*, they inherit Aqua. The client being Background doesn't matter.

⚠️ **The failure mode to watch for:** if that tmux server ever dies, the next `cerryde` connection silently starts a *new* server in the Background domain, and cloud access breaks with no obvious error. **This actually happened on 2026-07-28**: an SSH connection made before GUI login created a Background server, and when the LaunchAgent ran later it found a server already on the socket, so it only added its `_keep` session to the Background one instead of creating an Aqua server.

**Diagnose by reading an iCloud path from inside the server — not by `launchctl managername`.** That reading is unreliable here: after a correct Aqua recovery it still printed `Background` while iCloud read fine. The functional test is authoritative:

```bash
tmux new-session -d -s _chk 'ls ~/Library/Mobile\ Documents/com~apple~CloudDocs > /tmp/chk.txt 2>&1'
```

**Recovery** (~10s, drops all attached sessions, so ask first):

```bash
tmux kill-server
launchctl kickstart -k gui/501/com.psw95.cerryde-tmux
```

Never start the tmux server from an SSH shell. Note the ordering trap: killing the server is required — re-running the agent while a Background server still exists does nothing useful.

The Slack bot (`com.psw95.cerry-slack-bot`) is already a `gui/501` LaunchAgent, so it has the same access — see [[cerry-slack-bot-process]].

**Reading the symptoms** — the two errors mean very different things:
- `EPERM` / "Operation not permitted" — normal TCC denial. The rule above.
- `EINTR` / "Interrupted system call", or an indefinite hang — **`tccd` is wedged**, a machine-wide fault. This happened 2026-07-26 22:40 → 2026-07-28 03:12 (~28h): tccd stayed alive but stopped answering entirely, so every TCC decision timed out via `(Sandbox) watchdog expired for approval entry`. Apple's own processes (Dock, remindd, iCloudNotificationAgent) were starving too. Diagnose with `/usr/bin/log show --last 2m | grep -c "watchdog expired"` — healthy is 0. **Only a reboot cleared it** ([[reference-19mac-remote-reboot]]); `sudo killall -9 tccd` revived the query path but not the kernel approval queue.

Note `log` is a zsh builtin that shadows `/usr/bin/log` — always use the full path.

**How to apply:** Anything a headless Cerry must read has to be reached from the Aqua tmux server or a `gui/501` LaunchAgent. Cloud storage via the *mount path* now works from there, but MCP/API access is still the more robust route ([[id-and-log-convention]]), and memory stays on a local path ([[dual-computer-memory-sync]]).
