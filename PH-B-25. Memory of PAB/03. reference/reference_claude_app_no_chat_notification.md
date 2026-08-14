---
name: reference_claude_app_no_chat_notification
description: 클로드 앱 일반 채팅은 답변이 끝나도 맥 알림이 오지 않는다 — 고장이 아니라 기능 자체가 없음 (260810 확인)
metadata: 
  node_type: memory
  type: reference
  originSessionId: afa91413-5554-4b5f-901e-4b72c42b7f70
  modified: 2026-08-10T09:05:51.109Z
---

**클로드 데스크톱 앱(홈 탭 일반 채팅)은 답변 생성이 끝나도 맥 알림을 띄우지 않는다. 설정 문제가 아니라 그런 기능이 앱에 없다.** 260810에 앱 내부(`app.asar`)를 직접 확인했다.

앱이 정의한 알림 종류는 딱 셋이고 **전부 에이전트(Code·Cowork) 작업용**이다:

```
notificationLevels: { permission, idle, question }
```

그 외 네이티브 알림은 예약 작업 완료·실패, 사용량 소진(패스트 모드 꺼짐), 컴퓨터 사용 시작/종료, 트레이 이미지 복사 정도. **"채팅 답변 완료"에 해당하는 항목이 없다.**

- **권한을 뒤지는 건 헛수고다.** 알림 권한은 260402부터 승인 상태(`~/Library/Logs/Claude/main.log` 의 `Authorization granted: true`)이고 시스템 설정도 전부 켜져 있다. 그런데도 맥 알림 기록에 `com.anthropic.claudefordesktop` 전달 **0건** — 쏠 물건이 애초에 없어서다.
- **"알림이 안 온다"를 들으면 어느 클로드인지부터 가를 것.** Code 세션은 알림 배선이 따로 있다([[reference_cerry_notification_relay]]). 260810에 이걸 착각해서 훅·맥 권한·osascript를 한참 파헤쳤는데, 화면을 보고 나서야 홈 탭 채팅인 걸 알았다. **말이 아니라 화면을 먼저 볼 것** — "클로드 앱 답변"은 양쪽 다 가리킬 수 있다.
- **알림이 실제로 떴는지 확인하는 법** (눈보다 정확하다, 26맥 터미널은 전체 디스크 접근 권한 있음):
  ```
  sqlite3 ~/Library/Group\ Containers/group.com.apple.usernoted/db2/db \
    "select app.identifier, datetime(r.delivered_date+978307200,'unixepoch','localtime') \
     from record r join app on r.app_id=app.app_id order by r.delivered_date desc limit 5"
  ```
  osascript 배너는 Claude가 아니라 **`com.apple.scripteditor2`** 명의로 잡힌다.
- **260810에 앤트로픽에 기능 요청을 보냈다** (페리가 직접 전달). 앱 버전 1.26832.0, macOS 26.5. 나중에 앱 업데이트로 생길 수 있으니, 다시 물어보면 **최신 버전에서 `notificationLevels` 에 항목이 늘었는지부터 확인**하면 된다.

**How to apply:** 페리가 "클로드 앱 답변 알림이 안 온다"고 하면 → 화면으로 홈 탭 채팅인지 확인 → 맞으면 **고칠 수 없다고 바로 말할 것**(권한·설정을 뒤지지 말 것). 굳이 만들려면 앱 창을 접근성 API로 폴링해 "생성 중"이 끝나는 걸 감시하는 수밖에 없고, 이건 260810에 선택지로 제안했으나 페리가 고르지 않았다.
