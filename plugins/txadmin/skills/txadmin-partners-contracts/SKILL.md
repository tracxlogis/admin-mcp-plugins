---
name: txadmin-partners-contracts
description: TX 거래처·벤더 계약·스마트루트·API 인증키 — 거래처(Biz Partner) 목록·상세·상태 집계·계정과 승인 현황·재무담당자·변경 이력, 벤더 계약과 요율 조회·저장·일괄 반영, 스마트루트 원가·벤더계약 조회, API 인증키 목록·발급·수정. "거래처 확인", "벤더 계약 요율 넣어줘", "API 키 발급", biz partner, vendor contract, smart route, api key 요청에 사용.
---

# TX 거래처·벤더 계약·API 키

거래처 정보와 벤더 계약, 스마트루트 원가, 외부 연동 인증키를 다룬다.

## 1. 거래처(Biz Partner) 조회

- `list_biz_partners`, `get_biz_partner` — 목록·상세
- `get_biz_partner_status_summary` — 상태 집계
- 계정: `list_biz_partner_accounts`, `list_biz_partner_account_approvals`,
  `get_biz_partner_account_approval_status_summary`
- `list_biz_partner_financial_managers` — 재무 담당자
- `get_biz_partner_history` — 변경 이력. "언제 누가 바꿨나"는 여기서 확인한다.

이 도메인은 **조회 전용**이다. 거래처 정보 변경 도구는 제공되지 않으므로,
변경 요청은 어드민 화면에서 처리하도록 안내한다.

## 2. 벤더 계약·요율

- 조회: `list_vendor_contracts`, `get_vendor_contract_detail`, `list_vendor_contract_rates`,
  `list_vendors`, `list_shipping_regions`
- 쓰기(`confirm=true`): `set_vendor_contract`, `set_vendor_contract_rate_bulk`
  - **요율은 원가·정산 금액에 직접 반영된다.** 대상 계약·구간·전후 금액·적용 기간을 요약해
    동의를 받는다.
  - `set_vendor_contract_rate_bulk` 는 **한 번에 여러 구간을 덮어쓴다.** 반영 건수와 샘플을
    먼저 보여주고, 기존 값이 사라지는 범위를 분명히 알린 뒤 실행한다.

## 3. 스마트루트

- `list_smart_routes`, `get_smart_route_detail`, `list_smart_route_costs`,
  `list_smart_route_vendor_contracts` — 라우트와 원가·연결 계약 조회.

## 4. API 인증키

- 조회: `list_api_certification_keys`, `get_api_certification_key`,
  `list_api_certification_key_nations`
- 쓰기(`confirm=true`): `create_api_certification_key`, `update_api_certification_key`
  - **발급된 키 값을 대화에 그대로 남기지 않는다.** 키가 필요한 사람에게 화면에서 직접 확인하도록
    안내하고, 여기서는 발급 사실·대상 앱·국가만 보고한다.
  - 키를 새로 발급하면 기존 연동이 영향을 받을 수 있다 — 대상 앱과 영향을 먼저 확인받는다.

## 5. 확인과 조회 기준

- 변경 후 같은 조회 도구로 재조회해 반영을 확인하고 보고한다.
- 기간·적용일은 **서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고 답변에 밝힌다.
- 금액은 통화와 함께 표기하고 임의 환산하지 않는다.
- 화면과 다르면 적용 기간, 국가·벤더 범위, 계약 상태 필터부터 대조한다.
