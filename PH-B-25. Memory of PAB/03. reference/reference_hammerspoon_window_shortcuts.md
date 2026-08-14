---
name: reference_hammerspoon_window_shortcuts
description: "26맥 창 관리는 해머스푼 담당 (Rectangle 퇴역, 260807). 설정은 ~/.hammerspoon/init.lua 하나"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 6c0bfd66-b046-49fa-af73-ac37ee3d3fb6
  modified: 2026-08-07T10:04:13.814Z
---

26맥의 창 배치·모니터 이동 단축키는 **해머스푼** 한 곳에서 관리한다 (260807). Rectangle은 종료·퇴역했고 앱 파일만 `/Applications/@Download app/Rectangle.app`에 남아 있다 — **다시 켜면 같은 키를 두고 해머스푼과 다툰다.**

- 설정 파일: `~/.hammerspoon/init.lua`
- 정리 문서(정본): https://app.notion.com/p/3b5efc56b3a180edad5dfc0b7df181c5
- 상태 확인: `"/Applications/@Download app/Hammerspoon.app/Contents/Frameworks/hs/hs" -c 'return #hs.hotkey.getHotkeys()'`
  - 앱 번들 안의 `hs` CLI를 직접 부른다. `/usr/local/bin/hs`는 설치돼 있지 않고, AppleScript 통로(`tell application "Hammerspoon"`)는 타임아웃으로 안 통한다.

작업할 때 걸리는 함정 두 가지:

1. **단축키 등록 실패는 조용하다.** macOS나 다른 앱이 이미 쓰는 조합이면 `hs.hotkey.bind`가 nil을 반환하고 끝난다. 바꾼 뒤엔 반드시 위 명령으로 개수를 세어 확인할 것.
2. **`defaults write com.apple.symbolichotkeys`로 맥 기본 단축키를 꺼도 그 세션에서는 안 놓는다.** `activateSettings -u`도 `killall Dock`도 소용없고 **로그아웃/재시동해야** 반영된다 (260807 실측). 설정 창에는 이미 해제된 것처럼 보여서 더 헷갈린다.

앱 전용 단축키는 전역으로 걸지 말 것 — `⌘\`(Claude 사이드바)를 전역으로 걸어놨더니 노션 등 모든 앱의 `⌘\`를 가로챘다. `hs.window.filter`로 해당 앱이 맨 앞일 때만 켜지게 하는 게 정석.
