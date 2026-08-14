## 작업 방식 (feedback)

- [STYLE.md — 응답·작업 기준](<00. rules/STYLE.md>) — **항상 적용.** 두괄식·2차 검증·요청 전 제작 금지·선택형 답변·숫자 리스트 등 11개 항목. 베리티/팹/세리가 창구와 무관하게 동일 적용. DESIGN.md와 병합 금지 (260814 등록)
- [DESIGN.md — 디자인·프로토타입 원칙](<00. rules/DESIGN.md>) — **설계 작업일 때만 적용.** 콘텐츠·화면 범위·시각 원칙·코드 인계·산출 시 남길 것 5개 섹션. 답변 방식은 다루지 않음 (STYLE.md 소관). STYLE.md와 병합 금지 (260814 등록)
- [이 기기의 이름은 "세리"](<01. feedback/feedback_assistant_name.md>) — 이름은 **기기**로 정해진다: 19맥 = 세리(PB), 26맥 = 팹(DE). 창구(CLI/슬랙)로 판단하지 말 것 — 세리도 CLI 로 불린다. 소속·직무·주요 업무 정본 포함
- [Chrome task = local machine's Chrome](<01. feedback/feedback_chrome_browser_task_machine.md>) — 브라우저 작업은 항상 세션이 도는 그 맥(19맥)의 크롬으로. SSH로 반대편 브라우저를 조종할 수는 없다
- [설명은 기획자·디자이너 눈높이로](<01. feedback/feedback_explain_for_planner_designer.md>) — 페리는 개발자가 아님. 결론 먼저, 내부 식별자 말고 비유로. 명령어·경로는 정확히 유지
- [경로가 끊기면 페리에게 묻기](<01. feedback/feedback_ask_perry_when_path_breaks.md>) — 짐작으로 폴더를 연결하면 조용히 엉뚱한 곳에 읽고 쓴다. 자동 복구 한 번, 안 되면 멈추고 질문
- [링크 준 문서 밖은 묻고 볼 것](<01. feedback/feedback_stay_inside_given_link.md>) — 드라이브·노션 임의 검색 금지. 자료가 더 필요하면 물어본다. "작성하자"는 같이 하자는 말 — 완성본을 내밀지 말 것
- [Guide file versioning](<01. feedback/feedback_guide_file_versioning.md>) — 🔴 **드라이브는 "파일 보관"이지 "버전 보관"이 아니다** (260814 교정). 버전을 남길지는 **자료 유형**이 정하고, 최종 자료는 과거 버전을 지우고 최신본만 남긴다. 버전별 파일(`_v1.0`…)은 가이드·기획 문서 유형에 한정. 에이전트 규칙 문서(`00. rules/`)는 버전 안 쌓음. ⚠️ 드라이브는 끊어 읽기가 안 되니 **2만 자 안쪽 유지**
- [스크립트는 문서에 있는 것만](<01. feedback/feedback_script_matches_document_only.md>) — 발표 원고에 구 버전 살(상세 날짜·폐기안·예상질문)을 딸려보내지 말 것. 단 문서와 어긋난 노트 정정은 예외
- [메모리 파일 병합 금지](<01. feedback/feedback_memory_files_stay_split.md>) — 하나로 합치는 안은 260726에 검토·기각됨, 재제안 말 것

## 진행 중인 일 (project)

- [device-split-memory](<02. project/project_device_split_memory.md>) — 260729 R&R 개편 — 세리(19맥)와 팹(26맥)은 메모리를 공유하지 않는다. 각자 로컬에만 두고, 페리는 드라이브 사본으로만 들여다본다. 소속·직무·주요 업무·들어오는 길 표 포함
- [폴더명 개편과 목차 복구](<02. project/project_memory_folder_rename_reindex.md>) — 메모리 구조는 그대로, 이름만 바뀐다. 깨진 `MEMORY.md` 링크는 `cerry-index apply` 로 복구 (폴더는 두 자리 숫자로 시작해야 인식)

## 참고 정보 (reference)

- [구글 문서 API 도구](<03. reference/reference_google_docs_api_setup.md>) — 페리가 구글 문서 읽어달라 하면 `~/apps/gdocs/`. 계정은 psw95master. 인증 깨지면 7일 만료 함정부터 의심
- [AI Skill Notion DB](<03. reference/reference_ai_skill_notion_db.md>) — Crafted vs Imported Skill structure; imported GitHub bundles get only their flagship SKILL.md installed, not full installer
- [메모리 드라이브 사본](<03. reference/reference_memory_copy_on_drive.md>) — 페리 보고용 사본은 읽기 전용 단방향(26맥 `cerry-export`). 폴더 이름 말고 링크로 찾아갈 것
- [Cerry Slack bot process](<03. reference/reference_cerry_slack_bot_process.md>) — lives at `~/apps/agents-in-slack` (never iCloud), launchd-managed; restart via `kickstart`, not `pkill`
- [ID & log convention](<03. reference/reference_id_and_log_convention.md>) — `(코드)-(시리얼)-(넘버)` IDs; Drive logs are `{ID}_log_{YYMMDD}.md` with 배경/과정/결과 sections; reach Drive via MCP, never the mount (마운트는 로컬 CLI 에서도 폴더 절반이 막힌다 — 고치려 들지 말 것)
- [Obsidian 볼트 경로](<03. reference/reference_obsidian_vault_path.md>) — 사용자 폴더는 `perrykim` 이 아니라 `psw95`. ID 만 주어져도 볼트·홈·드라이브까지 훑어 찾을 것
- [reference_token_burn_is_cli_not_slack](<03. reference/reference_token_burn_is_cli_not_slack.md>) — 토큰 과소비의 진범은 슬랙 봇이 아니라 19맥의 장시간 CLI 세션 — 비용은 제곱으로 늘어난다
- [폰에서 19맥 접속 (Termius)](<03. reference/reference_phone_ssh_to_19mac.md>) — 주소는 `100.108.46.37`/`psw95`. `address already in use` 는 주소 충돌이 아니라 아이폰 Tailscale 이 꺼진 것
