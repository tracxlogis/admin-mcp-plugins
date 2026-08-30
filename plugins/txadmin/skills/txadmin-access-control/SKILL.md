---
name: txadmin-access-control
description: TX 어드민 접근 권한 관리 — 관리자 메뉴 등록·수정·복제·정렬·이동, 메뉴/기능 권한 부여·회수, MCP 카탈로그 항목 조회, 운영자 그룹과 멤버·메뉴/기능 권한, AD 그룹 멤버 조회·추가요청·삭제, 본인 Google OTP 발급·등록. "이 메뉴 권한 줘", "운영자 그룹에 추가해줘", "AD 그룹에서 빼줘", admin menu auth, operator group, ad group 요청에 사용.
---

# TX 어드민 접근 권한 관리

메뉴·기능 권한과 그룹 구성을 다룬다. **권한을 늘리는 작업은 보안 영향이 있고, 줄이는 작업은 즉시
업무를 막는다.** 조회로 현재 상태를 먼저 확정한 뒤에만 바꾼다.

## 1. 조회

- 메뉴: `list_admin_menu_applications`, `list_admin_menus`, `search_admin_menus`,
  `get_admin_menu_name`, `search_admin_menu_tree`
- 메뉴 권한 현황: `list_admin_menu_auth`, `list_admin_menu_operator_groups`
- 기능: `list_admin_functions`, `list_admin_menu_functions`, `list_admin_function_auth`
- MCP 카탈로그: `list_mcp_catalog_entries` — 어떤 도구가 어떤 기능 권한으로 게이트되는지 확인
- 운영자 그룹: `list_operator_groups`, `get_operator_group`, `list_operator_group_members`,
  `list_operator_group_menu_auth`, `list_operator_group_function_auth`
- AD 그룹: `get_ad_group_member`, `get_ad_group_permission_history`

## 2. 메뉴·기능 변경 (쓰기 — 전부 `confirm=true` 필요)

| 하려는 일 | 도구 |
|---|---|
| 메뉴 등록·수정 | `save_admin_menu` |
| 메뉴 복제·정렬·이동 | `copy_admin_menu`, `reorder_admin_menus`, `move_admin_menu` |
| 메뉴 권한 부여·회수 | `assign_admin_menu_auth`, `revoke_admin_menu_auth`, `copy_admin_menu_auth` |
| 기능 등록·비활성 | `save_admin_function`, `deactivate_admin_function` |
| 기능 권한 부여·회수 | `assign_admin_function_auth`, `revoke_admin_function_auth` |

- **회수(revoke)·비활성(deactivate)은 즉시 사용자를 막는다.** 대상자·대상 그룹과 막히는 화면을
  먼저 알리고 동의를 받는다.
- 권한 복사는 원본의 권한이 통째로 따라붙는다 — 무엇이 붙는지 목록으로 보여준 뒤 실행한다.

## 3. 운영자 그룹 (쓰기 — `confirm=true` 필요)

- 그룹: `save_operator_group`
- 멤버: `add_operator_group_member`, `remove_operator_group_member`,
  `update_operator_group_member_nation`
- 그룹 권한: `grant_operator_group_menu_auth`, `revoke_operator_group_menu_auth`,
  `grant_operator_group_function_auth`, `revoke_operator_group_function_auth`

그룹 권한 변경은 **그 그룹의 모든 멤버에게 동시에 적용된다.** 영향 인원 수를
`list_operator_group_members` 로 먼저 확인해 보고한다.

## 4. AD 그룹·OTP

- `request_add_self_to_ad_group` — **본인**을 AD 그룹에 추가 요청.
- `remove_ad_group_member` — 그룹에서 멤버 제거. 대상자와 사유를 확인받는다.
- `issue_self_google_otp`, `register_self_google_otp` — **본인 OTP** 발급·등록.
  발급된 시크릿·코드를 대화에 남기지 않는다. 화면에서 처리하도록 안내하는 편이 안전하다.

## 5. 확인과 규칙

- 변경 후 해당 조회 도구로 재조회해 실제 권한 상태를 확인하고 보고한다.
- 서버가 그룹·소유자 범위로 한 번 더 검증한다. 거절되면 우회하지 말고 필요한 승인 경로를 안내한다.
- 이 스킬의 변경은 대부분 **다른 사람의 접근 권한**을 바꾼다. 요청자가 그 권한을 가진 사람인지
  확인이 필요하면 실행 전에 되묻는다.
