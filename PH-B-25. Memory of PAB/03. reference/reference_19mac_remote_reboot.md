---
name: reference-19mac-remote-reboot
description: "How to reboot the 2019 Mac remotely without losing it — `sudo fdesetup authrestart`; Tailscale is now a system daemon so it comes back without GUI login, but keep LAN 172.30.1.23 as the safety net"
metadata: 
  node_type: memory
  type: reference
  originSessionId: d0ac12b3-2afc-4b87-91e2-7467f0106527
  modified: 2026-07-27T19:04:00.894Z
---

The 2019 Mac (`psw95`) has **FileVault On and no auto-login**. A plain `reboot` stops at the pre-boot unlock screen where **nothing runs at all** — no network, no SSH, no Tailscale — until someone is physically at the keyboard.

**Safe procedure** (verified 2026-07-28, twice):

1. `sudo fdesetup authrestart` — passes the unlock key through the reboot so FileVault clears unattended. Confirm support first with `fdesetup supportsauthrestart` (returns `true`). No confirmation prompt: it reboots the instant the password is accepted.
2. It boots to the login window. `sshd`, screen sharing, **and Tailscale** are all system daemons now, so remote access returns on its own — **no GUI login needed** (confirmed with `/dev/console` owned by `root` while `ssh 19macbook` succeeded).
3. LAN safety net, in case Tailscale doesn't come back:
   ```
   ssh -i ~/.ssh/id_ed25519_19macbook psw95@172.30.1.23     # 2026 Mac is 172.30.1.32, same subnet
   vnc://172.30.1.23                                         # for GUI login
   ```

Verify `ping 172.30.1.23` from the 2026 Mac **before** rebooting. If the two machines are ever on different networks, this procedure is not safe.

**Still needs GUI login after a reboot:** the Aqua-domain LaunchAgents — `cerryde-tmux` and `cerry-slack-bot`. Tailscale/SSH being up does *not* mean Cerry is up. See [[reference-19mac-tcc-aqua-domain]].

**FileVault is the remaining gap.** This only fixes "after unlock → before GUI login". A power loss or forced shutdown still needs the password typed physically; only turning FileVault off would remove that, which the user chose not to do.

**`sudo` password entry:** the Korean IME is active by default in these SSH terminals, so a correct password gets typed as Hangul and silently fails ("Sorry, try again"). Switch to English first. Verify a password without running anything using `dscl . -authonly psw95` — silence means correct, `eDSAuthFailed` means wrong.

**How to apply:** Reboot is a real repair tool for this machine (it's what cleared the wedged `tccd` in [[reference-19mac-tcc-aqua-domain]]) — just never issue one without checking the LAN path first. Tailscale details in [[reference-19mac-tailscale-system-daemon]].
