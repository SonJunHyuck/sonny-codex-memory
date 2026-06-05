# 2026-06-05 codex-memory.workflow

## 의도

Sonny의 Codex 메모리 저장소 설계 대화를 독립적인 project/topic 컨텍스트로 기록한다.

## 변경

- `codex-memory.workflow`를 활성 컨텍스트로 추가했다.
- `contexts/codex-memory/workflow/`를 만들었다.
- 메모리 저장소의 목적, 디바이스 독립 저장소 identity model, 결정, 타임라인, Notion 재료를 기록했다.

## 결정

- 메모리 운영 흐름 자체를 first-class project context로 취급한다.
- `context_id: codex-memory.workflow`를 사용한다.
- `repo_id`, 원격 주소, `_index/local-paths.md`를 통해 경로 처리를 디바이스 독립적으로 유지한다.

## Notion 재료

- 미래 page title: Codex 메모리 운영 흐름.
- 이 시스템을 Codex 컨텍스트를 위한 Git 기반 인수인계 계층으로 설명한다.
- 메모리 저장소와 구현 저장소의 분리를 강조한다.
- 로컬 경로가 기준 정보가 아니라 힌트인 이유를 설명한다.

## 다음

- Windows PC 로컬 경로 힌트를 알게 되면 추가한다.
- `ProjectBFX` 원격 주소가 확인되면 추가한다.
- 이 패턴이 자주 반복되면 새 컨텍스트 checklist나 script를 고려한다.
