---
name: reference-19mac-tailscale-system-daemon
description: "The 2019 Mac reaches Tailscale only through Homebrew tailscaled as a system daemon; its IP is 100.108.46.37 and the conflicting App Store app has been removed"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 6b1f0d16-d1dc-4d68-8b33-6da691886364
  modified: 2026-07-28T02:52:58.316Z
---

Since 2026-07-28 the 2019 Mac reaches Tailscale through **Homebrew's open-source `tailscaled` running as a system daemon**, so remote access survives a reboot without anyone logging into the GUI. This matters because the machine lives at home permanently and is driven remotely from the 2026 Air or the user's phone.

```
/Library/LaunchDaemons/com.tailscale.tailscaled.plist   domain = system, runatload
/usr/local/bin/tailscaled                                the daemon
/usr/local/opt/tailscale/bin/tailscale                   the CLI — use this full path
/Library/Tailscale/tailscaled.state                      auth state, root-owned, survives reboot
```

**Its IP is `100.108.46.37`** (was `100.116.55.41` under the old setup). The 2026 Mac's `~/.ssh/config` alias `19macbook` points there. The tailnet node is still named `psw95-macbookpro-1` — the `-1` is a leftover from when the old node held the name; harmless, and renameable in the admin console since the old node is now deleted.

**The App Store `/Applications/Tailscale.app` was removed on 2026-07-28, and must not be reinstalled.** While it was present it auto-started on every GUI login and stole the tunnel from the daemon: SSH to the daemon's IP timed out and the dead node lit up green in the device list. Its login item was registered through SMAppService, so it did **not** appear in the classic `System Events → login items` list — that check gave a false all-clear. If Tailscale ever misbehaves this way again, look for the app with `pgrep -lf "Applications/Tailscale.app"` and quit it via `osascript -e 'quit app "Tailscale"'`, which restores the daemon within seconds.

**The lesson worth keeping:** while both were running, `tailscale status` looked perfectly healthy even though the daemon had *no auth of its own* — the connection was riding on the App Store extension, and `tailscale up` exited 0 with no login URL, which read as success. It only surfaced on reboot as `Logged out.` **With two implementations of anything on one machine, a green status check proves nothing; verify with the other one fully down.**

App Store builds can't be daemonized at all (`_MASReceipt` present = sandboxed, user-session only), and auto-login isn't offered while FileVault is on — which is why the Homebrew route was the only option. See [[reference-19mac-remote-reboot]] for reboot procedure and [[reference-19mac-power-resilience]] for the power settings that keep it reachable.
