---
name: cerry-link-symlink-recovery
description: 세리 심링크(기억 1 + 스킬 4)가 iCloud 폴더 rename으로 끊겼을 때 cerry-link fix로 복구 — 경로가 아니라 내용 표식으로 찾는다
metadata: 
  node_type: memory
  type: reference
  originSessionId: 261575ab-c160-4bf7-b0f7-cbbc5991f7ea
  modified: 2026-08-13T15:48:21.521Z
---

`~/.claude`의 스킬 심링크는 전부 iCloud 최상위 폴더 밑을 가리킨다. 그 폴더 이름을 바꾸면 **전부 동시에 조용히 끊어진다.** (`memory` 는 260729에 심링크를 걷어내고 로컬 실폴더가 됐다 — [[device-split-memory]].)

- `cerry-link list` — 연결 상태 보기
- `cerry-link fix` — 끊어진 링크를 iCloud에서 다시 찾아 연결. 경로가 아니라 **내용 표식**으로 찾기 때문에 폴더명·구조가 전부 바뀌어도 복구된다 (기억=`MEMORY.md` 보유 폴더, 스킬=`SKILL.md`의 `name:` 값). 표식은 `~/.claude/cerry-links.tsv`에 기록
- `cerry-link save` — 현재 정상 상태를 표식으로 기록 (링크가 성할 때만)
- `cerry-guard` — SessionStart 훅. 기억을 `~/.claude/memory-snapshot/`에 스냅샷 + 목차 점검. 260728 설치

**⚠️ 스킬 4개가 260730부터 끊긴 채로 남아 있다 (260814 확인).** 폴더 개편 중 경고가 소음이어서 페리 지시로 `cerry-guard` 의 심링크 점검을 걷어냈고, 끊어져 있던 스킬 심링크 4개도 같이 지웠다. 그래서 **`~/.claude/skills/` 는 지금도 비어 있고** `save-log`·`ponytail`·`caveman`·`design-taste-frontend` 를 이 맥에서 쓸 수 없다. 명단(`~/.claude/cerry-links.tsv`)은 4줄 그대로 남아 있다.

**`fix` 는 명단을 읽어 링크를 새로 만드는 구조라 링크가 없어도 정상 동작한다.** 반면 **`check` 는 실제 심링크만 훑으므로 항상 "이상 없음"이다** — 스킬이 통째로 없는 지금 상태를 정상으로 오인한다. 되살리려면 `fix` → `list` 로 4개 확인 → `save`, 그리고 `~/.local/bin/cerry-guard` 에 `"$HOME/.local/bin/cerry-link" check` 한 줄을 되돌려 넣는다. 백업: `~/.claude/backups/symlink-removal-260730/`

**한계**: `fix`는 iCloud 안만 뒤진다. 기억/스킬 폴더를 iCloud 밖으로 옮기면 못 찾는다 — 그때는 스냅샷에서 되살린다.

스킬 폴더명과 `SKILL.md`의 `name:`은 서로 다르다 (예: 폴더 `AIS-3_ponytail` / name `ponytail`). 그래서 표식 기록이 필요하다.

**폴더명 개편 중에도 이 도구가 답이다 (260729).** 이름이 어떻게 바뀌든 `cerry-link fix` 는 내용 표식으로 다시 찾아낸다. 개편 직후 링크 상태를 한 번 점검하고, 정상이면 `cerry-link save` 로 새 상태를 박아둘 것.

관련: [[device-split-memory]]
