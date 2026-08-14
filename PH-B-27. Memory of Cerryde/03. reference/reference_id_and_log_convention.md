---
name: id-and-log-convention
description: "Perry's ID scheme (code-serial-number) and the Google Drive log format — filename {ID}_log_{YYMMDD}.md, sections 배경/과정/결과/다음 세션 참고사항"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 00e7347f-3b7f-47d1-be5a-9498fc745316
  modified: 2026-08-05T05:56:22.711Z
---

## ID 체계

IDs are generated automatically by Notion's ID feature, shaped `(코드)-(시리얼)-(넘버)`:

- **코드** — the space: `psw` = Perry's World, `pab` = PAB33
- **시리얼** — which database: `a` = brand, `b` = plan, `c` = project
- **넘버** — a random number, only there to prevent collisions (so it carries no meaning and is not sequential per parent)

Examples: `psw-a-11` 나우하이텍 / `psw-b-5` 그 웹사이트 리뉴얼 계획 / `psw-c-8` 그 기획서.

> ⚠️ **폴더명 규칙은 260729 현재 전면 개편 중이다.** 예전에 여기 적혀 있던 접두 코드 체계(저장소·워크스페이스·섹션을 조합한 세 자리 코드, `NN. 이름` 형태의 최상위 폴더명 등)는 **전부 무효**라 이 쪽지에서 들어냈다. 폴더 이름이 필요하면 **기억에서 꺼내 쓰지 말고 현물을 확인할 것** — Notion 페이지·DB 이름을 그대로 읽거나 실제 폴더 목록을 보고, 그래도 모르면 [[ask-perry-when-path-breaks]] 대로 페리에게 묻는다. 폴더명을 **조립하지 말 것.**

여전히 유효한 것은 **Notion이 발급하는 ID 자체**다 (위의 `(코드)-(시리얼)-(넘버)`). 폴더명이 그 ID를 품는다는 원칙도 유지되지만, ID 앞에 무엇이 더 붙는지는 개편 결과를 봐야 안다.

**Operating flow:** create the Notion page (ID is minted) → 그 ID 를 담아 구글 드라이브 폴더 이름을 짓는다 → IDs from the **a_brand** database become the Slack channel names. So a Slack channel name *is* a brand-level ID, and maps 1:1 to one Drive folder.

An older `PSW-*` scheme is still visible in places (e.g. GitHub repo `PSW-A-17-B-4`); the workspace is mid-migration to the `PAB-*` scheme. This project is now **`PAB-A-2` Agents in Slack** — see [[cerry-slack-bot-process]].

## 로그 형식 (Google Drive)

Each project's Drive folder holds a dedicated log subfolder (**그 폴더 이름은 개편 중이니 현물을 확인할 것**). 로그 **파일명** 규칙은 폴더명 개편의 영향을 받지 않는다:

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

**Reference folder** (the format's source of truth): Drive folder ID `1CM35E62VHlDnnsCqOxrQ9r8n68cs5Fxa` — `PAB-A-1` 의 로그 폴더. **폴더 ID 는 이름이 바뀌어도 그대로이므로, 이름 대신 이 ID 로 찾아갈 것.**

**Why:** This replaces the older iCloud pattern (`{코드}_00_log_{YYMMDD}.md` with flat `## N.` sections). Logs written before the migration still follow the old shape, so append to those in their own style rather than reformatting.

**How to apply:** Reach Drive through the **Google Drive MCP, not the mounted filesystem** — `~/Library/CloudStorage/GoogleDrive-*` fails with `Interrupted system call` from SSH and from launchd alike (iCloud 와 같은 FileProvider 제약). The MCP path works from any context. Related: [[feedback-guide-file-versioning]], [[ask-perry-when-path-breaks]].

> **260805 추가 — 마운트는 로컬 대화형 세션에서도 못 믿는다.** 19맥에서 직접 돌린 CLI 세션에서도 `Resource deadlock avoided` 로 막혔다. SSH·launchd 만의 문제가 아니다.
>
> - **범위가 넓다.** 깊이 3 폴더 12개 중 7개가 막혔다 (`ESL-C-3/5/6/7/10`, `PAB-A-1. Ops`, `ESL-C-11`). 형제 폴더끼리도 갈린다 — `PAB-A-1. Ops` 는 막히는데 `PAB-A-2. SSH` 는 열린다. 깊이·이름·내용물·빈 폴더 여부 어느 것으로도 예측 못 한다. **상위 폴더가 열렸다고 하위도 열릴 거라 가정하지 말 것.**
> - **재시도·앱 재시작·캐시 재구축 전부 무효.** 5회 재시도 동일, `DriveFS` 캐시를 통째로 새로 만들고 재로그인·재동기화까지 해도 증상 그대로였다. 클라우드 쪽 데이터는 API 로 보면 멀쩡하다. 클라이언트 버그이니 **고치려 들지 말고 우회할 것.**
> - **마운트로 파일을 옮기지 말 것.** 마운트가 막혔을 때 `read_file_content` 로 본문을 받아 다시 쓰는 우회는 **내용을 소리 없이 망가뜨린다** (`가맹점`→`가막점`, `공동임대인 김숙자`→`공김숙자` 처럼 글자가 통째로 빠짐). 부득이 그 방법을 쓴다면 **원본 바이트 수와 대조해 검증한 뒤**에야 원본을 지울 것. 삭제는 검증 전에 절대 먼저 하지 않는다.
> - 마운트에서만 가능한 일(예: 드라이브 파일 삭제 — MCP 에 삭제 도구가 없다)은 막히면 **페리에게 웹에서 처리해달라고 넘길 것.**
