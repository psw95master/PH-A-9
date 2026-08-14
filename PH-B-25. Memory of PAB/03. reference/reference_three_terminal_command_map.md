---
name: reference_three_terminal_command_map
description: "26맥/19맥/슬랙 세 갈래 터미널의 제어 명령어 지도 — cerryde는 19맥, cerryde_slack은 슬랙 세션"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 22eceb0a-5847-4b22-8241-5ed94eb9bafb
  modified: 2026-08-12T01:02:23.303Z
---

세 갈래 터미널이 헷갈리기 쉬워 260728에 명령어를 정리했다. **방향은 항상 26맥 → 19맥** 이며, 반대(19 → 26)는 없다.

| 어디서 치나 | 명령어 | 무엇을 제어하나 |
|---|---|---|
| 26맥 | `cerryde` | 19맥 터미널 (tmux 세션, `cerryde-1`, `cerryde-2`…) |
| 26맥 | `cerryde claude` | 19맥 세션 + 리모트컨트롤 켠 클로드 + 26맥 웹창 자동 열기 (260812 추가) |
| 26맥 | `cerryde_slack` | 슬랙 세리 세션 (ssh로 19맥의 봇 엔드포인트 조회) |
| 19맥 | `cerryde_slack` | 슬랙 세리 세션 (봇이 로컬이라 ssh 없이 `localhost:7391` 직접) |

- `slacksessions`는 `cerryde_slack`의 alias로 남겨 뒀다 (예전 이름, 근육기억용).
- 사용법: `cerryde_slack` (목록) / `cerryde_slack kill 01` / `cerryde_slack kill all`
- 슬랙 세션 이름은 봇이 `slack_cerryde_01`, `_02`, `_03` … 순으로 붙인다. 빈 번호 중 가장 작은 것을 재사용하므로 01을 닫으면 다음 세션이 다시 01이 된다. 라벨은 **봇 프로세스 메모리에만** 있어 재시작하면 01부터 다시 시작한다.

`cerryde claude`가 시작 폴더를 `~/projects`로 고정한 이유(260812 실측): **홈 폴더는 신뢰 확인이 저장되지 않는다.** 홈에서 띄우면 매번 "이 폴더를 신뢰합니까?" 창에서 멈추고, 그러면 세션 주소가 안 생겨 웹창 자동 열기가 통째로 막힌다. 일반 폴더는 한 번 눌러두면 유지된다. 시작 폴더는 `CERRYDE_CLAUDE_DIR`로 덮어쓸 수 있다. 웹 주소는 디버그 로그의 `sessions/cse_XXXX`를 `https://claude.ai/code/session_XXXX`로 바꿔서 얻는다 — 화면 긁기는 폭에 따라 잘리므로 로그를 쓸 것.

**주소를 기다리는 폴링을 백그라운드(`&`)로 돌리지 말 것** (260812에 이걸로 한 번 깨졌다). 백그라운드 ssh가 터미널 입력을 붙잡으려다 SIGTTIN으로 정지해 브라우저가 영영 안 열린다. 붙기 전에 앞단에서 기다렸다 열고, ssh에는 `-n`을 붙인다.

셸 함수를 고칠 때 주의: `kill all`을 셸 루프로 짜면 **26맥 zsh → ssh → 19맥 zsh → awk** 4겹 따옴표를 통과하며 깨진다. 그래서 `DELETE /all`을 엔드포인트에 넣어 셸 쪽 따옴표를 한 겹으로 줄여 놨다. 같은 실수를 반복하지 말 것.

관련: [[reference_ssh_bridge_between_macs]], [[reference_cerry_slack_bot_process]]
