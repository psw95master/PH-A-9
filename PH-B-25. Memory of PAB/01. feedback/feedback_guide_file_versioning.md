---
name: feedback-guide-file-versioning
description: "Preferred file structure for guide docs synced between Notion and iCloud — version-suffixed files in iCloud, latest-only in Notion"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 4f44a7ed-e9a6-43fb-96a7-1c1eba62ae2f
  modified: 2026-08-14T07:18:18.456Z
---

> ✅ **260812 확정 — 가이드 문서의 실물은 구글 드라이브에 둔다.** (`PZ-B. Files` 쪽. iCloud는 가상본만 있는 허브다 → [[feedback-icloud-hub-architecture]])
>
> **페리의 근거** — 가이드도 결국 슬라이드로 만들 자료라 **25,000자를 넘길 일이 없다.** 드라이브 읽기 한도에 걸리지 않으므로 문서류 규칙(문서=드라이브)에 예외를 둘 이유가 없다.
>
> ⚠️ **이 결정을 떠받치는 전제는 "짧다"는 것 하나뿐이다.** 드라이브는 **끊어 읽기가 안 된다** — 한 번에 못 가져오면 뒷부분이 조용히 잘리고 되살릴 방법이 없다. 그러니 가이드를 만들거나 키울 때:
> - 분량을 **2만 자 안쪽**으로 유지한다 (한도 코앞까지 채우지 않는다).
> - 넘어갈 낌새가 보이면 **키우지 말고 쪼갠다.**
> - 다 읽었는지 의심되면 **문서 끝 항목이 실제로 왔는지 확인**한다. 잘려도 에러가 안 난다.
>
> 버전 규칙(`_v1.0`, `_v1.1`)은 그대로 유효하며, 이제 드라이브 안에서 적용된다.

> 🔴 **260814 교정 — "드라이브 = 버전 보관"이 아니다.** 페리가 직접 바로잡았다.
>
> 1. **드라이브의 역할은 "파일 보관" 그 자체다.** 버전 보관소가 아니다.
> 2. **버전을 남길지는 자료 유형이 정한다.** 유형에 따라 버전이 있는 것도, 없는 것도 있다. 드라이브에 있다는 이유로 버전 파일을 만들지 말 것.
> 3. **최종 자료는 과거 버전을 지우고 최신본만 남긴다.** 예: `기획서 v1.0.0 → v1.0.5 → v1.0.8 → 최종 → v2.0` 에서 중간 버전은 정리한다. 버전 파일이 무한히 쌓이는 구조가 아니다.
>
> ⚠️ 아래 본문의 "드라이브 = 버전별 파일"은 **가이드·기획 문서 유형에 한정된 규칙**으로 읽을 것. 전체 규칙이 아니다.
>
> **버전을 쌓지 않는 유형 (확인된 것)**
> - **에이전트 규칙 문서** (`00. PH-B-24. Memory of comm/` 의 STYLE.md·DESIGN.md) — 팹이 매 세션 읽는 설정 파일이고, `cerry-export` 거울이 이미 최신 상태를 드라이브에 비춘다. `_v1.0` 을 따로 만들지 말 것 (260814 확정).
> - **로그 폴더가 있는 프로젝트의 마스터 가이드** — 아래 260803 예외 참조.

For guide/문서 제작 업무 (e.g. Notion 기획 문서를 로컬 md로 백업할 때), user's preferred structure:

- **Notion**: always holds only the latest version of the guide content (single source of truth for "current state").
- **구글 드라이브** (해당 프로젝트의 문서 폴더 — 경로는 개편 중이니 하드코딩하지 말고 현물을 확인할 것): each exported/synced version gets its own file with a version suffix in the filename, e.g. `…_v1.0.md`, `_v1.1.md`, `_v2.0.md` — one file per version, not one growing file with appended history. (260812 이전에는 iCloud에 뒀으나 드라이브로 옮기기로 확정 — 위 결정 블록 참조. 예시 파일명의 옛 ID는 **현행 체계가 아니다**.)

**Why**: User wants Notion to stay clean (latest only) while keeping a version history trail in iCloud. Considered putting a version tag inline in a single file instead, but rejected that because a single file accumulating full history over time would grow large — risking Claude missing content past the Read tool's default 2000-line window and costing more tokens on every read. Per-version separate files avoids this since each file stays a bounded snapshot.

> ⚠️ **260803 예외 — 로그 폴더가 있는 프로젝트에서는 버전 파일을 쌓지 않는다.** `PAB-B-12. Cli Alert` 에서 페리가 "여러 문서를 봐야 하면 피곤하다"며 **마스터 가이드 1개**로 통합했다 (`_v1.0`/`_v1.1` 삭제, 접미사 없는 파일 하나). 이력은 `PAB-B-12. log` 의 날짜별 로그가 이미 담당하므로 버전 파일이 중복이었다. **로그 폴더가 있으면 가이드는 1개·덮어쓰기, 없으면 아래 버전 규칙을 따른다.** 마스터 가이드에 `_v1.2` 같은 파일을 새로 만들지 말 것.

**How to apply**: When creating or updating local md copies of Notion guides for this user, name new local files with a version suffix (`_v1.0`, `_v1.1`, `_v2.0`, ...) rather than overwriting in place or appending changelog sections to one file. Ask the user (or infer from context) whether a change is minor (bump `_v1.0`→`_v1.1`) or major (→`_v2.0`) if ambiguous. Keep each individual file's content lean — current-state snapshot only, no embedded full history.
