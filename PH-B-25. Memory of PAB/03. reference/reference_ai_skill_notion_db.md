---
name: ai-skill-notion-db
description: "Structure and conventions for the AI Skill Notion database (Crafted Skill vs Imported Skill) and how imported multi-skill GitHub bundles get installed"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 0f243931-548a-440e-a26c-d85d342b752a
  modified: 2026-07-28T18:23:59.383Z
---

User maintains the AI Skill Notion database (data source `collection://3a9efc56-b3a1-80a1-b7df-000ba3c17fa1`) with a `Type` select property: **Crafted Skill** (Cerry-authored, e.g. `save-log`/AIS-1) vs **Imported Skill** (external GitHub tools, e.g. `caveman`/AIS-4, `taste-skill`/AIS-5, `ponytail`/AIS-3). Each row's page body follows a fixed shape: `## 개요` → `## 저장 위치` (real path + iCloud origin + symlink note) → `## 동작 흐름` (Crafted) or `## 설치·사용법` (Imported) → `## 참고`.

On disk: the real file lives in iCloud and `~/.claude/skills/AIS-{ID}_{name}` symlinks to it. **폴더 이름은 여기 적지 않는다 (2026-07-29 기준 개편 중).** **Don't hardcode these paths** — resolve them with `cerry-link list`, and run `cerry-link fix` if a link broke ([[cerry-link-symlink-recovery]]).

**The 2019 Mac no longer symlinks skills (2026-07-28)** — it holds real local copies, pushed one-way from the 2026 Mac by `cerry-skills`, so an iCloud fault can't cost it every skill at once. Only the 2026 Mac symlinks into iCloud.

**AIS-6 `frontend-design` is the deliberate exception** — it runs as a *plugin* (`frontend-design@claude-code-plugins`, installed on both Macs), not as a file-based skill. Do **not** symlink the iCloud copy into `~/.claude/skills/`: that would produce two copies of the same skill, which is the duplicate cleaned up on 2026-07-27 ([[anthropic-marketplace-duplicate]]). Anthropic auto-updates the plugin, and the customizable frontend skill in this workspace is already AIS-5 `taste-skill`, so there is no reason to own AIS-6. The iCloud copy is backup only. Decided 2026-07-28.

Imported Skill entries on GitHub are often large multi-skill bundles (caveman: 7 sub-skills + multi-tool installer; taste-skill: 13 sub-skills; ponytail: 6 sub-skills + hooks + its own MCP server). Asked the user whether to run the official installer (hooks/MCP/multi-tool config, bigger blast radius) vs. copy just the one flagship `SKILL.md` matching the repo name into the `~/.claude/skills/AIS-N_name/` slot — user chose the **flagship-file-only** option, no hooks/MCP/other-tool installers.

**Why:** Full installers touch global config across multiple AI tools (Codex, Cursor, etc.) and add persistent hooks — much bigger and less reversible than the single-file save-log pattern the database was designed around.

**How to apply:** When adding a new Imported Skill row in the future, default to installing only the one representative `SKILL.md` (same name as the repo where possible) rather than running the project's own installer script — confirm with the user first if the repo structure is ambiguous about which sub-skill is the "main" one. Keep the Notion page body in the same 4-section shape as existing rows.
