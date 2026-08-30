---
name: txadmin-system-monitoring
description: TX 시스템 운영 모니터링 — 앱 로그·에러 서브로그·팀/사용자별 에러 리포트와 해결 처리, 배치 스케줄러 목록·상세·로그·MQ 에러 처리, SP 실행로그·DB 쿼리 세션·느린 SP Top·세션 종료. "에러 로그 확인", "배치 실패했어?", "느린 SP 찾아줘", "이 세션 죽여줘", app log, job scheduler, slow SP, kill session 요청에 사용.
---

# TX 시스템 운영 모니터링

앱 에러·배치·DB 부하를 확인하고 조치하는 업무다. 순서:
**① 증상 범위 확인 → ② 상세 로그로 원인 좁히기 → ③ 조치**.

## 1. 앱 로그

- `get_app_log_list` — 애플리케이션 로그. **시작·종료 일시가 필수**(형식 `YYYY-MM-DD HH:mm:ss`)다.
- `get_app_log_detail` — 건별 상세.
- `get_app_error_sub_log_list`, `get_app_error_sub_log_detail` — 에러 서브로그.
- `get_app_error_log_team_report`, `get_app_error_log_user_report` — 팀·사용자별 집계.
- 조치(쓰기, `confirm=true`): `set_app_log_error_solve`, `set_app_log_error_solve_team`,
  `set_app_log_job_subtotal`, `run_app_log_auto_mapping`
  - 해결 처리는 **다른 팀의 담당 상태를 바꿀 수 있다** — 대상 건수와 팀을 밝히고 동의받는다.

## 2. 배치 스케줄러

- `get_job_scheduler_list`, `get_job_scheduler_detail`, `get_job_scheduler_log`
- MQ 에러: `get_job_scheduler_mq_error_log`, `get_job_scheduler_mq_error_log_detail`
- 조치(쓰기, `confirm=true`): `mutate_job_scheduler`(등록·수정·상태 변경),
  `request_edit_job_scheduler`(변경 요청), `set_job_scheduler_mq_error_solve`
  - **스케줄러 변경은 운영 배치의 실행 시각·동작을 바꾼다.** 무엇을 어떻게 바꾸는지 값 단위로
    보여주고, 되돌리는 방법까지 함께 설명한 뒤 실행한다.

## 3. DB·SP 부하

- `get_sp_exec_log` — SP 실행 로그.
- `get_db_query_session_by_time`, `get_db_query_object_by_time` — 시간대별 세션·오브젝트.
- `get_iauditor_query_session`, `get_iauditor_query_object` — iAuditor 기준 조회.
- `get_qlps_sp_runtime_sessions`, `get_kci_oracle_sp_runtime_sessions` — 현재 실행 중 세션.
- `get_top_sp_list`, `capture_top_sp` — 느린 SP 상위 목록과 스냅샷 저장.

### 세션 종료 — 최상위 위험

`kill_db_session` 은 **운영 DB 세션을 강제 종료한다.** 진행 중 트랜잭션이 롤백되고 사용자 작업이
끊긴다. 다음을 모두 지킨다.

1. 먼저 실행 중 세션 조회로 **무엇을 죽이는지**(SPID·SP·실행 시간·호출자) 확인한다.
2. 세션 정보와 예상 영향을 사용자에게 그대로 보여준다.
3. 사용자가 **명시적으로 종료를 지시한 경우에만** 실행한다. 추정으로 실행하지 않는다.
4. 실행 후 다시 조회해 실제로 사라졌는지 확인하고 보고한다.

## 조회 기준

- 로그·세션 도구는 **기간이 필수이고 서버 기본값이 없다.** 지정하지 않으면 먼저 제시하고
  (예: 최근 1시간 / 오늘), **답변에 사용한 기간을 반드시 밝힌다.**
- 시간대 해석이 어긋나면 건수가 통째로 달라진다 — 화면과 다르면 기간·시간대부터 대조한다.
- 조회 상한(top 건수)이 있으면 잘렸는지 여부를 함께 알린다.
