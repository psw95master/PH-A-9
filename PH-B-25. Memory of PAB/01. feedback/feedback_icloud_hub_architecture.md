---
name: feedback-icloud-hub-architecture
description: "iCloud는 내용을 담지 않는 허브 — 가상본(alias)으로 옵시디언·구글드라이브를 모으고, 노션이 대장 역할을 한다. 파일 ID는 노션 auto-increment가 생성"
metadata: 
  node_type: memory
  type: feedback
  modified: 2026-08-13T15:48:02.113Z
  originSessionId: 3c727165-6eec-4787-815e-7ffb03d3e4a5
---

260812 확인. **iCloud는 내용을 담는 곳이 아니라 허브(hub)다.** 실물은 옵시디언과 구글 드라이브에 있고, iCloud에는 그것들을 가리키는 **가상본(macOS alias)만** 들어간다.

## 실제 구조 — 프로젝트 하나당

```
iCloud/.../01. Projecrt zip/{프로젝트 폴더}/
├── PZ-B. Files      ← 가상본 → 구글 드라이브의 해당 프로젝트 폴더
└── PZ-C. Markdown   ← 가상본 → 옵시디언 볼트의 해당 프로젝트 폴더
```

> 폴더명 오타 `01. Projecrt zip`은 실제 경로다 (260812 기준). 고쳐 쓰지 말 것.

> 🚨 **목적지 경로를 기억에서 꺼내 쓰지 말 것.** 260812 대화 중에만도 옵시디언 쪽 경로가 두 번 바뀌었다 (`PZ-C. Appendix/…` → `PZ-C. Log/PZ-C. Log_…`). 폴더명·ID 체계는 260814 기준 개편이 거의 끝났지만, 그 기간에 경로가 여러 번 바뀌었으므로 옛 경로가 여전히 곳곳에 남아 있다. **매번 가상본을 풀어서 현물을 확인한다.**

| 도구 | 역할 | 실물 |
| --- | --- | --- |
| **iCloud** | **허브** — 가상본만 모음 | 없음 (1KB alias 파일) |
| **옵시디언** | 로그·마크다운 | 실물 |
| **구글 드라이브** | 문서류 (제안서·견적서 등) | 실물 |
| **노션** | **대장 + 정보류** | 실물 |

## 노션이 대장이다

프로젝트 페이지(`PZ-A-24`) 안에 인라인 DB 3개가 붙어 있다:

| DB | 역할 |
| --- | --- |
| `PZ-@. Calendar` | 일정 |
| `PZ-B. Files` | **파일 대장** — 이름 + URL + 계층(상위/하위) |
| `PZ-C. Appendix` | 정보류 본문 (예: `PZ-C-5. about 나우하이텍`) |

`PZ-B. Files`는 구글 드라이브 파일뿐 아니라 **GitHub 레포·피그마 파일까지** URL로 등록한다. 즉 프로젝트의 모든 외부 산출물이 여기 한 줄씩 있다.

> **⚠️ 파일 ID는 내가 짓지 않는다.** `PZ-B. Files`의 ID 열은 노션 **auto-increment 속성**이라 행을 만들면 번호가 자동으로 붙는다. 임의로 번호를 부여하거나 추측하지 말 것 — 노션에서 실제 값을 읽어온다.

> 🚨 **ID 체계는 260812 현재 현행화 전이다.** 기존 문서·로그 안에 남아 있는 ID(`ESL-*`, `BP-*`, `02. psw/…` 등)는 **옛 체계의 잔재**이며 지금 기준과 다르다. **옛 문서에서 본 ID·경로·명명 규칙을 학습하거나 그대로 따라 쓰지 말 것.** 새로 무언가를 만들 때 이름·ID가 필요하면 페리에게 묻거나 노션에서 받아온다.

## ⚠️ 가상본은 셸로 못 따라간다

`ls`·`cd`·`grep`은 alias 파일을 **1KB짜리 바이너리 파일로만** 본다. 허브 폴더에서 바로 내용을 읽으려 하면 실패한다. 목적지를 풀려면 Finder를 거쳐야 한다:

```bash
osascript -e 'tell application "Finder" to get POSIX path of ((original item of (POSIX file "/경로/PZ-B. Files" as alias)) as alias)'
```

**허브는 "어디에 뭐가 있는지 보는 지도"이고, 실제 읽기는 풀어낸 실물 경로나 노션·드라이브 MCP로 한다.**

## How to apply

- 프로젝트 자료를 찾을 때 **노션 `PZ-B. Files` 대장을 먼저 본다.** 파일을 폴더에서 뒤지는 것보다 빠르고, URL이 이미 적혀 있다.
- 허브 폴더에서 막히면 alias를 위 명령으로 풀어서 실물 경로를 얻는다.
- 새 파일을 만들면 노션 `PZ-B. Files`에 등록해야 대장에 잡힌다. ID는 노션이 매긴다.
- 같은 내용을 두 곳에 두지 않는다. 종류가 곧 위치다.

관련: [[feedback-log-reference-links-autofollow]] · [[reference-id-and-log-convention]]
