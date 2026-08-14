---
name: reference-settings-json-not-synced
description: "~/.claude/settings.json is a plain local file on each Mac and is never synced — any settings change must be applied to both machines by hand"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 6b1f0d16-d1dc-4d68-8b33-6da691886364
  modified: 2026-07-28T02:57:57.890Z
---

`~/.claude/settings.json` is a **plain local file on each machine**. Unlike the memory folder ([[dual-computer-memory-sync]], which is iCloud-symlinked on the 2026 Mac and rsync'd to the 2019 Mac), it is not synced by anything. A permissions, hooks, or env change made on one Mac has **no effect on the other**.

**As of 2026-07-28 the two `permissions` blocks are identical** — same `allow` (Read/Glob/Grep/Bash/Edit/Write/NotebookEdit + the claude-in-chrome tools) and same `deny`. Verified by diffing both.

Other keys deliberately differ, so never blindly copy the whole file:
- the 2026 Mac carries the `cerry-mem sync` hooks on `SessionStart`/`SessionEnd` and the orca `agent-hooks` entries; the 2019 Mac has no `hooks` block at all
- paths inside hook commands are user-specific (`/Users/perrykim/...` vs `/Users/psw95/...`)

**The `deny` list is what makes the broad `allow` safe.** `Bash` is allowed wholesale, which is only prudent because `Bash(rm -rf*)`, `Bash(sudo *)`, force-push and hard-reset patterns are denied. If you ever copy the allow list to a new machine, copy the deny list with it — allow alone is strictly more dangerous than the reference setup.

**How to apply:** When changing settings.json for a reason that isn't machine-specific, apply it to both Macs in the same session — the 2019 Mac is reachable at `ssh 19macbook` ([[ssh-bridge-between-macs]]). Back up first (`cp settings.json settings.json.bak-YYMMDD`), merge with `json.load`/`json.dump` rather than overwriting, and verify with `python3 -m json.tool` — a malformed settings.json silently disables every setting in the file.
