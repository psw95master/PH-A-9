---
name: reference-19mac-power-resilience
description: "The 2019 Mac is an always-home box driven remotely, so its power settings decide whether it stays reachable — acwake=1 and disablesleep=1 are set; a sleeping or shut-down machine cannot be woken remotely"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 6b1f0d16-d1dc-4d68-8b33-6da691886364
  modified: 2026-07-29T14:10:32.266Z
---

The 2019 MacBook Pro never travels — it stays at home and is driven remotely from the 2026 Air or the user's phone. So its power settings, not its screen, decide whether it is usable.

Current configuration (`pmset -g custom`):

| | AC | Battery |
|---|---|---|
| `sleep` | `0` — never sleeps | `1` — sleeps after 1 min, conserving charge during an outage |
| `acwake` | `1` | `1` | 
| `womp` (wake on network) | `1` | `0` |

**`acwake 1` was set on 2026-07-28** (`sudo pmset -a acwake 1`) and is the piece that makes a power cut survivable: before it, an outage put the machine to sleep on battery and it stayed asleep even after power returned. Now power restoration wakes it and Tailscale reconnects on its own.

**Closing the lid used to put it to sleep, and nothing could wake it remotely.** `sleep 0` only governs the *idle* timer — it does not stop clamshell sleep, so shutting the lid dropped the machine off the tailnet (`offline, last seen Nm ago`, `tx` climbing while `rx` stays 0). There is no remote rescue from that state: Tailscale connects awake peers, it cannot ring a sleeping one, and Wake-on-LAN magic packets are LAN-only — useless whenever the 2026 Air is away from the home 172.30.1.x network. `caffeinate` does not help either; lid-close overrides its power assertions.

**Fixed 2026-07-29** with `sudo pmset -a disablesleep 1`. The flag is real but **undocumented** — absent from `man pmset` and the usage text, though still present in the `/usr/bin/pmset` binary (verified on macOS 26.5 and applied on the 2019 Mac's macOS 15.7.7). It writes `SleepDisabled = 1` into `/Library/Preferences/com.apple.PowerManagement.plist`, so it survives reboots including `fdesetup authrestart`. Undo with `... disablesleep 0`.

Amphetamine was deleted the same day: a GUI app is the wrong tool here because it needs someone logged into the desktop, so it would be absent exactly after an unattended reboot. It had never even been launched.

**Waking from sleep is unaffected by FileVault** — the disk is already unlocked, so SSH and Tailscale work even while the screen is locked. FileVault only blocks a *cold boot*.

**What still needs a human, and cannot be fixed in software:**
- A long outage drains the battery to a full shutdown. This model does **not** support `autorestart` (not in `pmset -g cap`), so it stays off until someone presses the power button — and turning FileVault off would not change that, since a person is required either way. This is why FileVault stays **on**.
- Router or ISP failure. Tailscale is the only remote path once the LAN is out of reach.

A UPS is the only real mitigation for both. Keep the machine on AC.
