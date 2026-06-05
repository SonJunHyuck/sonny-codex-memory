# Codex 메모리 운영 흐름 결정 기록

이 파일은 Codex memory system 자체에 대한 오래 남길 결정을 기록한다.

## 2026-06-05: Git 기반 Memory Repo 사용

결정: `sonny-codex-memory`를 Codex 인수인계 컨텍스트를 위한 Git 기반 메모리 저장소로 사용한다.

이유:

- Codex chat history는 디바이스에 local로 남거나 특정 프로젝트 채팅에 묶일 수 있다.
- Sonny는 PC와 Mac을 오가면서도 프로젝트 컨텍스트를 잃지 않고 싶다.
- Git pull/push는 단순한 cross-device sync 메커니즘을 제공한다.

버린 대안:

- 한 디바이스의 raw local chat history에 의존한다.
- raw chat transcript를 주된 memory source로 저장한다.

영향:

- 작업은 메모리 저장소를 pull하고 관련 컨텍스트를 읽는 것으로 시작한다.
- 작업은 컨텍스트 파일을 업데이트하고 commit/push하는 것으로 끝난다.

## 2026-06-05: Memory와 구현 분리

결정: 메모리 저장소는 구현 workspace가 아니라 인수인계와 문서화 계층으로 취급한다.

이유:

- 구현 저장소에는 코드, 테스트, 프로젝트 산출물이 들어가야 한다.
- 메모리 저장소에는 왜, 무엇이 바뀌었는지, 무엇이 결정됐는지, 다음에 무엇을 해야 하는지가 들어가야 한다.
- 여러 프로젝트 컨텍스트가 구현 저장소를 오염시키지 않고 공존할 수 있다.

영향:

- BFX 구현은 `ProjectBFX`에서 진행한다.
- BFX EventLog 컨텍스트는 `contexts/bfx/eventlog/`에 둔다.

## 2026-06-05: 프로젝트와 토픽 단위로 컨텍스트 정리

결정: 메모리 컨텍스트 디렉토리는 `contexts/<project>/<topic>/` 구조를 사용한다.

이유:

- 하나의 프로젝트에도 여러 active topic이 생길 수 있다.
- 각 토픽에는 독립적인 현재 상태, 결정 기록, 타임라인, Notion 재료가 필요하다.

영향:

- `bfx.eventlog`는 `contexts/bfx/eventlog/`로 연결된다.
- `codex-memory.workflow`는 `contexts/codex-memory/workflow/`로 연결된다.

## 2026-06-05: Local Path는 디바이스별 정보로 유지

결정: 한 기기의 절대 경로를 기준 컨텍스트로 hard-code하지 않는다.

이유:

- Sonny는 PC와 Mac을 오갈 예정이다.
- `/Users/...` 같은 절대 경로는 한 디바이스에서만 의미가 있다.
- 저장소 identity는 디바이스를 넘어 안정적이어야 한다.

영향:

- `context_id`, `repo_id`, 원격 주소를 안정적인 식별자로 사용한다.
- 디바이스별 체크아웃 경로는 `_index/local-paths.md`에만 저장한다.

## 2026-06-05: Session 기록은 가볍게 유지

결정: `sessions/`는 exhaustive log가 아니라 짧은 원천 기록으로 유지한다.

이유:

- 일상적인 이어가기는 대부분 `current.md`, `decisions.md`, `timeline.md`만으로 충분하다.
- 나중에 Notion 문서를 만들 때는 무엇이 왜 일어났는지에 대한 짧은 기록이 도움이 된다.
- 전체 chat log는 너무 시끄럽고 기본적으로 저장할 필요가 없다.

영향:

- 세션 기록은 의도, 변경, 결정, Notion 재료, 다음 작업을 기록한다.
