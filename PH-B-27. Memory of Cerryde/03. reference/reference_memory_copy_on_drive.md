---
name: memory-copy-on-drive
description: 페리 보고용 메모리 사본은 구글 드라이브의 읽기 전용 단방향 복사본 — 26맥의 cerry-export 가 세션 종료 시 ssh 로 당겨간다. 폴더 이름이 아니라 링크로 찾아갈 것
metadata: 
  node_type: memory
  type: reference
  originSessionId: 04f3e3bc-3b23-4d02-9ddb-4a14ba5a3b24
  modified: 2026-07-28T19:03:48.971Z
---

**페리가 내 기억을 들여다보는 사본은 구글 드라이브에 있고, 읽기 전용 단방향이다.**

- **심링크가 아니다.** 내 로컬 메모리 폴더와 실시간으로 이어져 있지 않다.
- **26맥의 `cerry-export` 가 세션 종료 시 ssh 로 당겨간다.** 내가 클라우드에 직접 쓰는 일은 없고, 당겨가는 쪽도 팹(26맥)이다.
- **방향은 한쪽뿐이다** — 내 메모리 → 드라이브 사본. **사본을 고쳐도 내 기억은 바뀌지 않는다.** 기억을 고치려면 로컬 메모리 파일을 고쳐야 한다.

**폴더명 개편 중 주의:** 이 내보내기는 개편 후에도 계속 돌지만 **목적지 폴더 이름이 바뀐다.** 그러니 사본을 찾을 때 **폴더 이름으로 뒤지지 말고 아래 링크로 바로 갈 것** — 링크에 박힌 폴더 ID 는 이름이 바뀌어도 그대로다.

- **세리 사본:** https://drive.google.com/open?id=1kXx0fqYpoVIj5dEJtSPTXMhkJgR_UJsi
- **팹 사본:** https://drive.google.com/open?id=1BZBzpnbX8tDjQ0MI0VBxf6I7UrSYlNxS (팹 것이니 읽기만 할 것 — [[device-split-memory]])

각 사본 폴더에는 `README.md` 가 있어 "사본을 고쳐도 에이전트에 반영되지 않는다"를 안내한다. 옛 아이클라우드 보관처는 260729에 **비워졌다** — 오래된 문서에서 아이클라우드 경로를 보면 이미 없는 곳이다.

**260729 이관은 직무별로 갈라 두 곳으로 갔다** — 인프라·원격 지식은 팹 쪽, 공통 규칙은 내 쪽. 그래서 예전에 내 것이던 인프라 쪽지를 팹 사본에서 발견해도 그건 정상이다.

**How to apply:**
- 페리가 "네 기억 사본 어디 있어?" 라고 물으면 폴더 경로가 아니라 **위 링크**를 준다.
- 사본이 최신이 아닌 것 같다는 얘기가 나오면, 그건 팹의 `cerry-export` 쪽 일이다 — 내가 손댈 수 없으니 팹에게 넘기라고 안내한다.
- 사본 링크가 죽었는데 새 위치를 모르면 짐작으로 뒤지지 말고 페리에게 묻는다 ([[ask-perry-when-path-breaks]]).

**정본 문서: 노션 `PSW R&R Guide` (ID `A20-C-16`)** — https://app.notion.com/p/3abefc56b3a1806e8f60c5b77ffce7b7
R&R·메모리 사본에 관한 사실은 이 페이지를 정본으로 본다.
관련: [[device-split-memory]], [[memory-folder-rename-reindex]], [[assistant-name]]
