---
name: reference_token_burn_is_cli_not_slack
description: 토큰 과소비의 진범은 슬랙 봇이 아니라 19맥의 장시간 CLI 세션 — 비용은 제곱으로 늘어난다
metadata: 
  node_type: memory
  type: reference
  originSessionId: 22eceb0a-5847-4b22-8241-5ed94eb9bafb
  modified: 2026-07-28T08:01:05.823Z
---

260728에 `~/.claude/projects/-Users-psw95/` 57개 세션을 전부 집계한 결과: 총 4.96억 토큰 중 **상위 2개 세션이 93%** (2.66억 + 1.95억). 둘 다 슬랙이 아니라 **19맥에서 손으로 연 `cerryde` CLI 세션**이었다.

원인은 **비용이 제곱으로 늘어난다는 점**이다. 한 턴 안에서 툴을 부를 때마다 대화 전체가 다시 전송된다. 컨텍스트가 600k까지 자란 뒤엔 툴 호출 한 번이 600k 캐시 읽기다. 850콜 × 평균 313k = 2.66억.

- 슬랙 봇의 `SESSION_RESET_TOKENS` 150k 임계값은 **정상 작동 중**이다. 슬랙 세션은 모두 30~80k 선에 머문다. 슬랙을 의심하기 전에 CLI를 먼저 볼 것.
- 실질 대책은 CLI 쪽: 긴 작업 중간에 `/clear`, 주제가 바뀌면 새 세션.
- 슬랙 세션과 CLI 세션은 **같은 폴더**에 쌓인다 (봇이 `cwd: homedir()`로 돌아서). 구분하려면 첫 프롬프트를 봐야 한다.

관련: [[reference_cerry_slack_bot_process]], [[reference_three_terminal_command_map]]
