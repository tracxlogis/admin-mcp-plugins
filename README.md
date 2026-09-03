# TX Admin MCP Plugin

**한국어** | [English](#english)

TX 운영 어드민을 AI 어시스턴트에 연결하는 **사내용** 플러그인입니다. 설치하면 MCP 연결과 함께 운영
업무 스킬(정산·지급요청·증빙 매칭·KCI·HR·권한·시스템 모니터링·배송비 정책·거래처)이 한 번에
설정됩니다.

설치 후에는 이렇게 물어보면 됩니다. *"오늘 운영 이상 없나?"*, *"COD 수금 현황 보여줘"*,
*"이번 달 지급요청 상태별로 정리해줘"*

## 사용 전제

- **TX 관리자 계정** (평소 admin 에 로그인하는 그 계정)
- **사내망 접근** — 운영 MCP 는 사내 도메인이라 회사망 또는 VPN 이 필요합니다.
- **GitHub 접근** — 마켓플레이스는 GitHub 공개 저장소 `tracxlogis/admin-mcp-plugins`(표준 `owner/repo` 표기)로
  배포합니다. GitHub 계정이나 별도 인증 없이 인터넷만 있으면 등록·설치할 수 있습니다.
- 클라이언트 중 하나: Claude Code, Codex CLI, Claude 데스크톱 앱, ChatGPT 데스크톱 앱

## 설치

### Claude Code

```
/plugin marketplace add tracxlogis/admin-mcp-plugins
/plugin install txadmin@tx-admin-mcp-marketplace
```

### Codex CLI

```
codex plugin marketplace add tracxlogis/admin-mcp-plugins
codex plugin add txadmin@tx-admin-mcp-marketplace
```

### 데스크톱 앱

Claude 데스크톱 앱은 **설정 → 플러그인 → 추가 → 저장소에서 추가**, ChatGPT 데스크톱 앱은
**플러그인 → 추가 → 플러그인 마켓플레이스 추가** 에서 `tracxlogis/admin-mcp-plugins` 를 넣습니다.

## 인증

- **OAuth 로그인** — 브라우저가 열리면 TX 관리자 계정으로 로그인하고 접근 권한에 동의합니다.
- **개인 API 키** — 브라우저 왕복 없이 쓰려면 `/my-profile` 에서 발급한 개인 키(`txak_` 로 시작)를
  클라이언트 설정에 넣습니다. 키는 본인 것이며 공유하지 않습니다.

## 포함된 스킬

| 스킬 | 다루는 일 |
|---|---|
| `txadmin-ops-briefing` | 에러·배치·부하와 정산·승인 대기를 모아 우선순위로 보고 |
| `txadmin-settlement-cod` | COD 수금·배송비·기타비용 조회, 송금 생성·완료·되돌리기 |
| `txadmin-payment-requests` | 지급요청 목록·상세·저장, 업체 프로필, 개인경비 |
| `txadmin-invoice-matching` | 세금계산서·이체증 매칭, 문서함과 AI 분석 |
| `txadmin-kci-invoice` | KCI AP 문서 확인·확정, G-sabis 비교·전기, TxRM 결재요청 |
| `txadmin-hr-org` | 조직·사원·권한, AD/Entra/M365 계정 처리 |
| `txadmin-access-control` | 메뉴·기능 권한, 운영자 그룹, AD 그룹 |
| `txadmin-system-monitoring` | 앱 로그·배치 스케줄러·SP 실행로그와 세션 종료 |
| `txadmin-shipping-fee-policy` | 배송비 정책·계약 요율, 추천·알람 |
| `txadmin-partners-contracts` | 거래처, 벤더 계약·요율, 스마트루트, API 인증키 |

## 안전장치

- **데이터를 바꾸는 작업은 모두 명시적 확인이 필요합니다.** 어시스턴트가 대상·금액·영향 범위를
  요약해 보여주고, 동의한 뒤에만 실행됩니다. 위험도가 높은 작업은 변경 사유도 함께 받습니다.
- 권한은 **호출 시점에 서버가 판정**합니다. 화면에서 권한이 없는 기능은 여기서도 거절됩니다.
- 조회 기간에는 **서버 기본값이 없습니다.** 어시스턴트가 사용한 기간·기준 컬럼·법인 범위를 답변에
  밝히도록 스킬에 규정돼 있습니다.
- 계좌·개인정보·인증키 값은 답변에 나열하지 않습니다.

## 문의

연결 가이드: <https://admin-react.tracxlogis.com/mcp-user-guide>

---

## English

[한국어](#tx-admin-mcp-plugin) | **English**

An **internal** plugin that connects the TX operations admin to your AI assistant. Installing it sets
up the MCP connection together with operations skills — settlement, payment requests, document
matching, KCI invoices, HR, access control, system monitoring, shipping-fee policy, and partners.

Once installed you can ask: *"Anything wrong in operations today?"*, *"Show me the COD collection
status"*, *"Summarize this month's payment requests by status"*

### Requirements

- A **TX admin account** — the one you normally use to sign in to admin.
- **Internal network access** — the production MCP server is on an internal domain, so you need the
  office network or VPN.
- **GitHub access** — the marketplace is distributed from the public GitHub repository
  `tracxlogis/admin-mcp-plugins` (standard `owner/repo` form). Registering and installing need only an
  internet connection; no GitHub account or extra authentication is required.
- One of these clients: Claude Code, Codex CLI, Claude desktop app, ChatGPT desktop app.

### Install

Claude Code:

```
/plugin marketplace add tracxlogis/admin-mcp-plugins
/plugin install txadmin@tx-admin-mcp-marketplace
```

Codex CLI:

```
codex plugin marketplace add tracxlogis/admin-mcp-plugins
codex plugin add txadmin@tx-admin-mcp-marketplace
```

For the desktop apps, enter `tracxlogis/admin-mcp-plugins` as the repository from **Settings → Plugins → Add → Add from
repository** (Claude) or **Plugins → Add → Add plugin marketplace** (ChatGPT).

### Authentication

- **OAuth sign-in** — a browser window opens; sign in with your TX admin account and approve access.
- **Personal API key** — to skip the browser round trip, issue a personal key (it starts with `txak_`)
  from `/my-profile` and put it in your client configuration. The key is yours alone; do not share it.

### Included skills

| Skill | Covers |
|---|---|
| `txadmin-ops-briefing` | Errors, batches, DB load, and settlement or approval backlogs in priority order |
| `txadmin-settlement-cod` | COD collection and fees, remittance creation, completion and rollback |
| `txadmin-payment-requests` | Payment requests, vendor profiles, personal expenses |
| `txadmin-invoice-matching` | Tax invoice and transfer receipt matching, document AI analysis |
| `txadmin-kci-invoice` | KCI AP documents, G-sabis comparison and posting, TxRM approval requests |
| `txadmin-hr-org` | Organization, employees, authorizations, AD/Entra/M365 accounts |
| `txadmin-access-control` | Menu and function permissions, operator groups, AD groups |
| `txadmin-system-monitoring` | Application logs, job scheduler, SP execution logs, session kill |
| `txadmin-shipping-fee-policy` | Shipping fee policies and contract rates, recommendations, alarms |
| `txadmin-partners-contracts` | Business partners, vendor contracts and rates, smart routes, API keys |

### Safeguards

- **Every action that changes data requires explicit confirmation.** The assistant summarizes the
  targets, amounts, and blast radius, and runs only after you agree. High-risk actions also record a
  reason.
- Permissions are **evaluated by the server on each call**. Anything you cannot do on the screens is
  refused here too.
- The server applies **no default query period**. The skills require the assistant to state the
  period, the date basis, and the company scope it used.
- Bank accounts, personal data, and API key values are never listed in answers.

### Support

Connection guide: <https://admin-react.tracxlogis.com/mcp-user-guide>
