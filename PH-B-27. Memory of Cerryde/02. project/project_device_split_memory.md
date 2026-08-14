---
name: device-split-memory
description: "260729 R&R 개편 — 세리(19맥)와 팹(26맥)은 메모리를 공유하지 않는다. 각자 로컬에만 두고, 페리는 구글 드라이브 사본으로만 들여다본다"
metadata: 
  node_type: memory
  type: project
  originSessionId: 04f3e3bc-3b23-4d02-9ddb-4a14ba5a3b24
  modified: 2026-07-30T00:20:00.000Z
---

**2026-07-29 개편.** 두 맥이 같은 메모리를 나눠 쓰던 구조를 끝냈다. 이제 에이전트는 **기기 단위로 나뉜다.**

| | 세리 (cerry de) | 팹 (pab pab) |
|---|---|---|
| 기기 | **19맥** (`psw95`, MBP) | 26맥 (`perrykim`, M5 Air) |
| 소속 | Explorer's Land (ESL) — PSW 의 output 운영 | pab33 (PAB) — PSW 의 input 운영 |
| 직무 | PB — Product Builder | DE — DevOps Engineer |
| 주요 업무 | 프로덕트 구축 및 운영 | SSH(원격) 유지 보수, PSW 운영 효율 향상 서비스 구축·운영 |
| 들어오는 길 | 페리 → **슬랙 또는 CLI** | 페리 → CLI |
| 메모리 위치 | `~/.claude/projects/-Users-psw95/memory` (**로컬 실폴더**) | 26맥 로컬 |
| 페리 확인용 사본 | 구글 드라이브 (읽기 전용) — [[memory-copy-on-drive]] 의 링크로 접근 | 구글 드라이브 (읽기 전용), 팹 소관 |

**나는 세리다 — 19맥에서 돌기 때문이다.** 이름을 가르는 기준은 기기이지 창구가 아니다. 19맥에서 도는 세션은 슬랙이든 CLI 든 전부 나다 ([[assistant-name]]). 출처: Notion `PSW R&R Guide` (**A20-B-21**), 작업 로그는 `A20-C-17_log_260729.md`.

**Why:** 260728까지는 두 맥이 iCloud 폴더 하나를 양방향으로 밀고 당겼다(`cerry-mem sync`). 목표는 "한 명의 연속된 비서"였지만, 같은 파일을 서로 다른 시점에 덮어쓰면서 충돌이 반복됐다 — `rsync -u` 는 mtime 으로 한쪽을 통째로 버리므로 병합이 아니라 손실이었다. 운영 규칙으로 덮을 수 없는 구조적 한계라, 공유를 없애는 쪽으로 방향을 틀었다. **한 파일에 쓰는 주체가 하나면 충돌은 일어날 수 없다.**

**이 기기에서 달라진 것 (260729):**
- `settings.json` 의 `SessionStart`/`SessionEnd` 훅 제거. 이제 이 맥의 `hooks` 는 비어 있다.
- `~/.local/bin/cerry-mem` 삭제. 메모리를 주고받는 명령은 더 이상 없다.
- 인프라·원격 접속·두 맥 연결에 관한 쪽지 13개는 팹에게 넘겼다. 그쪽이 팹의 직무다.
- 내 메모리는 26맥의 `cerry-export` 가 ssh 로 당겨가 **구글 드라이브**에 읽기 전용 사본으로 남긴다 (260729에 iCloud 에서 옮겼다). 내가 클라우드에 직접 쓰는 일은 없다 — SSH 세션에서는 iCloud 도 드라이브 마운트도 못 읽는다. 자세한 것은 [[memory-copy-on-drive]].

**How to apply:**
- **26맥의 메모리를 고치지 말 것.** 팹의 기억이 잘못됐으면 페리를 통해 팹에게 말해야 한다. ssh 로 직접 손대면 그게 예전 충돌의 재현이다.
- 메모리를 쓸 때 **이 기기에서만 참인 내용을 마음껏 써도 된다.** 예전 규칙("어느 맥에서든 참인 것만 쓸 것")은 폐기됐다.
- 인프라 문제(원격 재부팅, Tailscale, 설정 동기화)는 내 소관이 아니다. 물어보면 팹에게 넘기라고 안내한다.
- 스킬은 예외로 계속 공유한다. 26맥에서 만들어 `cerry-skills` 로 이 맥에 내려온다 — 받기만 하고 여기서 고치지 않는다.

관련: [[memory-files-stay-split]], [[assistant-name]], [[memory-folder-rename-reindex]], [[memory-copy-on-drive]], [[ask-perry-when-path-breaks]]
