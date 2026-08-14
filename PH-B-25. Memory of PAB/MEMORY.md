## 작업 방식 (feedback)

- [STYLE.md — 응답·작업 기준](<00. PH-B-24. Memory of comm/STYLE.md>) — **항상 적용.** 두괄식·2차 검증·요청 전 제작 금지·선택형 답변·숫자 리스트 등 11개 항목. 베리티/팹/세리가 창구와 무관하게 동일 적용. DESIGN.md와 병합 금지 (260814 등록)
- [DESIGN.md — 디자인·프로토타입 원칙](<00. PH-B-24. Memory of comm/DESIGN.md>) — **설계 작업일 때만 적용.** 콘텐츠·화면 범위·시각 원칙·코드 인계·산출 시 남길 것 5개 섹션. 답변 방식은 다루지 않음 (STYLE.md 소관). STYLE.md와 병합 금지 (260814 등록)
- [이 기기의 이름은 "팹"](<01. feedback/feedback_assistant_name.md>) — 이름은 **기기**로 정해진다: 26맥 = 팹(DE), 19맥 = 세리(PB). 창구(CLI/슬랙)로 판단하지 말 것 — 세리도 CLI 로 부른다. 소속·직무·주요 업무 정본 포함
- [크롬은 26맥 것을 쓴다](<01. feedback/feedback_chrome_browser_selection_default.md>) — 260729부터 팹은 자기 기기(26맥) 크롬. 다만 확장 권한은 19맥에만 승인돼 있어 첫 사용 시 페리의 승인이 필요
- [Chrome task = local machine's Chrome](<01. feedback/feedback_chrome_browser_task_machine.md>) — 브라우저 작업은 항상 세션이 도는 그 맥의 크롬으로. SSH로 반대편 브라우저를 조종할 수는 없다
- [설명은 기획자·디자이너 눈높이로](<01. feedback/feedback_explain_for_planner_designer.md>) — 페리는 개발자가 아님. 결론 먼저, 내부 식별자 말고 비유로. 명령어·경로는 정확히 유지
- [Guide file versioning](<01. feedback/feedback_guide_file_versioning.md>) — 🔴 **드라이브는 "파일 보관"이지 "버전 보관"이 아니다** (260814 교정). 버전을 남길지는 **자료 유형**이 정하고, 최종 자료는 과거 버전을 지우고 최신본만 남긴다. 버전별 파일(`_v1.0`…)은 가이드·기획 문서 유형에 한정. 에이전트 규칙 문서(`00. rules/`)는 버전 안 쌓음. ⚠️ 드라이브는 끊어 읽기가 안 되니 **2만 자 안쪽 유지**
- [로그 속 링크는 같이 연다](<01. feedback/feedback_log_reference_links_autofollow.md>) — 로그에 박힌 노션·드라이브 링크는 출처가 아니라 본문의 일부. 지시 없어도 먼저 다 열고 답할 것. 작성 시 `🔗 참고자료 — 읽을 때 함께 열 것` 형식 사용
- [iCloud는 허브다](<01. feedback/feedback_icloud_hub_architecture.md>) — 실물은 옵시디언·구글드라이브에 있고 iCloud엔 **가상본만**. 노션이 대장(`PZ-B. Files`)이고 파일 ID는 노션 auto-increment가 매긴다. ⚠️ 가상본은 셸로 못 따라감 — `osascript`로 풀 것
- [반대편 기기 메모리는 덮어쓰지 말고 얹기](<01. feedback/feedback_other_agent_memory_merge_not_replace.md>) — 팹↔세리는 같은 이름이어도 별개 파일. 원문 그대로 두고 교정 블록만 위에 얹을 것. ⚠️ scp 는 `.overwritten/` 백업이 안 남는다. **규칙 문서(`00. rules/`)는 복사가 맞고, 메모리 파일은 병합** (260814)
- [메모리 파일 병합 금지](<01. feedback/feedback_memory_files_stay_split.md>) — 하나로 합치는 안은 260726에 검토·기각됨, 재제안 말 것
- [Skill registration routine](<01. feedback/feedback_skill_registration_routine.md>) — full steps for registering a skill; always finish by mirroring the `~/.claude/skills/` symlink on BOTH Macs, not just one

## 진행 중인 일 (project)

- [device-split-memory](<02. project/project_device_split_memory.md>) — 260729 R&R 개편 — 팹(26맥)과 세리(19맥)는 메모리를 공유하지 않는다. 각자 로컬 실폴더에 두고, 페리 확인용 읽기 전용 사본만 내보낸다. **사본 자리는 깃허브 비공개 저장소 `PH-A-9`** — 규칙과 한 저장소를 쓴다 (260814 확정, 구글 드라이브 사본은 같은 날 삭제 — 옛 경로 쓰지 말 것). 깃인 이유는 드라이브가 덮어쓰기라 **"뭐가 달라졌는지"를 못 봤기 때문**. 세리 사본도 여기 있고, 팹 세션이 끝날 때 밀린다. 소속·직무·주요 업무·들어오는 길 표 포함

## 참고 정보 (reference)

- [규칙·메모리 사본은 깃허브 PH-A-9 한 곳](<03. reference/reference_shared_rules_git_repo.md>) — 작업본은 양 맥 `~/.claude/agent-hub/`. `memory/00. rules` 는 그리로 가는 **심링크**. 규칙은 `rules-sync push "이유"`(수동 커밋), 메모리 사본은 `cerry-export`(자동 커밋). ⚠️ 같은 기기 안 심링크는 OK·기기 건너뛰는 심링크는 금지 (260814)
- [19맥 전원 복원력](<03. reference/reference_19mac_power_resilience.md>) — 항상 집에 있는 원격 전용 기기. `acwake 1` + `disablesleep 1`(260729, 뚜껑 닫아도 안 잠). 한번 잠들면 원격으로 못 깨움 — 완전 종료도 사람 필요
- [19맥 화면 공유 주소](<03. reference/reference_19mac_screen_sharing_address.md>) — Cmd+K는 항상 `vnc://100.108.46.37`. 집 랜 주소·SSH 별명은 파인더에서 안 통함. "원격 접속 안 됨"은 19맥 고장이 아니라 26맥이 붙은 네트워크부터 볼 것
- [19맥 원격 재부팅](<03. reference/reference_19mac_remote_reboot.md>) — `fdesetup authrestart`로 무인 재부팅, Tailscale은 이제 GUI 로그인 없이 복구됨 (LAN 172.30.1.23은 안전망)
- [19맥 Tailscale 시스템 데몬](<03. reference/reference_19mac_tailscale_system_daemon.md>) — brew `tailscaled`가 유일한 구현체, IP는 **100.108.46.37**. 충돌하던 App Store 앱은 260728 제거됨 (재설치 금지)
- [메모리 서랍은 시작 폴더별로 갈린다](<03. reference/reference_memory_drawer_per_start_folder.md>) — 홈이 아닌 곳에서 띄우면 빈 서랍이 열려 이름·R&R을 모른 채 시작한다. "자기 이름을 모른다"는 신고는 권한 말고 **시작 폴더**부터 볼 것. 19맥 `~/projects`는 260812에 홈 서랍으로 심링크됨
- [19맥 TCC = Aqua 도메인 전용](<03. reference/reference_19mac_tcc_aqua_domain.md>) — SSH(Background)는 FDA 무효, cerryde tmux 서버가 LaunchAgent라서 팬이 Aqua를 물려받음
- [구글 문서 API 도구](<03. reference/reference_google_docs_api_setup.md>) — 양쪽 맥 `~/apps/gdocs/`. 계정은 psw95master. 다른 기기로 옮길 땐 OAuth 재승인 말고 `token.json` 복사
- [AI Skill Notion DB](<03. reference/reference_ai_skill_notion_db.md>) — Crafted vs Imported Skill structure; imported GitHub bundles get only their flagship SKILL.md installed, not full installer
- [Anthropic marketplace duplicate](<03. reference/reference_anthropic_marketplace_duplicate.md>) — `claude-plugins-official` vs `claude-code-plugins` both ship `frontend-design`; check `claude plugin list` before installing to avoid dupes
- [세리 알림 중계](<03. reference/reference_cerry_notification_relay.md>) — 세리 답변 완료 알림을 26맥 화면에 띄우는 배선(19맥 훅 → SSH → 26맥 `cerry-notify`). 두 조각이라 한쪽만 옮기면 조용히 끊긴다. 슬랙 세리가 조용한 건 의도된 것. ⚠️ **Code 세션 전용** — 일반 채팅은 해당 없음
- [클로드 앱 채팅은 알림이 없다](<03. reference/reference_claude_app_no_chat_notification.md>) — 홈 탭 일반 채팅은 답변이 끝나도 배너가 안 뜬다. 고장이 아니라 기능 부재라 **권한을 뒤지지 말 것**. "알림이 안 온다"를 들으면 어느 클로드인지 화면부터 확인
- [심링크 복구 도구](<03. reference/reference_cerry_link_symlink_recovery.md>) — iCloud 폴더명 바뀌어 스킬 심링크가 끊기면 `cerry-link fix`; 경로 아닌 내용 표식으로 찾아 복구. ⚠️ **스킬 4개가 260730부터 끊긴 채라 `~/.claude/skills/` 가 비어 있다** — `check` 는 이 상태를 정상으로 오인함
- [Cerry Slack bot process](<03. reference/reference_cerry_slack_bot_process.md>) — lives at `~/apps/agents-in-slack` (never iCloud), launchd-managed; restart via `kickstart`, not `pkill`
- [옵시디언 ID는 `OBS-*`](<03. reference/reference_obsidian_obs_id_system.md>) — 노션 `ESL-*`과 번호대가 갈라져 있다 (충돌 방지, 260804 결정). ⚠️ 자동 채번은 260804에 철거돼 지금은 수동 — 템플릿이 없는 건 고장이 아니다
- [26맥 창 단축키 = 해머스푼](<03. reference/reference_hammerspoon_window_shortcuts.md>) — Rectangle은 260807 퇴역(다시 켜면 충돌). 등록 실패는 조용하니 개수로 확인, 맥 기본 단축키 해제는 재시동해야 반영
- [ID & log convention](<03. reference/reference_id_and_log_convention.md>) — `(코드)-(시리얼)-(넘버)` IDs; Drive logs are `{ID}_log_{YYMMDD}.md` with 배경/과정/결과 sections; reach Drive via MCP, never the mount. 폴더명 규칙은 개편 중이라 제거됨
- [Sandbox blocks tmux spawn](<03. reference/reference_sandbox_blocks_tmux_spawn.md>) — tmux servers started from a Cerry session die in ~1s; never design bots that spawn tmux
- [settings.json은 동기화 안 됨](<03. reference/reference_settings_json_not_synced.md>) — 기기별 로컬 파일. 260728 기준 양쪽 permissions 동일. deny 없이 allow만 복사하지 말 것
- [SSH bridge between Macs](<03. reference/reference_ssh_bridge_between_macs.md>) — `ssh mba26` reaches the 2026 Air from the 2019 MBP over Tailscale (user `perrykim` there vs `psw95` here), key auth already works; reverse alias `19macbook` → 100.108.46.37
- [reference_three_terminal_command_map](<03. reference/reference_three_terminal_command_map.md>) — 26맥/19맥/슬랙 세 갈래 터미널의 제어 명령어 지도. `cerryde claude`(리모트컨트롤+웹창 자동)가 홈이 아닌 `~/projects`에서 시작하는 이유 포함
- [reference_token_burn_is_cli_not_slack](<03. reference/reference_token_burn_is_cli_not_slack.md>) — 토큰 과소비의 진범은 슬랙 봇이 아니라 19맥의 장시간 CLI 세션 — 비용은 제곱으로 늘어난다
- [two-macs-share-one-icloud-drive](<03. reference/reference_two_macs_share_one_icloud_drive.md>) — 두 맥은 같은 iCloud Drive를 쓴다 (260728 실측 확인). 계정 목록에 여러 개 보이는 것과 별개 — iCloud 폴더 경로는 기억에서 꺼내 쓰지 말고 매번 현물 확인
