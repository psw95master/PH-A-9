---
name: skill-registration-routine
description: "End-to-end routine for registering a new Crafted/Imported Skill in the AI Skill Notion DB — must always finish by mirroring the ~/.claude/skills/ symlink on BOTH Macs, not just the one the session is running on"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 0f243931-548a-440e-a26c-d85d342b752a
  modified: 2026-07-28T18:23:39.030Z
---

When the user asks to "register" a skill row in the AI Skill Notion database (see [[ai-skill-notion-db]] for the DB structure), follow this full routine, confirmed 2026-07-26:

1. **Find the source.** Open the Notion page, find its embedded GitHub link (Notion's fetch tool often can't read embed blocks as text — use `mcp__claude-in-chrome__find` on the opened page to pull the actual href).
2. **Research + fill Summary.** Fetch the repo (WebFetch or clone), write a one-line Korean summary into the page's `Summary` property.
3. **Check scope before installing.** Many "single skill" repos are actually multi-skill bundles with installers that add hooks/MCP servers/multi-tool config. Ask the user how much to install — default answer so far has been **flagship `SKILL.md` only**, no hooks/MCP/installer script (see [[ai-skill-notion-db]]).
4. **Place the real file in iCloud**, not locally — AI Skill 폴더 밑의 Crafted / Imported 갈래 중 맞는 쪽에 `AIS-{ID}_{name}/SKILL.md` 로. **경로는 기억에서 꺼내 쓰지 말고** `cerry-link list` 로 기존 스킬이 어디를 가리키는지 보고 그 옆에 만들 것 (폴더명 개편 중).
5. **Symlink `~/.claude/skills/AIS-{ID}_{name}`** to that iCloud folder — on whichever Mac the current session is running on.
6. **Write the Notion page body** in the fixed shape: `## 개요` → `## 저장 위치` → `## 동작 흐름` (Crafted) or `## 설치·사용법` (Imported) → `## 참고`.
7. **Mirror step 5 on the OTHER Mac before calling the task done.** This is the point the user explicitly emphasized: Cerry's skills (like Cerry's memory, [[dual-computer-memory-sync]]) must stay identical on the 2019 MacBook Pro and the 2026 MacBook Air — not just synced in iCloud content, but actually symlinked into `~/.claude/skills/` on both. iCloud sync alone does NOT create the local symlink on the other machine.
8. **Use the SSH bridge to do step 7 directly** ([[ssh-bridge-between-macs]]: `ssh mba26 '...'` reaches the 2026 Air, user `perrykim` there vs `psw95` on the 2019 MBP) instead of just telling the user to go do it themselves on the other machine. Only fall back to manual instructions if `ssh mba26 echo ok` fails (machine offline/unreachable).
9. Verify both sides end up listing the same set of skills in `~/.claude/skills/` (`ls -la` both, via SSH for the remote one).

**Why:** The user runs Cerry on two Macs in parallel and treats it as one continuous assistant — a skill installed on only one machine breaks that continuity, the same failure mode already tracked for settings.json in [[reference-settings-json-not-synced]].

**How to apply:** Treat "register a skill" as incomplete until BOTH machines' `~/.claude/skills/` symlinks match, every time, without needing to be asked separately for the second machine.

**Naming note (confirmed 2026-07-26):** Keep local symlinks named `AIS-{ID}_{name}` even when it differs from the SKILL.md frontmatter `name:` (e.g. folder `AIS-4_caveman` vs frontmatter `name: caveman`). This makes the slash command display as `/AIS-4_caveman` instead of the clean `/caveman`, but the user confirmed this is purely cosmetic — the frontmatter `name`/`description`/body are read and applied in full regardless of folder name, so skill behavior is 100% unaffected. Don't rename the symlink to "clean up" the display name unless the user asks again.
