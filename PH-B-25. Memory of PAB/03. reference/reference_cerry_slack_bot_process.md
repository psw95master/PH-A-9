---
name: cerry-slack-bot-process
description: "Cerry's Slack bot lives at ~/apps/agents-in-slack (NOT iCloud) and is managed by launchd — restart it with launchctl kickstart, never pkill"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 00e7347f-3b7f-47d1-be5a-9498fc745316
  modified: 2026-07-27T19:04:45.005Z
---

The Slack bot that lets "세리"/Cerry answer in Slack (app name "Cerry de") is a Node.js process (`index.js`, `@slack/bolt` Socket Mode + `@anthropic-ai/claude-agent-sdk`) running on the **2019 Mac** (`psw95ui-MacBookPro.local`, user `psw95`).

- **Code:** `~/apps/agents-in-slack` — a clone of GitHub `psw95master/PSW-A-17-B-4`. Deliberately **outside iCloud** (as of 2026-07-27).
- **Service:** launchd agent `com.psw95.cerry-slack-bot` (`RunAtLoad` + `KeepAlive`), so it survives reboots and crashes.
- **Secrets:** `~/.secrets/agents-in-slack.env` (outside iCloud, unchanged).
- **Log:** `/tmp/cerry-slack-bot.log`

From the 2026 Mac, `cerrycheck` reports bot/endpoint/keychain/stuck-process status in one shot and `cerryfix` clears and restarts; `slacksessions` lists and kills Slack sessions. On the 19 Mac directly:

```bash
launchctl list | grep cerry                                    # 상태
launchctl kickstart -k gui/$(id -u)/com.psw95.cerry-slack-bot   # 재시작
curl -s localhost:7391                                          # 활성 슬랙 세션 목록
curl -sX DELETE localhost:7391/<세션키>                          # 개별 세션 종료
~/apps/agents-in-slack/healthcheck.sh                           # 전체 점검
```

Sessions auto-reset past `SESSION_RESET_TOKENS` (in the env file, currently 250000) because every turn replays the whole conversation. Don't lower it to the 150000 the old bridge used — a fresh turn here already measures ~61k, so 150k cuts conversations off after two or three messages.

**Why:** The project used to live in iCloud Drive next to its Notion tree, and the bot was started by hand with `nohup`. Both were dead ends: a launchd-started process **hangs forever opening any file under `~/Library/Mobile Documents`** (verified twice — `getcwd()` then `uv_fs_open`), and the manual `nohup` only worked because it inherited a logged-in shell's iCloud access, so it would have failed exactly when auto-start was needed. Moving the code local fixed both, and incidentally removed the `node_modules` symlink workaround that kept breaking on every `npm install`.

**The keychain trap (bit us on 2026-07-27):** the bot can be running perfectly while Slack answers never arrive — every reply stuck on "생각 중". That means the login keychain is locked. `claude` reads credentials from the macOS Keychain first; under launchd (a UI-capable context) a locked keychain makes it wait forever on an unlock dialog nobody can answer, so it never even opens a network connection. From an SSH shell the same call fails instantly and it falls back to `~/.claude/.credentials.json`, which is why SSH-launched bots work and launchd-launched ones hang. **Fix: log into the 19 Mac over VNC** (`vnc://100.108.46.37`, or LAN `vnc://172.30.1.23`), which unlocks the keychain, then clear the stuck children and restart:

```bash
ps -ef | grep claude-agent-sdk | grep -v grep    # 쌓여 있으면 키체인 문제
pkill -9 -f claude-agent-sdk-darwin-x64
launchctl kickstart -k gui/$(id -u)/com.psw95.cerry-slack-bot
```

The keychain itself is set to `no-timeout`, so it never re-locks while logged in — only a reboot/logout locks it. FileVault is on, so an unattended reboot can never fully self-recover; someone has to unlock the machine regardless.

**How to apply:** Never move this project (or any launchd-run code) back under iCloud. `pkill` is useless for the bot — launchd revives it instantly, so use `kickstart` to restart after editing. If Slack Cerry goes quiet, distinguish the two failure modes first: no "생각 중" at all → process is down (`launchctl list | grep cerry`); "생각 중" forever → keychain locked. And verify any autostart change by sending a real Slack message, not just by checking that the process is up — that omission is exactly what caused the 07-27 outage. Related: [[sandbox-blocks-tmux-spawn]], [[ssh-bridge-between-macs]]. Operational guide lives in Notion `PSW-C-9 슬랙 세션 가이드`.
