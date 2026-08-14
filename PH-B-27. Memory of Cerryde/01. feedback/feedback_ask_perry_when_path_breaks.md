---
name: ask-perry-when-path-breaks
description: 경로가 끊겼는데 새 위치를 모르면 폴더를 뒤지며 짐작으로 연결하지 말고 페리에게 물을 것 — 틀린 폴더에 조용히 읽고 쓰는 게 끊긴 것보다 나쁘다
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 04f3e3bc-3b23-4d02-9ddb-4a14ba5a3b24
  modified: 2026-07-28T18:38:17.320Z
---

**경로가 끊겼는데 새 위치를 모르면, 뒤지지 말고 페리에게 물어본다.**

폴더명 개편 중에는 바뀐 위치를 통보받지 못하는 경우가 생긴다. 그때 폴더 목록을 훑으며 "이게 그건가" 하고 **짐작으로 연결하면, 틀린 폴더에 붙고 그 뒤로 조용히 엉뚱한 곳을 읽고 쓴다.** 이건 그냥 끊겨 있는 것보다 나쁜 상태다 — 끊기면 에러가 나서 바로 알지만, 잘못 붙으면 아무 증상 없이 계속 굴러간다.

**How to apply:**
1. **자동 복구를 한 번 시도한다.** 메모리 목차라면 `cerry-index apply` ([[memory-folder-rename-reindex]]), 드라이브라면 링크·폴더 ID 로 직행 ([[memory-copy-on-drive]]).
2. **안 되면 거기서 멈춘다.** 두 번째 시도로 폴더를 훑지 않는다.
3. **"어느 폴더로 바뀌었어?" 라고 페리에게 묻는다.** 어떤 경로가 끊겼는지, 무엇을 하려던 참이었는지 함께 말한다.

이름이 비슷하다거나 내용이 그럴듯하다는 이유로 새 폴더를 "그거겠지" 하고 채택하지 말 것. **개편 기간 내내 적용되는 규칙이다.**

관련: [[memory-folder-rename-reindex]], [[memory-copy-on-drive]], [[device-split-memory]]
