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

## 2026-06-09

- Windows PC의 실제 BFX 체크아웃 경로가 `C:\Users\ahkff\Project-BFX`임을 확인했다.
- `project-bfx` 원격 주소가 `https://github.com/redforce01/Project-BFX.git`임을 확인했다.
- BFX 최근 커밋에서 `81406770 [Campaign] HUD Event Log 시스템 추가`를 확인했다.
- Campaign HUD EventLog 구현이 `CampaignEventTrigger`, `BfxCampaignEventLogService`, `BfxCampaignEventLogView`, GPM InfiniteScroll 어댑터들로 구성되어 있음을 정리했다.
- EventLog 컨텍스트를 계획/더미 내용에서 실제 구현 기반 인수인계 문서로 갱신했다.
- Unity Play 흐름에서 EventLog 정상작동을 확인했다.
- 프리팹 연결 확인 기준을 `BfxCampaignEventLogView` -> GPM `InfiniteScroll` -> EventLog row prefab -> TMP text 참조 체인으로 정리했다.
- `BfxCampaignEventLogService`는 scene 전환 시 보존하지 않고, `DontDestroyOnLoad` 없이 Campaign scene 수명주기를 따르기로 결정했다.
- 현지화/데이터화, `_maxEntryCount` 조정, 저장/로드는 이번 EventLog 범위에서 진행하지 않기로 했다.
- BFX EventLog 토픽을 완료 상태로 종결했다.
