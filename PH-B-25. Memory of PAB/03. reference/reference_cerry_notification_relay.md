---
name: reference_cerry_notification_relay
description: 팹(26맥)·세리(19맥) 답변 완료 알림을 26맥 화면 한 곳에 모으는 배선 — 발신부 2 + 공용 수신부 cerry-notify. 클로드 앱에서만 알림이 안 뜨는 async 훅 함정 포함
metadata: 
  node_type: memory
  type: reference
  originSessionId: 3cce8030-9e28-4005-b679-e35fcb19f612
  modified: 2026-08-13T15:48:07.748Z
---

세리는 19맥에서 돌기 때문에 알림도 19맥 화면에 뜬다. 페리는 26맥 터미널로 붙어서 보므로 그 알림을 볼 수 없었다. 260802에 세리 알림을 26맥으로 넘기는 배선을 깔았고, 260803에 팹 알림도 같은 창구에 붙였다.

**발신부 2 + 수신부 1 구조. 수신부가 깨지면 팹·세리가 동시에 멎는다:**

| 어디 | 파일 | 역할 |
|---|---|---|
| 26맥 | `~/.local/bin/cerry-notify` | **공용 수신부.** 배너를 띄우는 창구. `제목 본문 소리` 3개 인자 |
| 26맥 | `~/.claude/hooks/notify-fab.sh` | 팹 발신부. SSH 없이 직접 호출 |
| 19맥 | `~/.claude/hooks/notify-26mac.sh` | 세리 발신부. `ssh mba26` 로 중계 |
| 양쪽 | `~/.claude/settings.json` 의 `hooks.Stop` · `Notification` · `UserPromptSubmit` · `SessionEnd` | 등록부 (`async: true`) |
| 양쪽 | `~/.claude/hooks/state/pending-*` | 15분 타이머 표식. 세션마다 1개 |

소리로 누구인지 구분한다 — **팹 = Hero/Sosumi, 세리 = Glass/Ping.**

> ⚠️ **훅은 네 갈래다 (260804부터).** `Stop`·`Notification` 만 옮기면 타이머를 끄는 `UserPromptSubmit`·`SessionEnd` 가 빠져, 답을 해도 15분 뒤 알림이 계속 뜬다. 증상이 "알림이 안 온다"가 아니라 "쓸데없이 온다"로 나타나서 원인을 찾기 어렵다.

> 💤 **잠자는 결함: 클로드 앱에서 도는 Code 세션은 `async: true` 훅이 일을 마치기 전에 죽는다 (260810 실측, 미조치).** 앱은 답변이 끝나는 순간 세션을 바로 정리하는데, `async` 훅은 "뒤에서 돌라"고 떼어놓은 것이라 배너를 띄우기도 전에 같이 치워진다(`Stop` 훅이 두 줄 찍고 사라지는 것까지 로그로 확인). 터미널 세션은 답변 뒤에도 살아 있어 멀쩡히 뜬다. **페리는 Code 를 터미널로만 쓰므로 실제 피해는 없고, 그래서 고치지 않고 되돌렸다** — `settings.json` 은 지금도 `async: true` 원본 그대로다. 고칠 일이 생기면 `Stop`·`Notification` 에서 `async` 만 빼면 된다(알림은 50ms라 동기로 돌려도 체감 없음). 앞으로 앱에서 Code 를 쓰게 되면 **"눈에 보이는 일을 하는 훅에 `async` 금지"** 를 기억할 것.

> ⚠️ **26맥 훅은 반드시 "덧붙이기"로 수정할 것.** `hooks.Stop` 에 **Orca 훅**이 이미 걸려 있어, 배열을 새로 쓰면 조용히 날아간다. 19맥엔 기존 훅이 없어 이 문제가 없다.

