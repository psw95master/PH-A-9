---
name: ai-skill-notion-db
description: "Structure and conventions for the AI Skill Notion database (Crafted Skill vs Imported Skill) and how imported multi-skill GitHub bundles get installed"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 0f243931-548a-440e-a26c-d85d342b752a
  modified: 2026-07-28T19:03:15.659Z
---

User maintains the **AI Skill** Notion database (data source `collection://3a9efc56-b3a1-80a1-b7df-000ba3c17fa1` — 이름이 바뀌어도 이 ID 는 그대로다) with a `Type` select property: **Crafted Skill** (Cerry-authored, e.g. `save-log`/AIS-1) vs **Imported Skill** (external GitHub tools, e.g. `caveman`/AIS-4, `taste-skill`/AIS-5, `ponytail`/AIS-3). Each row's page body follows a fixed shape: `## 개요` → `## 저장 위치` → `## 동작 흐름` (Crafted) or `## 설치·사용법` (Imported) → `## 참고`.

Inside, rows are split into a Crafted group and an Imported group. **폴더·DB 이름은 개편 중이므로 여기서 꺼내 쓰지 말 것** — 기억에 남은 이름을 믿지 말고 그때그때 현물(Notion DB, 실제 폴더)을 확인한다.

**이 맥(19맥)의 스킬은 `~/.claude/skills/` 안의 실제 로컬 파일이다.** 26맥에서 만들어 내려받는 구조라 여기서 고치지 않는다 ([[device-split-memory]]).

**AIS-6 `frontend-design` is the deliberate exception** — it runs as a *plugin* (`frontend-design@claude-code-plugins`, installed on both Macs), not as a file-based skill. **AIS-6 를 파일 스킬로 따로 두지 말 것** — 같은 스킬이 두 벌 생기고, 그게 2026-07-27 에 마켓플레이스 중복으로 한 번 치웠던 상태다. Anthropic auto-updates the plugin, and the customizable frontend skill in this workspace is already AIS-5 `taste-skill`, so there is no reason to own AIS-6. Decided 2026-07-28.

Imported Skill entries on GitHub are often large multi-skill bundles (caveman: 7 sub-skills + multi-tool installer; taste-skill: 13 sub-skills; ponytail: 6 sub-skills + hooks + its own MCP server). Asked the user whether to run the official installer (hooks/MCP/multi-tool config, bigger blast radius) vs. copy just the one flagship `SKILL.md` matching the repo name into the `~/.claude/skills/AIS-N_name/` slot — user chose the **flagship-file-only** option, no hooks/MCP/other-tool installers.

**Why:** Full installers touch global config across multiple AI tools (Codex, Cursor, etc.) and add persistent hooks — much bigger and less reversible than the single-file save-log pattern the database was designed around.

**How to apply:** When adding a new Imported Skill row in the future, default to installing only the one representative `SKILL.md` (same name as the repo where possible) rather than running the project's own installer script — confirm with the user first if the repo structure is ambiguous about which sub-skill is the "main" one. Keep the Notion page body in the same 4-section shape as existing rows.
