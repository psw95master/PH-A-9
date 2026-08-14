---
name: shared-rules-git-repo
description: "에이전트 규칙과 메모리 사본은 깃허브 비공개 저장소 PH-A-9 하나에 모여 있다 — agent-hub 작업본, rules-sync 와 cerry-export 가 나눠 담당"
metadata: 
  node_type: memory
  type: reference
  originSessionId: b9db681d-349f-4e2c-aded-f2b00b0962b6
  modified: 2026-08-14T14:55:29.889Z
---

**저장소는 하나다.** 공유 규칙도, 두 에이전트의 메모리 사본도 `psw95master/PH-A-9` (비공개) 안에 있다. 저장소가 늘면 관리가 어렵다는 페리 판단으로 260814에 통합했다.

```
~/.claude/agent-hub/               ← 작업본 (양 맥 같은 이름)
├── PH-B-24. Memory of comm/       ← 공유 규칙 실물. STYLE.md, DESIGN.md
├── PH-B-25. Memory of PAB/        ← 팹 메모리 사본
└── PH-B-27. Memory of Cerryde/    ← 세리 메모리 사본

memory/00. PH-B-24. Memory of comm  →(심링크)→  ~/.claude/agent-hub/PH-B-24. Memory of comm
```

> 📌 **폴더 이름은 260814에 `rules/` → `PH-B-24. Memory of comm` 으로 바뀌었다.** 형제 폴더(`PH-B-25`/`PH-B-27`)와 이름 규칙을 맞춘 것이다. **`rules/` 를 찾지 말 것.** 또 바뀌면 `rules-sync` 의 `RULES_DIR` 한 줄만 고치면 된다.

| 대상 | 도구 | 커밋 | 자동 실행 |
|---|---|---|---|
| 규칙 `PH-B-24` | `rules-sync` | **수동** | `SessionStart` 에 받아오기만 |
| 메모리 사본 | `cerry-export` | **자동** | `SessionEnd` |

**쓰는 법**
1. `rules-sync` — 받아오기. 훅이 돌리므로 손으로 칠 일은 거의 없다.
2. `rules-sync push "무엇을 왜 고쳤는지"` — 규칙 고친 것 올리기.
3. `cerry-export` — 메모리 사본 밀기. 세션 끝날 때 자동.

**커밋 정책이 둘로 갈리는 이유:** 규칙은 *왜* 바꿨는지가 남아야 하므로 사람이 이유를 적을 때만 올린다. 메모리 사본은 "언제 이렇게 돼 있었다"는 스냅샷이라 자동이 맞다 — 사람이 이유를 적을 성질이 아니다.

**⚠️ 커밋 메시지는 `[팹]`·`[세리]`·`[베리티]` 로 시작한다** (260814 페리 요청). 깃허브 계정이 셋 다 `psw95master` **하나**라서, 메시지에 안 적으면 누가 고쳤는지 구분할 방법이 없다. `rules-sync`·`cerry-export` 의 `agent_name()` 이 **기기**(`id -un`: `perrykim`→팹, `psw95`→세리)를 보고 자동으로 붙인다 — 창구가 아니라 기기 기준이라 [[assistant-name]] 과 같은 규칙이다. **베리티는 깃허브 API 로 직접 커밋하므로 손으로 붙여야 한다.**

**⚠️ 커밋 작성자(이름·이메일)를 스크립트에서 지정하지 말 것.** 전역 git 설정(`psw95master` / `308692554+psw95master@users.noreply.github.com`)이 이미 옳다. 260814에 임의로 `minwoo5836@gmail.com` 을 넣었다가 **커밋 10개가 페리의 다른 계정(`Perry-Kim-1995`)으로 붙어** 이력을 다시 쓰고 강제 푸시해야 했다. 깃허브는 **이메일로** 커밋을 계정에 연결한다. 그때 백업: `~/.claude/backups/PH-A-9-before-authorfix-260814.bundle`.

**Why 깃인가 (드라이브·볼트를 버린 이유):** 드라이브 사본은 매번 덮어써져 **"지난번과 뭐가 달라졌는지"를 볼 수 없었다.** 260814에 팹이 세리 메모리를 통째로 덮어쓴 사고가 있었는데 깃이었다면 바로 보였을 일이다 ([[feedback-other-agent-memory-merge-not-replace]]). 덤으로 깃허브는 마크다운을 렌더하고 노션에 붙일 URL 도 준다.

**심링크에 대하여 — 되는 것과 안 되는 것을 구분할 것**
1. ✅ **같은 기기 안**의 고정 경로 심링크(`memory/00. PH-B-24. Memory of comm` → `agent-hub/PH-B-24. Memory of comm`)는 쓴다. 규칙 실물을 한 벌로 두면서 메모리 폴더에서 "서랍"으로 보이게 하려는 것이다.
2. ❌ **기기를 건너뛰는** 심링크(iCloud·드라이브 경유)는 금지. 260730에 정확히 그 구조로 `~/.claude/skills/` 심링크 4개가 끊겨 2주간 죽어 있었다 (26맥은 지금도 빈 폴더다 → [[cerry-link-symlink-recovery]]). 기기를 건너뛰는 일은 **깃이 담당한다.**

**⚠️ 걸려 넘어질 자리 3가지**
1. **`cerry-export` 는 메모리 사본에서 그 서랍을 제외한다.** 안 그러면 같은 규칙이 저장소에 세 벌(`PH-B-24` + 팹 사본 + 세리 사본) 들어간다.
2. **26맥은 HTTPS(`gh` 로그인), 19맥은 SSH 키로 붙는다.** 26맥 SSH 키는 깃허브에 등록돼 있지 않다 — 26맥 원격을 SSH 로 바꾸면 인증이 깨진다.
3. **저장소 삭제 권한이 없다.** `gh` 토큰에 `delete_repo` 스코프가 없어서, 저장소를 지우려면 페리가 웹에서 하거나 `gh auth refresh -h github.com -s delete_repo` 를 직접 돌려야 한다.

**메모리와 규칙을 헷갈리지 말 것.** 규칙은 셋이 **같아야** 하므로 한 벌·공유다. 메모리는 기기마다 **달라야** 하므로 각자 로컬이고 사본만 나간다 ([[device-split-memory]]).
