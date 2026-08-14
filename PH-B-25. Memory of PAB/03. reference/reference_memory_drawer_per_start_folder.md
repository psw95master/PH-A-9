---
name: memory-drawer-per-start-folder
description: "메모리 서랍은 세션의 시작 폴더별로 갈린다 — 홈이 아닌 곳에서 띄우면 빈 서랍이 열려 이름·R&R을 통째로 모른 채 시작한다. 19맥 `~/projects` 는 260812에 홈 서랍으로 심링크했다"
metadata: 
  node_type: memory
  type: reference
  originSessionId: d2cdd418-9ed7-4e79-9a76-5e93fbf4612d
  modified: 2026-08-12T11:12:57.819Z
---

**메모리 서랍(`~/.claude/projects/{시작폴더-슬러그}/memory/`)은 세션의 시작 폴더마다 따로 생긴다.** 기기별이 아니라 **폴더별**이다.

## 260812에 터진 증상

19맥에서 `cerryde claude` 로 띄운 세션이 자기를 **"클로드 코드"라고 소개**했다. 세리인 줄 몰랐던 것이다.

원인: `cerryde claude` 는 시작 폴더를 `~/projects` 로 고정한다(홈은 "이 폴더를 신뢰합니까?" 창에 매번 걸려서 — PAB-C-17). 그런데 세리 기억 정본은 **홈 서랍**(`-Users-psw95/memory`)에 있고 `-Users-psw95-projects/memory` 는 **빈 폴더**였다. 이름·R&R·진행 중인 일을 통째로 못 본 채 시작한 것이다.

19맥에는 `CLAUDE.md` 가 홈·`~/projects`·전역 어디에도 없어서 이름을 알려줄 대체 경로도 없었다.

## 고친 방법 (260812)

빈 서랍을 홈 서랍의 심링크로 갈아끼웠다. 실물은 홈 한 곳에 그대로 있고 공유만 된다.

```
~/.claude/projects/-Users-psw95-projects/memory
  → ~/.claude/projects/-Users-psw95/memory
```

되돌리려면 심링크만 지우면 된다(`rm`, 뒤에 `/` 붙이지 말 것).

## How to apply

- **"에이전트가 자기 이름·설정을 모른다"는 신고를 받으면 권한이나 모델을 뒤지지 말고 시작 폴더부터 본다.** `~/.claude/projects/` 에서 그 폴더의 슬러그를 찾아 `memory/` 가 비었는지 확인한다.
- 26맥은 260812 기준 서랍이 홈(`-Users-perrykim`) 하나뿐이라 안 걸린다. **다만 26맥에서도 다른 폴더로 들어가 띄우면 같은 일이 난다** — 기기의 문제가 아니라 폴더의 문제다.
- 19맥 슬랙 봇(`~/apps/agents-in-slack`)은 `claude` 를 **홈에서** 띄우므로 홈 서랍을 본다 — 지금은 정상이다. 봇의 실행 폴더를 바꾸면 같은 사고가 난다.
- 새 시작 폴더를 상시로 쓰게 되면 심링크를 같이 만들어 준다.

관련: [[device-split-memory]] · [[assistant-name]] · [[three-terminal-command-map]]
