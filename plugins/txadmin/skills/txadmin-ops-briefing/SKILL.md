---
name: txadmin-ops-briefing
description: TX 운영 어드민 업무 브리핑 — 앱 에러 로그, 배치 스케줄러 실패·MQ 에러, 느린 SP, 지급요청·개인경비 상태 집계, 세금계산서·이체증 미매칭, 거래처 계정 승인 대기를 한 번에 모아 우선순위로 보고. "오늘 운영 이상 없나", "처리할 일 정리해줘", "에러 많이 났어?", admin briefing, operations status 요청에 사용.
---

# TX 운영 어드민 업무 브리핑

운영자가 화면 여러 개를 돌지 않고 지금 상태를 파악하도록, 여러 도메인의 집계를 모아
**우선순위 순으로** 보고한다.

## 수집

| 항목 | 도구 |
|---|---|
| MCP 연결·내 권한 확인 | `ping_admin_mcp` |
| 앱 에러 로그 | `get_app_log_list`, `get_app_error_log_team_report` |
| 배치 스케줄러 | `get_job_scheduler_list`, `get_job_scheduler_mq_error_log` |
| 느린 SP·DB 부하 | `get_top_sp_list` |
| 지급요청 현황 | `get_payment_request_status_summary` |
| 개인경비 현황 | `get_personal_expense_status_summary` |
| 세금계산서·이체증 | `get_tax_invoice_summary`, `get_transfer_receipt_summary` |
| 거래처 계정 승인 대기 | `get_biz_partner_account_approval_status_summary` |

사용자의 팀·업무에 따라 관련 항목만 골라 조회한다. 권한이 없는 도메인은 서버가 거절하므로
그 항목은 **"권한 없음"** 으로 표시하고 나머지를 계속 보고한다.

## 보고 형식

1. **지금 조치 필요** — 실패한 배치, 미해결 에러 급증, 승인·매칭 대기가 임계치를 넘은 항목.
2. **오늘 처리할 일** — 도메인별 대기 건수와 다음 액션 한 줄.
3. **참고 현황** — 추세·부하 지표.

- 0건 항목은 생략한다. 전부 0이면 "특이사항 없음"을 분명히 말한다.
- 조회 하나가 실패해도 나머지는 보고하고, 실패 항목은 **"확인 불가"** 로 표시한다. 0으로 바꾸지 않는다.
- 사용자가 "그거 처리해줘"라고 하면 해당 도메인 스킬(`txadmin-settlement-cod`,
  `txadmin-payment-requests`, `txadmin-system-monitoring` 등)의 절차로 넘어간다. 브리핑 안에서
  바로 쓰기 도구를 호출하지 않는다.

## 조회 기준

- **이 서버의 목록·로그 도구는 기간이 필수이고 서버 기본값이 없다.** 사용자가 기간을 말하지 않으면
  조회 전에 기간을 제시하고(예: 오늘 / 최근 7일), **답변에 사용한 기간과 기준을 반드시 밝힌다.**
- 법인·국가 범위를 비우면 권한이 허용하는 전 법인이 섞여 나온다. 어느 범위로 봤는지 밝힌다.
- 화면 숫자와 다르면 기간, 기준 컬럼(등록일/완료일 등), 법인 범위부터 대조한다.
