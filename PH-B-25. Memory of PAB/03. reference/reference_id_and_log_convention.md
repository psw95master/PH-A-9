---
name: id-and-log-convention
description: "Perry's ID scheme (code-serial-number) and the Google Drive log format — filename {ID}_log_{YYMMDD}.md, sections 배경/과정/결과/다음 세션 참고사항"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 00e7347f-3b7f-47d1-be5a-9498fc745316
  modified: 2026-08-16T08:54:18.225Z
---

## ID 체계

IDs are generated automatically by Notion's ID feature, shaped `(코드)-(시리얼)-(넘버)`:

- **코드** — the space. Explorer's Land = `ESL` (260730 개편으로 `A30` → `ESL`)
- **시리얼** — 층 = DB. **`A` 브랜드 / `B` 프로젝트 / `C` 문서.** 층마다 DB 가 하나씩, 1:1 로 못 박혀 있다
- **넘버** — Notion 의 `auto_increment_id`. **만든 순서대로 증가한다** (랜덤 아님). DB 마다 독립 카운터

Examples: `ESL-A-11` 나우하이텍(브랜드) / `ESL-B-16` 나우하이텍 팩트북(프로젝트) / `ESL-C-1` 유압프레스 회사 리스트업(문서).

**드라이브 폴더 이름 = 노션 ID + `_` + 이름.** 글자 하나까지 같다 (`ESL-B-16_나우하이텍 팩트북`). 날짜는 폴더명에 넣지 않는다 — 260730 에 검토하고 **일부러 뺐다.** 시점이 궁금하면 노션의 `생성 일시` 를 보거나 드라이브를 "만든 날짜" 로 정렬할 것.

**깊이는 4층에서 끝난다** (스페이스 / 브랜드 / 프로젝트 / 문서). 문서 아래로는 폴더를 만들지 않고 버전 파일만 쌓는다. 260730 개편에서 Project DB 의 자기 참조를 없애고 `A→B→C` 관계로 바꿔서, **더 깊이 파는 것이 구조적으로 불가능해졌다.** 되살리지 말 것.

**부를 때는 이름이 아니라 ID 로 부른다** (260730 페리와 합의). 프로젝트와 그 산출물이 같은 이름을 갖는 일이 흔해서다 — `ESL-B-16_나우하이텍 팩트북` 안에 `ESL-C-2_나우하이텍 팩트북` 이 있다. "팩트북"이라고만 하면 어느 층인지 모른다. 이름을 억지로 다르게 짓는 대신 ID 로 지칭하기로 했다.

> 판단 기준 — **문서에 로그가 필요해지면 그건 문서가 아니라 프로젝트다.** C 에서 빼서 B 로 올린다. "층을 하나 더 팔까?" 싶으면 답은 언제나 "아니, 위로 올려".

> ⚠️ **260730 정정 — 이 문단은 원래 두 군데가 틀려 있었다.** ①"`b` = plan(계획)" — **계획 DB 는 존재한 적이 없다.** B 는 Project DB 다. ②"넘버는 랜덤" — **auto-increment 다.**

> ⚠️ **문자 정렬이 시간순과 다르다.** 자리수가 섞이면(`B-7`, `B-15`) 이름순이 `B-14, B-15, B-6, B-7` 이 된다. 구글 드라이브는 자연 정렬을 지원하지 않는다 (실측 확인). **번호를 시간순으로 믿지 말 것** — 순서는 담겨 있어도 화면엔 안 나온다.

> ⚠️ **폴더명 규칙은 여기서 뺐다 (2026-07-29).** 폴더명 체계가 전면 개편 중이라, 예전에 적어둔 접두사·섹션 코드 규칙은 전부 무효가 된다. 폴더 이름은 **추측하거나 조립하지 말고** Notion 페이지·DB의 이름과 ID 열을 그대로 읽어 쓸 것. 개편이 끝나면 그때 다시 기록한다.

**주의 — Notion API로는 ID 접두사를 못 읽는다.** SQL 조회는 숫자 부분만 돌려준다. 접두사는 **DB 이름**이나 페리가 준 스크린샷에서 얻어야 한다. 260728에 이 때문에 접두사를 오가는 혼선이 있었다. 폴더명을 바꾸기 전에는 **"이 이름으로 바꿀게, 맞아?"라고 한 번 확인할 것.**

**Operating flow:** create the Notion page (ID is minted) → 그 ID로 구글 드라이브 폴더를 만든다 → IDs from the **a_brand** database become the Slack channel names. So a Slack channel name *is* a brand-level ID, and maps 1:1 to a Drive folder.

An older `PSW-*` scheme is still visible in places (e.g. GitHub repo `PSW-A-17-B-4`); the workspace is mid-migration to the `PAB-*` scheme. This project is now **`PAB-A-2` Agents in Slack** — see [[cerry-slack-bot-process]].

## 로그 형식 (Google Drive)

각 프로젝트의 드라이브 폴더 안에 로그 폴더가 하나 있다 (이름은 개편 중이니 현물을 확인할 것). 파일명:

```
{ID}_log_{YYMMDD}.md      e.g. PAB-A-1_log_260713.md
```

Body skeleton, shared by every log in the reference folder:

```markdown
# {ID}_{YYMMDD} — {제목}

{한 줄 요약}

## 배경 (Why)
## 과정 (How)
### 1. …
### 2. …
## 결과 (What)
## 다음 세션 참고사항
## Next Step        ← 선택
```

Cross-references between logs use `[[문서명]]`. Superseded facts are kept but flagged with a `> ⚠️` callout naming what replaced them, rather than deleted.

**Reference folder** (the format's source of truth): Drive folder ID `1CM35E62VHlDnnsCqOxrQ9r8n68cs5Fxa` — `PAB-A-1` 의 로그 폴더. **폴더 ID 는 이름을 바꿔도 그대로**이니 이름 말고 이 ID 로 찾을 것.

## 로그 위치 — 드라이브 말고 깃허브인 경우가 있다 (260816 추가)

`PH-A-13` 은 **공개** 깃허브 저장소(`psw95master/PH-A-13`)이고, 작업 로그가 `log/` 폴더 안에 쌓인다. 파일명은 드라이브와 같은 규칙 — `PH-A-13_log_260816.md`.

- 260816 에 페리가 저장소를 만들면서 `log` 를 **빈 파일**로 만들어뒀다. 깃은 같은 이름을 파일과 폴더로 못 쓰므로, 그 파일을 지우고 `log/` 폴더로 바꿨다 (페리 확인 후 실행).
- ⚠️ **공개 저장소다.** 규칙·메모리 사본이 있는 `PH-A-9` 는 비공개인데 이쪽은 아니다. 로그에 뭘 적을지 판단할 때 이 차이를 기억할 것. 관련: [[reference-shared-rules-git-repo]]
- 커밋 메시지는 `[팹]` 으로 시작 (깃허브 계정이 하나라 안 적으면 구분 불가).
- `gh api repos/.../contents/...` 로 넣는다. 로컬 클론은 없다.

**Why:** This replaces the older iCloud pattern (`{코드}_00_log_{YYMMDD}.md` with flat `## N.` sections). Logs written before the migration still follow the old shape, so append to those in their own style rather than reformatting.

**How to apply:** Reach Drive through the **Google Drive MCP, not the mounted filesystem** — `~/Library/CloudStorage/GoogleDrive-*` fails with `Interrupted system call` from SSH and from launchd alike (same FileProvider limitation as iCloud, see [[sandbox-blocks-tmux-spawn]]). The MCP path works from any context. Related: [[feedback-guide-file-versioning]].
