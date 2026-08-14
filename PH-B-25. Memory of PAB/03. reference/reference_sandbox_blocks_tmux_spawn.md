---
name: sandbox-blocks-tmux-spawn
description: "tmux servers spawned from inside a Cerry/Claude Code session on the 2019 Mac die within a second — the sandbox blocks daemonization, so bots cannot create tmux sessions"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 00e7347f-3b7f-47d1-be5a-9498fc745316
  modified: 2026-07-26T16:49:37.515Z
---

On the 2019 Mac (`psw95`), any `tmux new-session` issued from inside a Cerry/Claude Code session — directly via Bash, via `nohup`/`disown`, or via `execFileSync` from a Node process launched by that session — **returns exit 0 but the tmux server disappears within a second**. Verified 2026-07-27 across all three invocation styles and with explicit `-S <socket>` paths.

Cause appears to be the sandbox blocking `setsid()`-style daemonization: `tmux` forks a detached server process, which is what gets killed. A plain `node index.js` backgrounded the same way survives indefinitely, because it never tries to detach into its own session.

tmux sessions the user opens themselves (`cerryde` from the 2026 Mac, which SSHes in and runs tmux outside this sandbox) are unaffected and persist for hours — see [[ssh-bridge-between-macs]].

**Why:** This killed a whole design. The plan was for the Slack bot to create empty `cerryde-slack-N` tmux sessions as visible markers so open Slack sessions would show up in `cerryde list` / `cerryde kill all`. It cannot work, no matter how the socket is configured.

**How to apply:** Never design anything that has a bot or script spawn tmux sessions on this machine. If something needs to be observable from a terminal, expose it from the already-running process instead (the Slack bot uses a `127.0.0.1` HTTP endpoint for exactly this — see [[cerry-slack-bot-process]]). Also note: tmux resolves its default socket to `/tmp/tmux-<uid>/`, **not** `$TMPDIR` — so there is only ever one default server, and "socket fragmentation" is not a real failure mode to chase.
