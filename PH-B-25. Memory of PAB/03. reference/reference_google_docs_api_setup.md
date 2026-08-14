---
name: reference-google-docs-api-setup
description: 구글 문서/드라이브/시트를 프로그램으로 다루는 도구의 위치·계정·프로젝트 (양쪽 맥 모두 설치됨)
metadata: 
  node_type: memory
  type: reference
  originSessionId: 6377735c-0d34-4a2a-87e2-fcaa14651117
  modified: 2026-08-03T09:55:58.657Z
---

구글 문서를 직접 읽고 쓰는 도구는 **양쪽 맥 모두** `~/apps/gdocs/` 에 있다 (iCloud 아님, 각 기기 로컬).

- 26맥(팹): `/Users/perrykim/apps/gdocs/`
- 19맥(세리): `/Users/psw95/apps/gdocs/`

양쪽 다 brew 파이썬 3.14.6 + 같은 라이브러리 버전으로 맞춰 뒀다 (260803). 19맥은 원래 시스템 파이썬 3.9(지원 종료)를 쓰다가 이날 갈아끼웠고, brew 경로가 26맥과 다르다 — 인텔이라 `/usr/local/bin`, 26맥은 `/opt/homebrew/bin`.

실행: `~/apps/gdocs/venv/bin/python ~/apps/gdocs/check.py [문서ID|URL]` (인자 없으면 최근 문서 목록)

## 인증 (260803 구축)

- 구글 계정: **psw95master@gmail.com** — 문서 작업은 전부 이 계정 기준. 다른 계정 이야기가 나오면 결제용이니 문서와 엮지 말 것
- GCP 프로젝트: 표시 이름 `psw95-Google-Cloud-OAuth-2026` / ID `psw95-gcp-oauth-2026`
  - ID에 `google` 을 못 넣는다 (구글이 금지어로 막음) → `gcp` 로 대체함
- 켜둔 API: docs / drive / sheets
- `client_secret.json`(OAuth 클라이언트) + `token.json`(발급된 토큰) 두 파일이 인증의 전부. 권한 600
- 옛 프로젝트 `pab33cerryde` 는 260803 삭제 (용어 통일 목적)

## 갑자기 인증이 깨지면

게시 상태는 260803에 **프로덕션**으로 올려뒀다 (테스트 상태면 refresh token 이 7일마다 만료되는데, 그걸 피하려고). `invalid_grant` 오류가 나면 콘솔에서 이게 테스트로 돌아가 있지 않은지부터 확인.

재인증: `~/apps/gdocs/venv/bin/python ~/apps/gdocs/auth.py` — **브라우저가 열리므로 페리가 직접 실행해야 한다.** 새 `token.json` 은 19맥에도 복사해야 양쪽이 같이 산다.

## 다른 기기에 옮길 때

**OAuth 승인을 다시 받지 말 것.** `token.json` 을 복사하면 그만이다 — 기기에 묶이지 않는다. venv 만들고 `google-api-python-client google-auth google-auth-oauthlib google-auth-httplib2` 설치 후 `auth.py` `check.py` `client_secret.json` `token.json` 복사하면 끝. SSH로는 브라우저를 못 띄우므로 이게 유일하게 깔끔한 방법 ([[reference-ssh-bridge-between-macs]], [[feedback-chrome-browser-task-machine]])

`gcloud` CLI 는 프로젝트 생성·API 활성화에만 썼고 평소 실행에는 필요 없다 (26맥에만 설치됨).
