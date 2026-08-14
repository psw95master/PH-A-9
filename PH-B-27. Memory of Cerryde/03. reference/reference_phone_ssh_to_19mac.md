---
name: reference-phone-ssh-to-19mac
description: "아이폰 Termius 로 19맥(세리)에 접속하는 값과, \"address already in use\" 오류의 진짜 원인"
metadata: 
  node_type: memory
  type: reference
  originSessionId: e0f19af8-e9b3-4d0b-bff0-d0902e6ed010
  modified: 2026-08-05T06:16:56.707Z
---

아이폰 **Termius** → 19맥(세리) 접속 값. 260805 재설정 후 확인됨.

| 항목 | 값 |
|---|---|
| Address | `100.108.46.37` (Tailscale 주소) |
| Port | `22` |
| Username | `psw95` |
| Key | `perry-iphone-termius` |

**Termius 가 `address already in use` 를 뱉으면 주소 충돌이 아니라 아이폰 Tailscale 이 꺼진 것이다.**
`100.108.46.37` 은 탈네트워크 안에서만 통하는 주소라, VPN 이 없으면 갈 길 자체가 없다.
Termius 가 그 상황을 이 문구로 잘못 표시한다. 호스트 설정을 고치려 들지 말고 폰의 Tailscale
스위치부터 볼 것. 확인법 — 19맥에서 `tailscale status | grep iphone` 했을 때 `offline` 이면 꺼진 것.
아이폰은 VPN 을 하나만 켤 수 있으므로 다른 VPN 이 켜져 있으면 Tailscale 이 안 붙는다.

주의할 점:
- **이 맥은 비밀번호 로그인이 막혀 있다** (`PasswordAuthentication no`). 키가 없으면 접속 불가.
- MagicDNS 이름은 `psw95-macbookpro-1.tail3736fb.ts.net` 로 `-1` 이 붙어 있다. 이름은 또 바뀔 수
  있으니 **숫자 주소를 쓸 것**. 이 맥이 스스로 부르는 이름(`psw95-macbookpro`)과 달라도 정상이다.
- 집 와이파이 주소 `172.30.1.23` 은 밖에서 안 되고 공유기 재부팅하면 바뀐다.
- 19맥이 잠들어 있으면 접속 안 된다.

관련: [[project-device-split-memory]], [[feedback-chrome-browser-task-machine]]
