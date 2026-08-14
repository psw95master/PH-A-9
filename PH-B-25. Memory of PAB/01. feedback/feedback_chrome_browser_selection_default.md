---
name: chrome-browser-selection-default
description: Which Chrome to pick when mcp__claude-in-chrome reports multiple connected browsers and asks the user to choose
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 84eff1c7-8ee0-431c-8d9e-246b0026237f
  modified: 2026-07-28T17:43:25.889Z
---

When `mcp__claude-in-chrome__*` reports that multiple Chrome browsers are connected and no browser has been selected for the session, default to the **2019 MacBook's Chrome**. As of 2026-07-27 that one is listed as "Browser 1" with deviceId `a49d11fe-1867-4305-8e81-3cfb0cb5dda7` (the other, `17ef9486-461d-49f2-baaf-782cf607ef90`, is the 2026 Air's).

**Why:** The user keeps the Claude-in-Chrome extension permissions approved on the 2019 MacBook, so that browser is the one that actually works without a fresh approval step. Picking the other one leads to "Permission denied by user" errors on navigate. The user gave this instruction on 2026-07-27 after a Google Sheets task stalled on exactly that.

**How to apply:** The tool still requires asking the user before selecting (it refuses to let the model pick unprompted), so present the choice as instructed but recommend the 2019 MacBook's browser first. Treat the deviceIds above as hints, not guarantees — they can change when the extension re-pairs, so match on the browser the user confirms rather than blindly reusing the ID. Related: [[chrome-browser-task-machine]], [[ssh-bridge-between-macs]].

> ⚠️ **260729 개편 이후 이 규칙은 [[chrome-browser-task-machine]] 과 정면으로 부딪친다.** 팹은 26맥에서 돌고, 그 규칙은 "자기가 도는 맥의 크롬을 쓰라"고 한다. 즉 팹의 기본값은 **26맥 크롬**이어야 한다. 위의 "19맥 우선"은 두 맥이 한 몸이던 시절의 규칙이고, 지금 남은 값어치는 하나뿐이다 — **확장 프로그램 권한이 19맥에만 승인돼 있다는 사실.** 팹이 26맥 크롬으로 처음 작업하면 권한 승인 단계를 한 번 거쳐야 하고, 그건 페리가 화면 앞에서 눌러야 한다. 그러니 26맥 크롬을 고르되, `Permission denied by user` 가 나면 원인은 이것이라고 바로 알려줄 것. 관련: [[device-split-memory]]
