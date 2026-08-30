---
name: txadmin-shipping-fee-policy
description: TX 배송비 정책·요율 — 배송비 정책, 계약 업체·마스터·상세 요율 조회, 할인·할증 매핑, 국가 라우트·배송사·채널·창고·지역 후보값, 환율 조회, 배송비 추천 대상 탐지·설정·통지, 요율 알람 설정과 수신자. "배송비 정책 보여줘", "이 계약 요율 확인", "배송비 추천 대상 찾아줘", shipping fee policy, rate table, fee alarm 요청에 사용.
---

# TX 배송비 정책·요율

배송비 정책과 계약 요율을 확인하고 조정하는 업무다. 요율은 **정책 → 계약 → 마스터 → 상세**의
계층 구조이므로, 어느 계층을 보고 있는지 항상 밝힌다.

## 1. 조회

- 정책: `list_shipping_policies`, `list_shipping_policy_nation_routes`
- 계약: `list_shipping_contract_companies`, `list_shipping_contract_masters`,
  `list_shipping_contract_details`
- 계약 상세 묶음 조회: `get_contract_master_details_by_masters`,
  `get_contract_master_details_by_companies`
- 할인·할증: `list_shipping_discounts`, `list_contract_surcharge_mappings`
- 후보값: `list_shipping_companies`, `list_shipping_channels`, `list_service_nations`,
  `list_warehouses`, `list_shipping_regions`
- 환율: `get_transfer_exchange_rate`

## 2. 배송비 추천

- `detect_shipping_fee_recommendation_targets` — 추천 대상 탐지.
- `get_shipping_fee_recommendation` — 추천 내용 확인.
- `set_shipping_fee_recommendation` (쓰기, `confirm=true`) — 추천값 반영.
  **요율은 청구 금액에 직접 반영된다** — 대상 구간·전후 금액·적용 시점을 요약해 동의를 받는다.
- `notify_shipping_fee_recommendation` (쓰기, `confirm=true`) — **관련자에게 통지가 나간다.**
  수신 대상과 내용을 먼저 보여주고 동의를 받는다.

## 3. 요율 알람

- `list_shipping_fee_alarm_configs`, `list_shipping_fee_alarm_recipients` — 현재 설정·수신자.
- `set_shipping_fee_alarm_config` (쓰기, `confirm=true`) — 알람 조건 변경.
  조건을 느슨하게 바꾸면 이상 요율을 놓치고, 조이면 알림이 폭주한다. 전후 값을 함께 보여준다.

## 4. 확인

변경 후 같은 조회 도구로 재조회해 반영을 확인하고, **적용 범위(국가·배송사·구간)와 시점**을
함께 보고한다.

## 조회 기준

- 기간·적용일 기준은 **서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고 답변에 밝힌다.
- 통화가 섞이므로 금액은 **통화와 함께** 표기하고 임의로 환산하지 않는다.
  환산이 필요하면 `get_transfer_exchange_rate` 값을 쓰고 어느 시점 환율인지 밝힌다.
- 화면 금액과 다르면 계층(정책/계약/마스터/상세), 적용일, 국가·배송사 범위부터 대조한다.
