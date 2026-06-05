# BFX EventLog 타임라인

## 2026-06-05

- Codex 대화가 디바이스나 프로젝트 컨텍스트에 local로 묶일 수 있다는 문제를 논의했다.
- 디바이스를 넘는 이어가기를 위해 Git 기반 메모리 저장소 방식을 선택했다.
- `sonny-codex-memory`를 메모리와 인수인계 저장소로 만들었다.
- 메모리 저장소에서 GitHub SSH pull/push가 동작할 수 있음을 확인했다.
- 실제 BFX 구현은 BFX 프로젝트 저장소에서 진행하고, 이 저장소에는 오래 남길 컨텍스트를 저장하기로 했다.
- 컨텍스트를 project/topic 구조로 정리하기로 했다.
- `sessions/`는 나중의 복기와 Notion 문서화를 위한 가벼운 기록으로 유지하기로 했다.
- 미래 Notion page를 위한 topic-level staging document로 `notion.md`를 추가했다.
