---
name: reference-google-docs-api-setup
description: 구글 문서/드라이브/시트를 직접 읽고 쓰는 도구 — 19맥 `~/apps/gdocs/`. 계정·프로젝트·7일 함정 포함
metadata:
  type: reference
---

페리가 "이 구글 문서 읽어줘/고쳐줘" 하면 이 도구를 쓴다. 260803 구축, 26맥(팹)에도 같은 것이 깔려 있다.

위치: `/Users/psw95/apps/gdocs/` (iCloud 아님, 로컬 실폴더)

```
~/apps/gdocs/venv/bin/python ~/apps/gdocs/check.py            # 최근 문서 목록
~/apps/gdocs/venv/bin/python ~/apps/gdocs/check.py <문서ID|URL>  # 제목 + 본문
```

`check.py` 는 읽기 전용 확인용이다. 실제 편집은 `venv/bin/python` 으로 새 스크립트를 짜서 Docs/Drive/Sheets API 를 호출하면 된다. 권한은 읽기·쓰기 모두 열려 있으니 **문서 수정은 되돌리기 어렵다 — 페리에게 확인받고 실행할 것.**

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
