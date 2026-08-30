---
name: txadmin-kci-invoice
description: TX KCI AP 문서·인보이스 — KCI AP 문서 목록·상세, AI 추출 원문과 G-sabis 비교 뷰, 라인 변경 이력 조회, 확정·확정취소·취소·마감·초기화·삭제, 대상일자 설정, TxRM 결재요청 생성, G-sabis 매입 인보이스 조회·비용 전기. "KCI 인보이스 확인", "AP 문서 확정해줘", "G-sabis 비교", KCI AP document, gsabis 요청에 사용.
---

# TX KCI AP 문서·인보이스

KCI 매입 인보이스(AP) 문서를 확인하고 확정·전기하는 업무다. 순서:
**① 문서 조회 → ② AI 추출값과 원천 비교 → ③ 확정 → ④ 결재·전기**.

## 1. 조회·검증

- `list_kci_ap_documents` — AP 문서 목록.
- `get_kci_ap_document_detail` — 건별 상세.
- `get_kci_invoice_compare_view` — **AI 추출값과 G-sabis 원천 비교.** 확정 전 반드시 본다.
- `get_kci_invoice_ai_raw` — AI 추출 원문. 값이 이상할 때 근거 확인용.
- `get_kci_invoice_line_history` — 라인 변경 이력.
- `fetch_kci_gsabis_purchase_invoice` — G-sabis 매입 인보이스 조회.

**AI 추출값을 그대로 믿고 확정하지 않는다.** 비교 뷰에서 금액·수량·공급처가 원천과 맞는지 확인하고,
차이가 있으면 무엇이 다른지 사용자에게 보여준다.

## 2. 상태 처리 (쓰기 — 전부 `confirm=true` 필요)

| 하려는 일 | 도구 | 주의 |
|---|---|---|
| 확정 | `confirm_kci_ap_documents` | 확정 후 회계 처리로 이어진다. 대상 문서번호를 하나씩 확인받는다 |
| 확정 취소 | `unconfirm_kci_ap_documents` | 이미 전기됐으면 되돌릴 수 없을 수 있다 — 상태를 먼저 조회 |
| 취소 | `cancel_kci_ap_documents` | |
| 마감 | `close_kci_ap_documents` | 마감 후에는 수정이 막힌다 |
| 초기화 | `reset_kci_ap_documents` | 입력값이 지워질 수 있다. 무엇이 지워지는지 먼저 설명 |
| 삭제 | `delete_kci_ap_documents` | 되돌릴 수 없다 |
| 대상일자 설정 | `set_kci_ap_target_date` | 귀속 기간이 바뀐다 |

## 3. 결재·전기 (쓰기 — 영향 큼)

- `create_kci_txrm_report` — TxRM 결재요청 생성. **결재선에 알림이 나간다.** 제출 전 대상 문서·금액·
  결재 문서 유형을 요약해 동의를 받는다.
- `post_kci_invoice_expense_to_gsabis` — G-sabis 로 비용 전기. **외부 회계 시스템에 기록이 생긴다.**
  되돌리기가 어려우므로 대상과 금액을 건별로 확인받는다.

## 4. 확인

처리 후 `list_kci_ap_documents` / `get_kci_ap_document_detail` 로 재조회해 상태가 실제로 바뀌었는지
확인하고 보고한다. 일부만 성공했으면 건별로 나눠 알린다.

## 조회 기준

- 기간·대상일자는 **서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고 답변에 밝힌다.
- 화면과 건수가 다르면 기간 기준(등록일/대상일자), 상태 필터, 법인 범위부터 대조한다.
