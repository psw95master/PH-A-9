---
name: obsidian-vault-path
description: 19맥 Obsidian 볼트 위치 — 사용자 폴더는 perrykim 이 아니라 psw95. ID(ESL-D-2 등) 만 주어져도 볼트 전체에서 찾을 것
metadata: 
  node_type: memory
  type: reference
  originSessionId: 9ea7b8dd-703c-43e0-b4d9-2c2ee3bf1887
  modified: 2026-08-05T05:56:39.821Z
---

19맥의 Obsidian 볼트:

```
/Users/psw95/Library/Mobile Documents/iCloud~md~obsidian/Documents/01. Obsidian/
├── 01. Perry Kim/     ← 낱장 문서
└── 02. psw/           ← ID 로그 폴더들 (ESL-D-1, ESL-D-2 …)
```

**사용자 폴더 이름은 `psw95` 다.** 페리는 경로를 불러줄 때 `/Users/perrykim/...` 이라고 말하는 일이 잦은데, 그런 계정은 없다. 앞부분만 `psw95` 로 바꾸면 그대로 열린다 — 이건 [[ask-perry-when-path-breaks]] 가 말하는 "자동 복구 한 번"에 해당하니 묻지 말고 바로 고쳐 쓰면 된다.

**ID 만 던져도 찾아낼 것.** 페리는 `A30-B-11_260726` 처럼 [[id-and-log-convention]] 의 ID 만 주고 "이 파일" 이라고 한다. 폴더명 전체를 되묻지 말고 파일명·본문 양쪽으로 검색한다:

```bash
find "…/01. Obsidian" -iname "*ESL-D-2*" -not -path "*/.obsidian/*"
grep -rl "ESL-D-2" "…/01. Obsidian" --include="*.md"
```

**Why:** 260805 에 페리가 `/Users/perrykim/...` 경로와 `A30-B-11_260726` 이라는 ID 만 주고 파일 이동을 시켰다. 계정명은 틀렸고, 파일은 볼트가 아니라 구글 드라이브에 있었다 — 즉 **ID 가 어느 저장소에 있는지도 페리는 말해주지 않는다.**

**How to apply:** ID 로 찾을 때 볼트에서 안 나오면 거기서 멈추지 말고 홈 디렉터리 전체와 구글 드라이브(MCP 로, [[id-and-log-convention]] 참고)까지 훑는다. 볼트 안 파일은 마운트 문제 없이 평범하게 읽고 쓸 수 있다 — 드라이브와 달리 iCloud 쪽은 멀쩡하다.
