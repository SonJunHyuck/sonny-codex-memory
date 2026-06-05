# 활성 컨텍스트

이 파일은 현재 활성화된 Codex 메모리 컨텍스트를 찾기 위한 시작 지도다.

## 활성 목록

| Context ID | 프로젝트 | 토픽 | 현재 컨텍스트 | 상태 |
| --- | --- | --- | --- | --- |
| `bfx.eventlog` | BFX | EventLog | [current.md](../contexts/bfx/eventlog/current.md) | 활성 |
| `codex-memory.workflow` | Codex 메모리 | 운영 흐름 | [current.md](../contexts/codex-memory/workflow/current.md) | 활성 |

## 운영 모델

- 코드를 구현할 때는 실제 프로젝트 저장소의 채팅에서 작업한다.
- 이 메모리 저장소는 장기 인수인계와 컨텍스트 계층으로 사용한다.
- 작업을 시작할 때는 현재 디바이스의 로컬 체크아웃에서 이 저장소를 pull하고 관련 컨텍스트를 읽는다.
- 작업을 끝낼 때는 컨텍스트를 업데이트하고, 이 저장소를 commit/push한다.
- 로컬 filesystem path는 Mac과 PC에서 다를 수 있다. 안정적인 저장소 식별자는 `_index/repositories.md`를 보고, `_index/local-paths.md`는 디바이스별 경로 힌트로만 사용한다.

## 알려진 프로젝트 저장소

| 프로젝트 | Repo ID | 역할 |
| --- | --- | --- |
| BFX | `project-bfx` | 실제 구현 저장소 |
| sonny-codex-memory | `sonny-codex-memory` | Codex 메모리와 인수인계 저장소 |