- **"확인을 기다립니다"는 답변 직후가 아니라 15분 뒤에 뜬다 (260804 변경).** `Stop` 이 표식 파일에 토큰을 남기고 `nohup` 으로 15분짜리 타이머를 떼어 놓는다. 페리가 답하면(`UserPromptSubmit`) 표식이 지워져 타이머가 깨어나 스스로 물러난다. 답변이 또 끝나면 토큰이 바뀌어 먼저 걸린 타이머가 자동 폐기된다 — 그래서 창을 여러 개 띄워도 안 엉킨다. 시험할 땐 `FAB_WAIT_SECONDS`(팹)·`CERRY_WAIT_SECONDS`(세리)로 초를 줄인다.
- **권한·질문 알림만 즉시 뜬다.** `Notification` 훅이 메시지에 `permission`/`question`/`권한`/`질문` 이 들어간 것만 통과시키고 나머지는 버린다(그건 15분 타이머 담당). 제목도 갈라 놨다 — **"권한을 기다립니다"=즉시, "확인을 기다립니다"=15분**.
- **슬랙 세리는 알림이 안 간다 — 의도된 것.** 훅이 `$TMUX` 와 세션 이름 `cerryde-*` 로 걸러낸다. 슬랙 봇은 tmux 없이 돌기 때문([[reference_sandbox_blocks_tmux_spawn]])에 이 판별이 성립한다. 이 조건을 풀면 슬랙 대화마다 26맥에 알림이 쏟아진다.
- **`cerry-notify`는 반드시 전체 경로(`~/.local/bin/cerry-notify`)로 불러야 한다.** SSH 비대화 세션은 프로필을 안 읽어 PATH에 없다 — 19맥의 `claude` 와 같은 함정([[reference_ssh_bridge_between_macs]]).
- 알림 본문에는 **세션 이름만** 넣는다. 대화 내용을 넣으면 따옴표·줄바꿈이 ssh→원격 zsh를 지나며 깨지고, 알림에 대화가 새어 나간다([[reference_three_terminal_command_map]] 의 따옴표 4겹 교훈).
- osascript 배너가 SSH 경유로도 26맥 화면에 뜨는 것은 260802에 실측 확인됨 (macOS 26.5).
- **알림이 실제로 떴는지는 눈이 아니라 기록으로 확인할 수 있다.** `sqlite3 ~/Library/Group\ Containers/group.com.apple.usernoted/db2/db "select app.identifier, datetime(r.delivered_date+978307200,'unixepoch','localtime') from record r join app on r.app_id=app.app_id order by r.delivered_date desc limit 5"` — 26맥 터미널은 전체 디스크 접근 권한이 있어 바로 읽힌다. osascript 배너는 **`com.apple.scripteditor2`** 명의로 잡힌다(Claude 명의가 아니다).
- 폰 알림은 별개 경로 — 19맥의 `inputNeededNotifEnabled`/`agentPushNotifEnabled` 가 담당하며 둘 다 켜져 있다.

**How to apply:** ⚠️ **먼저 "어느 클로드냐"부터 물을 것.** 이 배선은 **Code 세션 전용**이다. 클로드 앱의 **일반 채팅**은 여기 해당 없고 앱 자체에 그 기능이 없다([[reference_claude_app_no_chat_notification]]) — 260810에 이걸 착각해 훅과 맥 권한을 한참 헤집었다. 둘 다 안 오면 공용 수신부(`cerry-notify`)를, **세리만** 안 오면 `ssh mba26` 도달 여부와 세션 이름(`cerryde-*`)을, **팹만** 안 오면 26맥 `hooks.Stop` 에 `notify-fab.sh` 가 살아 있는지(Orca 훅과 함께 날아가지 않았는지) 본다. 상세 자료는 드라이브 `PAB-B-12. Cli Alert` → `PAB-C-14. 마스터 가이드` 안의 **파일 1개**(`PAB-C-14_Cli Alert 마스터 가이드.md`). 코드 전문이 실려 있어 이 문서만으로 재구축 가능하다. ⚠️ 이 배선의 경로들은 드라이브·iCloud 폴더명 개편과 무관하지만, 26맥 홈 경로가 바뀌면 `cerry-notify` 호출이 끊긴다.
