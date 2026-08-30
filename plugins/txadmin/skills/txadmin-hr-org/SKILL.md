---
name: txadmin-hr-org
description: TX HR 조직·사원 관리 — 조직 트리·사원·부서·팀·법인 조회, Role Matrix와 권한 현황, 조직 구조/부서/팀 메일/하위 구성원 변경, 사원 정보·경력·상벌·계좌 저장, 권한 회수·복사, 팀장 동기화, AD/Entra/M365 계정 처리(비밀번호 초기화·활성화·직책·라이선스·삭제). "조직도 보여줘", "이 사원 부서 옮겨줘", "AD 비밀번호 초기화", HR org, employee, Entra, M365 요청에 사용.
---

# TX HR 조직·사원 관리

조직·사원 정보와 계정을 다루는 업무다. **다른 사람의 신원·권한·계정에 직접 영향**을 주므로
조회와 변경의 경계를 분명히 지킨다.

## 1. 조회

- 조직: `list_hr_org_tree`, `list_hr_teams`, `list_hr_companies`, `get_hr_department`
- 사원: `list_hr_users`, `get_hr_employee`, `list_hr_rewards`
- 권한 현황: `list_hr_team_role_matrix`, `list_hr_authorized_groups`, `list_hr_authorized_menus`,
  `list_hr_authorized_functions`
- 내보내기: `export_hr_employee_list`
- 동기화 사전 확인: `preview_hr_team_manager_sync` — **실제 동기화 전에 먼저 본다.**

## 2. 조직·사원 변경 (쓰기 — 전부 `confirm=true` 필요)

실행 전 **대상자 이름·사번, 바꿀 값(전/후)** 을 요약해 보여주고 동의를 받는다.

| 영역 | 도구 |
|---|---|
| 조직 구조·부서 | `save_hr_organization_structure`, `set_hr_department` |
| 팀 역할·구성원 | `set_hr_team_role_matrix`, `assign_hr_team_role_matrix_members`, `set_hr_team_under_members` |
| 팀 메일 | `set_hr_team_group_mail`, `release_hr_team_group_mail`, `set_hr_employee_team_mail_sync` |
| 사원 소속·정보 | `set_hr_employee_organization_info`, `save_hr_employee`, `delete_hr_employee_sub_team`, `restore_hr_employee` |
| 경력·상벌·계좌 | `save_hr_career`, `delete_hr_career_history`, `set_hr_reward_disciplinary`, `set_hr_employee_bank_account` |
| 팀장 동기화 | `sync_hr_team_manager` (반드시 `preview_hr_team_manager_sync` 로 대상 확인 후) |
| 거래처 동기화 | `sync_hr_biz_partner` |

## 3. 권한 회수·복사 (영향 큼)

- `recall_hr_all_auth` — **그 사람의 권한을 한꺼번에 회수한다.** 퇴사·이동 처리에만 쓰고,
  대상자와 회수 범위를 명시적으로 확인받는다.
- `recall_hr_auth_item` — 항목 단위 회수.
- `copy_hr_auth` — 다른 사람의 권한을 복사한다. **필요 이상 권한이 따라붙을 수 있으므로**
  복사 원본이 누구인지, 어떤 권한이 붙는지 먼저 보여준다.

## 4. 계정(AD/Entra/M365) 처리 (영향 큼)

- `reset_hr_ad_password` — 비밀번호 초기화. **본인 확인이 된 요청인지 먼저 묻는다.**
  초기화된 비밀번호를 대화에 그대로 남기지 않는다.
- `set_hr_ad_account_active` — 계정 활성/비활성. 비활성화하면 즉시 로그인이 막힌다.
- `set_hr_entra_job_title`, `set_hr_m365_license` — 직책·라이선스 변경(비용이 발생할 수 있다).
- `delete_hr_entra_user` — **되돌릴 수 없다.** 대상자와 사유를 확인받고 마지막에만 실행한다.

## 5. 확인과 규칙

- 처리 후 해당 조회 도구로 재조회해 실제 반영을 확인하고 보고한다.
- 개인정보(주민·계좌·연락처)는 답변에 필요한 최소한만, 마스킹된 형태로 다룬다. 목록으로 나열하지 않는다.
- 조회 기간·범위는 서버 기본값이 없다 — 사용한 범위(법인·팀·재직 상태)를 답변에 밝힌다.
- 권한은 호출 시점에 서버가 판정한다. 거절되면 우회하지 말고 필요한 권한을 안내한다.
