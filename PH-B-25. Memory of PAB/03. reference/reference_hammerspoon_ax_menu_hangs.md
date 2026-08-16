---
name: hammerspoon-ax-menu-hangs
description: 해머스푼으로 앱 메뉴·접근성 트리를 훑으면 일부 앱에서 멈춘다 (260816 3회 실측). 앱 하나를 콕 집어 읽는 건 됨. hs CLI 위치와 -t 옵션 포함
metadata: 
  node_type: memory
  type: reference
  originSessionId: f4a3ea4f-53f8-49e7-9f4f-e488e555e685
  modified: 2026-08-16T09:22:46.885Z
---

해머스푼으로 **여러 앱의 메뉴나 접근성(AX) 트리를 훑으면 멈춘다.** 260816 세션에서 3번 겪었고, 매번 2분 타임아웃까지 갔다.

멈춘 사례:
1. 레이캐스트 창의 AX 트리 순회 (`hs.axuielement` 재귀)
2. `hs.application.runningApplications()` 전체를 돌며 `getMenuItems()`
3. 앱 5개를 지정해 `findMenuItem()` — **개수를 줄여도 멈췄다**

되는 사례:
- **앱 하나를 콕 집어** `getMenuItems()` 로 특정 메뉴만 읽기 (크롬 `파일` 메뉴는 즉시 나왔다)
- `hs.hotkey.getHotkeys()`, `hs.spaces.spacesForScreen()` 같은 목록 조회

**How to apply:**
- 메뉴를 읽어야 하면 **앱 하나 + 메뉴 하나**로 좁힐 것. 훑지 말 것.
- 멈추면 `pkill -f "Frameworks/hs/hs"` 로 끊는다. 해머스푼 본체는 안 죽는다.
- `hs.spaces.windowSpaces()` 도 멈춘 적이 있다 (창 하나 대상이었는데도).
- ⚠️ **레이캐스트 설정 창은 GUI 자동화로 읽으려 하지 말 것.** 포커스가 어긋나 엉뚱한 앱의 설정 창이 열린다 (260816에 메일 앱 '설정 > 서명' 창을 잘못 열었다).

## hs CLI 쓰는 법

바이너리는 PATH 에 없다. 전체 경로로 부른다 — 앱이 `/Applications` 이 아니라 `/Applications/@Download app/` 안에 있다.

```
"/Applications/@Download app/Hammerspoon.app/Contents/Frameworks/hs/hs" -t 20 -c '...'
```

`-t` 는 초 단위 타임아웃. `timeout` 명령어는 맥에 없으니 이걸 쓴다. `require("hs.ipc")` 가 `init.lua` 1행에 있어서 동작한다. 관련: [[hammerspoon-window-shortcuts]]
