---
name: two-macs-share-one-icloud-drive
description: 두 맥은 같은 iCloud Drive를 쓴다 (260728 실측 확인). 계정 목록에 여러 개가 보이는 것과 별개 — 260728 접속 불가의 원인은 계정이 아니라 최상위 폴더명 변경이었다
metadata: 
  node_type: memory
  type: reference
  originSessionId: 261575ab-c160-4bf7-b0f7-cbbc5991f7ea
  modified: 2026-07-28T18:22:52.565Z
---

**두 맥은 같은 iCloud Drive 를 공유한다.** 26맥 iCloud 최상위에 표시 파일을 심고 19맥에서 같은 내용으로 읽히는지 확인해 2026-07-28에 실측했다.

계정 목록만 보면 헷갈린다 — 26맥에는 `psw95perrykim@gmail.com` 과 `psw95master@gmail.com` 두 개가 동시에 로그인돼 있고, `MobileMeAccounts` 로는 그중 어느 것이 `com~apple~CloudDocs` 를 소유하는지 알 수 없다 (네 계정 모두 서비스 목록에 `MOBILE_DOCUMENTS` 가 들어있다). 실제 소유자는 19맥과 같은 `psw95master` 다.

> ⚠️ 이전 기억 `reference_two_macs_different_icloud_accounts` 는 "계정이 서로 다르다"고 적었다. 계정 *목록*은 다르지만 **iCloud Drive 는 하나**이므로 그 제목은 오해를 준다. 이 쪽지가 대체한다.

**260728 "iCloud 접속 안 됨"의 원인은 계정이 아니라 폴더명 변경이었다.** 최상위 폴더 이름이 바뀌면서 옛 경로를 쓰던 심링크가 전부 끊겼다. 끊기면 [[cerry-link-symlink-recovery]] 의 `cerry-link fix`.

**How to apply:** **iCloud 안의 폴더 경로를 기억이나 옛 문서에서 가져다 쓰지 말 것.** 이름은 수시로 바뀌고 지금도 개편 중이다 — 항상 `cerry-link list` 나 실제 디렉터리 조회로 현재 위치를 확인할 것. 그리고 iCloud 가 하나라고 해서 두 맥의 기억이 자동으로 같아지는 것은 아니다 — 260729부터 두 맥은 메모리를 아예 공유하지 않는다 ([[device-split-memory]]).
