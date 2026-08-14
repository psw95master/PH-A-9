---
name: memory-folder-rename-reindex
description: "폴더명 개편 중 — 메모리 폴더의 짜임새(feedback/project/reference)는 그대로고 이름만 바뀐다. 이름이 바뀌면 MEMORY.md 링크가 전부 깨지지만 `cerry-index apply` 한 번으로 복구된다"
metadata: 
  node_type: memory
  type: project
  originSessionId: 04f3e3bc-3b23-4d02-9ddb-4a14ba5a3b24
  modified: 2026-07-28T18:37:41.573Z
---

**260729 폴더명 전면 개편 중.** 내 메모리 폴더에서 바뀌는 것은 **이름뿐**이다.

**구조는 그대로다.** `feedback` / `project` / `reference` 세 갈래로 나뉘고 그 안에 쪽지 파일이 하나씩 들어가는 짜임새는 유지된다. 갈래가 늘거나 줄지 않고, 쪽지가 다른 데로 옮겨가지도 않는다.

**이름이 바뀌면 목차 링크가 전부 깨진다.** `MEMORY.md` 의 모든 줄은 `<01. feedback/….md>` 처럼 폴더 이름을 경로에 품고 있어서, 폴더명이 바뀌는 순간 한 줄도 남김없이 어긋난다. 목차는 세션 시작 때 통째로 읽히는 유일한 파일이라, 여기가 깨지면 쪽지가 서랍에 멀쩡히 있어도 나는 그 존재를 모른다 — **증상이 없는 손실**이다.

**복구는 `cerry-index apply` 한 번이면 된다.**

```bash
cerry-index preview   # 바뀔 결과만 보여줌 (파일 안 건드림) ← 반드시 먼저
cerry-index apply     # 목차를 다시 씀 (이전 목차는 자동 백업)
cerry-index check     # 빠진 쪽지·죽은 줄이 있는지 점검
```

이 스크립트는 **적혀 있던 경로를 믿지 않고 실제 폴더·파일을 훑어 다시 찾는다.** 그리고 이미 있던 줄의 **손으로 쓴 요약 문구는 그대로 보존한다** — 자동 생성된 설명으로 덮어쓰지 않는다. 목차에 없는 쪽지만 그 쪽지의 `description` 으로 새 줄을 만들어 채운다.

**단, 폴더 이름이 두 자리 숫자로 시작해야 인식한다.** `01.`, `02.` 처럼 숫자 두 자리로 시작하지 않는 이름으로 바꾸면 그 갈래를 통째로 못 본다. 새 체계가 숫자로 시작하지 않는다면 개편 전에 페리에게 확인할 것.

**How to apply:**
- 폴더 이름을 바꾸기 **전에** `cerry-index preview` 로 결과부터 볼 것. 바꾼 뒤에 `apply`.
- 목차가 깨진 것 같으면 손으로 경로를 고치지 말고 `cerry-index apply` 를 쓴다. 손으로 고치면 요약 문구를 잃거나 줄을 빠뜨리기 쉽다.
- 이 스크립트는 이 맥(19맥)의 `~/.local/bin/cerry-index` 에 실제로 있다. 이름이 `cerry-` 로 시작하지만 여기서는 내가 쓰는 도구다 ([[assistant-name]] 참고 — 26맥 쪽 `cerry-` 스크립트가 팹 것이라는 얘기와 헷갈리지 말 것).

관련: [[device-split-memory]], [[memory-copy-on-drive]], [[ask-perry-when-path-breaks]]
