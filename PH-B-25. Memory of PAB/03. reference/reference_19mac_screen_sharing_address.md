---
name: reference_19mac_screen_sharing_address
description: 19맥 화면 공유(Cmd+K)는 항상 vnc://100.108.46.37 — 집 랜 주소와 SSH 별명은 파인더에서 안 통한다
metadata: 
  node_type: memory
  type: reference
  originSessionId: 365cfbaa-2eb3-4f9e-ae9f-78a8c6c0e4c6
  modified: 2026-08-02T05:34:03.142Z
---

26맥에서 19맥에 **화면 공유(파인더 Cmd+K)** 로 붙을 때 쓰는 주소는 텔레비(Tailscale) 주소 하나로 통일한다.

```
vnc://100.108.46.37
```

계정은 `psw95` + 19맥 로그인 암호. 19맥의 화면 공유 허용 대상이 관리자 그룹(`com.apple.access_screensharing` → admin)으로 잡혀 있어 그대로 통과한다.

**막히는 주소 두 가지 (260801~02에 둘 다 겪음):**

| 입력한 것 | 왜 실패하나 |
|---|---|
| `19macbook` | `~/.ssh/config`에만 있는 SSH 별명. 파인더는 그 파일을 안 읽어서 이름 조회 자체가 빈다 (`dscacheutil -q host -a name 19macbook` → 무응답) |
| `172.30.1.23` | 집 공유기 내부 주소. 26맥이 집 밖(아이폰 핫스팟 `192.0.0.x` 등)에 있으면 아예 안 닿음 |

**진단 순서** — "원격 접속 안 된다"는 말이 나오면 19맥부터 의심하지 말 것. 260802 기준 19맥은 4일 넘게 멀쩡히 켜져 있었고 SSH·VNC·tailscaled 다 정상이었다. 먼저 **26맥이 지금 어느 네트워크에 있는지** 본다:

```
route -n get default | grep -E "interface|gateway"   # 게이트웨이 192.0.0.1 이면 아이폰 핫스팟
ifconfig en0 | grep "inet "                          # 집이면 172.30.1.x
nc -z -G 5 100.108.46.37 5900                        # VNC 문 열렸는지
```

핫스팟일 때는 텔레비가 직통을 못 잡고 중계를 타서 왕복 650ms까지 늘어진다 (집 랜 15ms, 집 와이파이 경유 텔레비 107ms). 화면 공유는 되지만 느리니, 터미널 작업이면 `cerryde`를 권할 것.

관련: [[reference_ssh_bridge_between_macs]], [[reference_19mac_tailscale_system_daemon]], [[reference_three_terminal_command_map]]
