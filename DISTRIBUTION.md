# Distribution notes / 배포 안내

**한국어** | [English](#english)

이 저장소(`tracxlogis/admin-mcp-plugins`)는 TX 운영 어드민 MCP 플러그인의 **배포본**이다. 정본은 사내
`tx-admin-react` 저장소의 `mcp-marketplace/` 디렉터리이며, 스킬·매니페스트는 그쪽에서 고쳐 이곳으로
동기화한다 — MCP 도구 레지스트리(`src/lib/mcp/registry.ts`) 변경과 그 도구를 설명하는 스킬이 항상 같은
커밋에 들어가게 하기 위해서다.

- 사용자 설치 방법: [README.md](README.md) 참고. 마켓플레이스는 표준 `owner/repo` 표기
  `tracxlogis/admin-mcp-plugins` 로 등록한다.
- 연결 가이드와 자세한 사용법: <https://admin-react.tracxlogis.com/mcp-user-guide>
- 동기화 방법(정본 저장소 루트에서):

  ```bash
  git subtree push --prefix=mcp-marketplace https://github.com/tracxlogis/admin-mcp-plugins.git main
  ```

이 저장소로 직접 Pull Request 를 보내지 않는다 — 다음 동기화 때 정본 내용으로 덮어써진다.

---

## English

[한국어](#distribution-notes--배포-안내) | **English**

This repository (`tracxlogis/admin-mcp-plugins`) is the **distribution copy** of the TX operations admin
MCP plugin. The source of truth is the `mcp-marketplace/` directory inside the internal `tx-admin-react`
repository; skills and manifests are edited there and synced here, so that a change to the MCP tool
registry (`src/lib/mcp/registry.ts`) and the skills that describe it always land in the same commit.

- Install instructions for users: see [README.md](README.md). Register the marketplace with the standard
  `owner/repo` form, `tracxlogis/admin-mcp-plugins`.
- Guide and detailed usage: <https://admin-react.tracxlogis.com/mcp-user-guide>
- How to sync (from the root of the source repository):

  ```bash
  git subtree push --prefix=mcp-marketplace https://github.com/tracxlogis/admin-mcp-plugins.git main
  ```

Please do not open pull requests against this repository — changes made here would be overwritten by the
next sync from the source of truth.
