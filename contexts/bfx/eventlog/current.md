---
context_id: bfx.eventlog
project: BFX
topic: EventLog
status: active
memory_repo:
  repo_id: sonny-codex-memory
  remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: project-bfx
    name: ProjectBFX
    remote: TBD
---

# BFX EventLog 현재 컨텍스트

## 목적

이 컨텍스트는 Codex가 하나의 local chat history에 의존하지 않고, 여러 디바이스와 프로젝트 채팅, 세션을 오가며 BFX EventLog 작업을 이어갈 수 있게 하기 위해 존재한다.

메모리 저장소는 구현 workspace가 아니다. 무엇이 결정됐는지, 현재 어디까지 왔는지, 다음에 무엇을 해야 하는지 같은 오래 남길 컨텍스트를 저장한다.

## 운영 흐름

BFX 작업은 BFX 프로젝트 채팅에서 다음과 같은 prompt로 시작한다.

```text
BFX 작업 시작하자.
먼저 현재 디바이스의 sonny-codex-memory checkout을 pull하고,
bfx.eventlog 컨텍스트 읽어서 현재 상태 요약한 뒤
이 BFX 프로젝트에서 이어서 작업해줘.
```

예상되는 Codex 흐름:

1. repo id `sonny-codex-memory`의 로컬 체크아웃을 찾고 pull한다.
2. `contexts/bfx/eventlog/current.md`, `decisions.md`, `timeline.md`를 읽는다.
3. repo id `project-bfx`로 실제 BFX 프로젝트 저장소를 찾아 읽는다.
4. 요청된 경우 프로젝트 저장소에서 구현, 테스트, commit을 진행한다.
5. 작업이 끝나면 이 메모리 컨텍스트를 업데이트하고 메모리 저장소를 push한다.

## 현재 이해

- `sonny-codex-memory`는 장기 메모리와 인수인계 저장소다.
- `ProjectBFX`는 실제 구현 저장소다.
- 로컬 체크아웃 경로는 디바이스마다 다르며 기준 경로로 취급하지 않는다.
- 컨텍스트는 프로젝트와 토픽 단위로 정리한다.
- BFX EventLog의 기준 컨텍스트 경로는 `contexts/bfx/eventlog/`다.
- 핵심 작업 파일은 `current.md`, `decisions.md`, `timeline.md`, `notion.md`다.

## 중요한 파일

- `current.md`: 현재 상태와 다음 인수인계 요약.
- `decisions.md`: 오래 남길 결정, 이유, 대안, 영향.
- `timeline.md`: 주요 변경과 마일스톤의 시간순 기록.
- `notion.md`: 나중에 Notion 문서화를 위한 구조화된 재료.
- `sessions/YYYY/MM/*.md`: 개별 작업 세션의 가벼운 원천 기록.
- `_index/repositories.md`: 안정적인 저장소 identity와 원격 주소.
- `_index/local-paths.md`: 디바이스별 체크아웃 경로 힌트.

## 다음 작업

- `ProjectBFX` 원격 주소를 확인한다.
- Windows PC 체크아웃 경로를 알게 되면 `_index/local-paths.md`에 추가한다.
- BFX EventLog 구현 작업이 시작되면 프로젝트 코드를 읽고, 이 계획 컨텍스트를 실제 구현 컨텍스트로 갱신한다.
- 세션 기록은 짧고 유용하게 유지하되, 결정이나 Notion으로 옮길 만한 이유가 생긴 세션은 꼭 기록한다.
