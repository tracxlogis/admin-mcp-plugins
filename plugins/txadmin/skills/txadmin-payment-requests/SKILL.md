---
name: txadmin-payment-requests
description: TX 지급요청·개인경비 — 지급요청 목록·상세·저장, 상태/카테고리 금액 집계, 지급 업체 프로필 관리, 개인경비 거래 조회·등록·일괄수정과 코드(리포트그룹·법인·세무그룹·카드·사원). "지급요청 현황", "이번 달 지급 예정", "개인경비 등록해줘", payment request, personal expense 요청에 사용.
---

# TX 지급요청·개인경비

지급요청(Payment Request)과 개인경비(Personal Expense) 업무를 수행한다.

## 1. 지급요청 조회

- `list_payment_requests` — 목록. **시작·종료일이 필수**다.
  - `date_type`: `R`=등록일(기본, 빠름) / `T`=지급예정일 / `P`=지급완료일.
    **T·P 는 넓게 조회한 뒤 다시 걸러내므로 느리다** — 꼭 필요할 때만 쓴다.
  - `limit` 기본 200, 최대 1000. 결과가 잘리면 응답에 `truncated` 가 표시되므로
    **잘렸다는 사실을 반드시 사용자에게 알린다.**
  - 법인 범위(국가·법인구분·법인코드)를 비우면 권한이 허용하는 **전 법인이 섞여** 나온다.
- `get_payment_request_detail` — 건별 상세. 계좌 정보는 마스킹된 형태로만 보인다.
- 집계: `get_payment_request_status_summary`, `get_payment_request_category_amount_summary`
- 설정·코드: `get_payment_request_line_setup`, `list_payment_request_sub_categories`

## 2. 개인경비 조회

- `list_personal_expense_transactions`, `get_personal_expense_transaction`
- 집계: `get_personal_expense_status_summary`
- 코드: `list_personal_expense_report_groups`, `list_personal_expense_companies`,
  `list_personal_expense_tax_groups`, `list_personal_expense_cards`, `list_personal_expense_employees`

## 3. 등록·수정 (쓰기 — 전부 `confirm=true` 필요)

금액과 지급 대상이 걸린 업무다. 실행 전 **금액·통화·대상 업체·귀속 법인**을 요약해 동의를 받는다.

| 하려는 일 | 도구 |
|---|---|
| 지급요청 저장 | `save_payment_request` |
| 지급 업체 프로필 등록·수정 | `upsert_payment_request_vendor_profile` |
| 업체 프로필 삭제·복구 | `delete_payment_request_vendor_profile`, `restore_payment_request_vendor_profile` |
| 개인경비 거래 저장 | `save_personal_expense_transaction` |
| 개인경비 일괄 수정 | `bulk_update_personal_expenses` |

- 업체 프로필 조회는 `list_payment_request_vendor_profiles`, `get_payment_request_vendor_profile`.
- **일괄 수정은 건수를 먼저 보여주고 범위를 확정받는다.** 샘플 3~5건을 함께 보여준다.
- 계좌·개인정보는 답변에 그대로 나열하지 않는다. 마스킹된 형태 그대로 다룬다.

## 4. 확인

저장 후 같은 조건으로 재조회해 반영을 확인하고, 상태·금액 집계 변화를 함께 보고한다.

## 조회 기준

- 기간은 **서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고 답변에 밝힌다.
- 답변에는 **기간·`date_type`·법인 범위·limit 적용 여부**를 함께 적는다. 화면 숫자와 다르면
  이 네 가지부터 대조한다.
