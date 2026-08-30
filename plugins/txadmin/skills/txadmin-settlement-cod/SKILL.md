---
name: txadmin-settlement-cod
description: TX COD 정산 — 수금(Collection)·배송비·기타비용·미수 보류 조회와 송금(Remittance) 생성·완료·되돌리기·삭제, 가감 설정, 인보이스 확정, 재집계. "COD 수금 현황", "송금 생성해줘", "정산 되돌려줘", COD collection, remittance, settlement 요청에 사용.
---

# TX COD 정산

COD 수금과 송금(Remittance) 업무를 수행한다. 순서:
**① 수금 대상 조회 → ② 송금 생성 → ③ 가감·보류 반영 → ④ 완료 확정**. 각 단계는 되돌리는 도구가 따로 있다.

## 1. 조회

- `list_cod_collection` — 수금 목록. **기간 기준 컬럼(`date_type`)과 시작·종료일이 필수**다.
  배송완료일/수금일/발송일/송금예정일 중 무엇을 기준으로 볼지 먼저 정한다.
- `list_cod_shipping_fee` — 배송비 정산 대상.
- `list_cod_remittance`, `list_cod_remittance_detail` — 생성된 송금과 상세.
- `list_cod_others_fee`, `search_cod_others_fee_invoices` — 기타비용과 그 인보이스.
- `list_cod_hold_outstanding`, `list_cod_hold_outstanding_targets` — 미수 보류 현황과 등록 대상.

## 2. 처리 (쓰기 — 전부 `confirm=true` 필요)

**돈이 오가는 정산이다.** 실행 전 대상 건수·금액·국가·기간을 요약해 보여주고 사용자 동의를 받는다.
위험도가 높은 도구는 변경 사유(`reason`)도 함께 받는다.

| 단계 | 도구 |
|---|---|
| 송금 생성 | `generate_cod_remittance`, `generate_cod_shipping_fee_remittance` |
| 가감·기타비용·보류 반영 | `set_cod_remittance_adding_shipping_fee`, `set_cod_remittance_others_fee`, `set_cod_remittance_hold_outstanding`, `auto_register_cod_remittance_hold_outstanding` |
| 인보이스 확정 | `confirm_cod_remittance_invoice` |
| 완료 처리 | `complete_cod_remittance` |
| 되돌리기 | `return_cod_remittance_to_collected` |
| 삭제 | `delete_cod_remittance`, `delete_cod_remittance_collected_rows` |
| 재집계 | `recount_cod_collection_list` |

- **완료(complete)·삭제는 영향이 크다.** 대상 송금 번호를 하나씩 확인받고 실행한다.
- 되돌리기(`return_cod_remittance_to_collected`)가 가능한 단계인지 먼저 상태를 조회해 확인한다.
- 자동 등록(`auto_register_...`)은 조건에 맞는 건을 한꺼번에 등록한다 — 대상 건수를 먼저 보여준다.

## 3. 확인

처리 후 `list_cod_remittance` / `list_cod_remittance_detail` 로 **같은 조건으로 재조회**해
상태·금액이 실제로 바뀌었는지 확인하고 보고한다. 부분 실패를 전체 성공으로 말하지 않는다.

## 조회 기준

- 기간과 기준 컬럼은 **서버 기본값이 없다.** 사용자가 지정하지 않으면 먼저 제시하고,
  답변에 사용한 기간·기준 컬럼·국가를 밝힌다.
- 국가 코드를 비우면 전체가 아니라 도구 기본값이 적용될 수 있다 — 어느 국가로 조회했는지 밝힌다.
- 화면 금액과 다르면 기간 기준 컬럼, 국가, 수금여부·정산생성여부 필터부터 대조한다.
