---
name: reference-google-docs-api-setup
description: "구글 문서/드라이브/시트를 직접 읽고 쓰는 도구 — 19맥 `~/apps/gdocs/`. 계정·프로젝트·7일 함정 포함"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 7ffe1f91-8bfd-4a35-9e40-57d98ff70622
  modified: 2026-08-15T07:03:13.647Z
---

페리가 "이 구글 문서 읽어줘/고쳐줘" 하면 이 도구를 쓴다. 260803 구축, 26맥(팹)에도 같은 것이 깔려 있다.

위치: `/Users/psw95/apps/gdocs/` (iCloud 아님, 로컬 실폴더)

```
~/apps/gdocs/venv/bin/python ~/apps/gdocs/check.py            # 최근 문서 목록
~/apps/gdocs/venv/bin/python ~/apps/gdocs/check.py <문서ID|URL>  # 제목 + 본문
```

`check.py` 는 읽기 전용 확인용이다. 실제 편집은 `venv/bin/python` 으로 새 스크립트를 짜서 Docs/Drive/Sheets API 를 호출하면 된다. 권한은 읽기·쓰기 모두 열려 있으니 **문서 수정은 되돌리기 어렵다 — 페리에게 확인받고 실행할 것.**

## 슬라이드도 된다 (260815 확인)

`auth.py` 의 SCOPES 에 `presentations` 가 없는데도 **Slides API 읽기·쓰기가 통과한다.** `drive` 스코프가 전체 접근이라 그렇다. 재인증 없이 `build("slides", "v1", ...)` 를 바로 쓰면 된다.

**함정 두 개:**

1. `presentations().create()` 로 새로 만들면 페이지가 **10 × 5.625 in** 이다. 원본 덱들은 **13.333 × 7.5 in** 이라 좌표가 통째로 잘린다. Slides API 는 페이지 크기 변경을 지원하지 않으므로, **기존 덱을 `drive.files().copy()` 로 복사한 뒤 슬라이드만 전부 지우고 새로 그리는 것**이 유일한 방법이다. 마스터·폰트 테마도 같이 딸려온다.
2. `objectId` 는 **5자 이상**이어야 한다. `s1`, `o3` 같은 짧은 ID 는 400 으로 튕긴다.

검수는 `presentations().pages().getThumbnail()` 로 PNG 를 받아 눈으로 본다. 텍스트 박스 겹침은 좌표 계산만으로는 안 잡히니 반드시 렌더링을 확인할 것.

## 인증

- OAuth 2.0. `token.json` 안의 refresh token 으로 자동 갱신되므로 평소엔 로그인 절차가 없다
- 구글 계정: **psw95master@gmail.com** — 문서 작업은 전부 이 계정 기준. 다른 계정 이야기가 나오면 결제용이니 문서와 엮지 말 것
- GCP 프로젝트: 표시 이름 `psw95-Google-Cloud-OAuth-2026` / ID `psw95-gcp-oauth-2026`
- 인증의 전부는 `client_secret.json` + `token.json` 두 파일 (권한 600)

## 갑자기 인증이 깨지면

게시 상태는 260803에 **프로덕션**으로 올려뒀다 (테스트 상태면 refresh token 이 7일마다 만료되는데, 그걸 피하려고). `invalid_grant` 오류가 나면 콘솔에서 이게 테스트로 돌아가 있지 않은지부터 확인.

재인증: `~/apps/gdocs/venv/bin/python ~/apps/gdocs/auth.py` — **브라우저가 열리므로 페리가 직접 실행해야 한다** ([[feedback-chrome-browser-task-machine]])

재인증 후 새 `token.json` 을 26맥에도 복사해 주면 양쪽이 같이 산다 ([[reference-ssh-bridge-between-macs]]).

## 파이썬 환경

brew 파이썬 3.14.6 (260803에 시스템 파이썬 3.9 에서 갈아끼움). 26맥과 같은 버전·같은 라이브러리다.
19맥은 인텔이라 brew 경로가 `/usr/local/bin` — 26맥(`/opt/homebrew/bin`)과 다르니 주의.
