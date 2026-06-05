---
context_id: codex-memory.workflow
project: Codex 메모리
topic: 운영 흐름
status: active
memory_repo:
  repo_id: sonny-codex-memory
  remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: sonny-codex-memory
    name: sonny-codex-memory
    remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
---

# Codex 메모리 운영 흐름 현재 컨텍스트

## 목적

이 컨텍스트는 Sonny의 Git 기반 Codex 메모리 운영 흐름 설계를 기록한다.

목표는 raw local chat history에 의존하지 않고, 오래 남길 컨텍스트를 담은 가벼운 저장소를 통해 Codex가 PC, Mac, 여러 디바이스, 프로젝트 채팅, 세션을 오가며 작업을 이어갈 수 있게 하는 것이다.

## 현재 모델

- `sonny-codex-memory`는 오래 남길 인수인계 컨텍스트, 결정 기록, 타임라인, 세션 기록, 템플릿, 미래 Notion 문서화 재료를 저장한다.
- 실제 구현 작업은 `ProjectBFX` 같은 관련 프로젝트 저장소에서 진행한다.
- 메모리 저장소는 작업 세션을 시작할 때 읽고, 끝낼 때 업데이트한다.
- 컨텍스트는 `contexts/<project>/<topic>/` 아래에 프로젝트와 토픽 단위로 정리한다.
- 로컬 체크아웃 경로는 디바이스마다 다르므로 기준 경로로 취급하지 않는다.

## 표준 컨텍스트 파일

- `current.md`: 작업을 이어가기 위해 필요한 최신 상태.
- `decisions.md`: 오래 남길 결정, 이유, 대안, 영향.
- `timeline.md`: 주요 마일스톤과 방향 전환.
- `notion.md`: 미래 Notion page를 위한 구조화된 재료.
- `sessions/YYYY/MM/*.md`: 특정 작업 세션에서 나온 가벼운 원천 기록.

## 디바이스 독립 경로 모델

안정적인 식별자:

- `context_id`: `bfx.eventlog`, `codex-memory.workflow`처럼 토픽 컨텍스트를 식별한다.
- `repo_id`: `sonny-codex-memory`, `project-bfx`처럼 디바이스를 넘어 저장소를 식별한다.
- `remote`: Git 저장소를 식별한다.

디바이스별 힌트:

- `_index/local-paths.md`는 Mac과 Windows PC의 로컬 체크아웃 경로를 저장한다.
- 경로가 없거나 오래됐으면 Codex는 원격 주소로 저장소를 찾거나 Sonny에게 현재 디바이스의 경로를 물어본다.

## 운영 Prompt

작업 시작:

```text
<Project/topic> 작업 시작하자.
현재 디바이스의 sonny-codex-memory checkout을 pull하고,
<context_id> 컨텍스트를 읽어서 현재 상태 요약한 뒤 이어서 작업해줘.
```

작업 종료:

```text
오늘 작업 내용을 sonny-codex-memory의 <context_id> 컨텍스트에 요약하고 commit/push해줘.
```

## 다음 작업

- Windows PC 로컬 경로를 알게 되면 `_index/local-paths.md`에 채운다.
- `ProjectBFX` 원격 주소를 확인해서 `_index/repositories.md`에 추가한다.
- 새 프로젝트/토픽이 생기면 템플릿을 기반으로 해당 컨텍스트 디렉토리를 만든다.
- 나중에 `notion.md`, `decisions.md`, `timeline.md`, 세션 기록을 이용해 Notion 문서를 만든다.
