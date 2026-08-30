---
name: txadmin-invoice-matching
description: TX 세금계산서·이체증 매칭과 문서 관리 — 세금계산서/이체증 목록·요약·매칭 후보 조회, AP 인보이스 검색, 매칭 연결과 상태 변경, 이체증 항목 추출·중복 표시, 문서함 조회와 AI 분석·유형 변경·수집. "세금계산서 매칭해줘", "이체증 확인", "미매칭 건 보여줘", tax invoice matching, transfer receipt, document AI 요청에 사용.
---

# TX 세금계산서·이체증 매칭

증빙(세금계산서·이체증)을 AP 인보이스에 연결하는 업무다. 순서:
**① 미매칭 조회 → ② 후보 확인 → ③ 연결 → ④ 상태 정리**.

## 1. 조회

- 세금계산서: `list_tax_invoices`, `get_tax_invoice_summary`
- 이체증: `list_transfer_receipts`, `get_transfer_receipt_summary`
- 매칭 후보: `list_tax_invoice_match_candidates`, `list_transfer_match_candidates`
- 상대편 인보이스 검색: `search_ap_invoices_for_tax`, `search_ap_invoices_for_transfer`
- 문서함: `list_document_mgt_tree`, `get_document_ai_master`, `get_document_ai_upload`

## 2. 매칭 (쓰기 — 전부 `confirm=true` 필요)

**잘못 연결하면 회계 귀속이 어긋난다.** 실행 전 증빙 번호·금액·거래처·대상 인보이스를
한 줄로 요약해 보여주고 동의를 받는다.

| 하려는 일 | 도구 |
|---|---|
| 세금계산서 매칭 연결 | `link_tax_invoice_match` |
| 세금계산서 매칭 상태 변경 | `update_tax_invoice_match_status` |
| 이체증 매칭 연결 | `link_transfer_receipt_match` |
| 이체증 매칭 상태 변경 | `update_transfer_match_status` |
| 이체증 항목 추출 | `extract_transfer_receipt_fields` |
| 이체증 중복 표시 | `mark_transfer_receipt_duplicate` |
| 삭제 | `delete_tax_invoices`, `delete_transfer_receipts` |

- **삭제는 마지막 수단**이다. 중복이면 삭제 대신 중복 표시를 먼저 검토한다.
- 금액이 정확히 일치하지 않는 후보를 연결할 때는 차액을 명시해 보고한다.

## 3. 문서 처리 (쓰기 — `confirm=true` 필요)

- `analyze_document_file_uploads`, `analyze_invoice_uploads` — 업로드 문서 AI 분석.
  **분석은 비용과 시간이 든다** — 대상 건수를 먼저 보여준다.
- `change_document_type` — 문서 유형 변경. 유형이 바뀌면 이후 처리 경로가 달라진다.
- `collect_documents_by_ticket` — 티켓 기준 문서 수집.
- `change_document_etax_collect_flag` — 전자세금계산서 수집 대상 여부 변경.
- `delete_document_uploads` — 업로드 삭제. 되돌릴 수 없다.

## 4. 확인

처리 후 목록·요약을 **같은 조건으로 재조회**해 매칭 상태가 실제로 바뀌었는지 확인하고 보고한다.
여러 건을 처리했으면 성공·실패 건수를 나눠 사실대로 알린다.

## 조회 기준

- 기간은 **서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고 답변에 밝힌다.
- 법인·국가 범위를 비우면 권한이 허용하는 전 범위가 섞인다 — 어느 범위인지 밝힌다.
- 화면 건수와 다르면 기간, 매칭 상태 필터, 법인 범위부터 대조한다.
