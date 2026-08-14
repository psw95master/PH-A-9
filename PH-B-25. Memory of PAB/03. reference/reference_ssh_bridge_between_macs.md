---
name: ssh-bridge-between-macs
description: "Working SSH route from the 2019 MacBook Pro to the 2026 MacBook Air over Tailscale, usable to apply changes on the other machine directly instead of asking the user to do it manually"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 0f243931-548a-440e-a26c-d85d342b752a
  modified: 2026-07-27T19:04:38.389Z
---

The user's two Macs are reachable from each other over Tailscale. From this machine (2019 16" MacBook Pro, hw.model `MacBookPro16,1`, Tailscale name `psw95-macbookpro-1` since 2026-07-28 — the old `psw95-macbookpro` node is dead but still holds the name), the 2026 MacBook Air is reachable via the SSH config alias `mba26` (defined in `~/.ssh/config`: `HostName 100.89.64.58`, `User perrykim`, key-based auth already trusted — `ssh mba26 '<command>'` just works, no password).

Local username differs per machine: this machine's user is `psw95`, the 2026 Air's user is `perrykim` — always account for this when constructing paths (e.g. iCloud Drive is at `/Users/psw95/Library/Mobile Documents/...` here but `/Users/perrykim/Library/Mobile Documents/...` there).

**Reverse direction confirmed 2026-07-27:** from the 2026 Air, the alias `19macbook` reaches the 2019 MacBook Pro (`~/.ssh/config` there: `HostName 100.108.46.37`, `User psw95`, `IdentityFile ~/.ssh/id_ed25519_19macbook`) — also works passwordlessly. The IP changed from `100.116.55.41` on 2026-07-28 when Tailscale became a system daemon ([[reference-19mac-tailscale-system-daemon]]); LAN `172.30.1.23` also reaches it. On the 2019 Mac, non-interactive SSH sessions don't source the shell profile, so the `claude` CLI isn't on `$PATH`; it must be called by full path, `~/.local/bin/claude` (symlink into `~/.local/share/claude/versions/<ver>`), e.g. `ssh 19macbook '~/.local/bin/claude plugin list'`.

**Why this matters:** Confirmed on 2026-07-26 (see [[ai-skill-notion-db]]) — when a task needs to be applied to both Macs (e.g. creating a `~/.claude/skills/` symlink), this SSH bridge lets it be done directly on the 2026 Air in the same session, rather than telling the user to go run commands there themselves.

**How to apply:** Before telling the user "you'll need to do X on the other Mac yourself," check first whether `ssh mba26 '<command>'` can just do it. Reachability isn't guaranteed every session (the other Mac must be online/awake) — if `ssh mba26 echo ok` fails, fall back to giving the user the command to run there themselves. Also see [[reference-settings-json-not-synced]] — this bridge may make similar pending cross-machine syncs actionable directly.
