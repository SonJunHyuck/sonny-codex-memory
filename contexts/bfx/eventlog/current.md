---
context_id: bfx.eventlog
project: BFX
topic: EventLog
status: completed
memory_repo:
  repo_id: sonny-codex-memory
  remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: project-bfx
    name: Project-BFX
    remote: https://github.com/redforce01/Project-BFX.git
---

# BFX EventLog 현재 컨텍스트

## 목적

이 컨텍스트는 BFX Campaign HUD EventLog 구현 상태를 Codex가 여러 디바이스와 세션에서 이어갈 수 있도록 남긴 인수인계 문서다.

`sonny-codex-memory`는 구현 workspace가 아니다. 실제 코드는 `Project-BFX`에 있고, 이 파일에는 현재 구조, 주요 파일, 결정, 다음 작업처럼 오래 남길 컨텍스트만 둔다.

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

- 실제 구현 저장소는 `Project-BFX`이고, Windows PC 로컬 경로는 `C:\Users\ahkff\Project-BFX`다.
- 원격 주소는 `https://github.com/redforce01/Project-BFX.git`이다.
- Campaign EventLog는 Campaign 주행 중 발생한 이벤트를 HUD에 누적 표시하는 시스템이다.
- `CampaignEventTrigger`가 Player 트리거 진입 시 총 발동 확률과 이벤트별 가중치를 기준으로 이벤트를 뽑는다.
- 선택된 이벤트는 `BfxCampaignEventLogService.Instance.AddLog(...)`로 기록된다.
- `BfxCampaignEventLogService`는 최대 50개 로그를 보관하고 `OnLogAdded` 이벤트로 View에 알린다.
- `BfxCampaignEventLogView`는 GPM `InfiniteScroll`을 사용해 로그를 표시하고 새 로그가 들어오면 하단으로 이동한다.
- 로그 한 줄은 `BfxCampaignEventLogEntry.DisplayText`에서 `[Day N  HH:MM] [이벤트 라벨] 위치 설명` 형식으로 만들어진다.
- 구현 근거 커밋: `81406770 [Campaign] HUD Event Log 시스템 추가`.
- 최근 BFX 작업 트리에는 ProjectSettings/Thumbs.db 관련 미커밋 변경이 존재했다. EventLog 문서화와 직접 관련 없는 변경으로 취급한다.

## 중요한 파일

- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/CampaignEventTrigger.cs`: 캠페인 이벤트 발동, 가중치 선택, EventLog 기록 진입점.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/BfxCampaignEventLogService.cs`: 로그 보관, 최대 개수 제한, View 알림.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/BfxCampaignEventLogEntry.cs`: 로그 데이터와 표시 문자열.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/BfxCampaignEventLogView.cs`: GPM InfiniteScroll 기반 HUD 표시.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/InfiniteScrollDataEventLog.cs`: InfiniteScroll 데이터 어댑터.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/InfiniteScrollItemEventLog.cs`: InfiniteScroll 아이템을 TMP View에 바인딩.
- `Project-BFX/Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/BfxCampaignEventLog.cs`: TMP 텍스트를 가진 단일 로그 row view.
- `Project-BFX/Assets/01_PROJECT BFX/Art/UI/Campaign/Campaign.EventLog.prefab`: EventLog UI 프리팹.
- `Project-BFX/Assets/01_PROJECT BFX/Art/UI/UI Prefabs/UI.CampaignHUD.prefab`: Campaign HUD에 EventLog를 연결한 프리팹.

## 종결 상태

- 2026-06-09에 Unity Play 흐름에서 EventLog 정상작동을 확인했다.
- Campaign HUD/EventLog 프리팹 연결은 `UI.CampaignHUD.prefab`의 `BfxCampaignEventLogView` 아래에 GPM `InfiniteScroll`이 있고, row prefab인 `Campaign.EventLog.prefab`에 `InfiniteScrollItemEventLog`, `BfxCampaignEventLog`, TMP 텍스트 참조가 이어지는지만 확인하면 된다.
- `BfxCampaignEventLogService`는 scene 전환 후 보존하지 않는다. `DontDestroyOnLoad`는 추가하지 않으며, scene이 넘어가면 로그도 함께 사라지는 정책으로 종결한다.
- 로그 문구/이벤트 라벨의 현지화나 데이터화는 이번 EventLog 작업 범위에서 진행하지 않는다. 현재 코드 기반 표시 형식을 유지한다.
- `_maxEntryCount` 50과 저장/로드 미지원 정책도 이번 범위에서는 유지한다.

## 후속 작업

현재 EventLog 토픽은 완료 상태다. 이후 다시 열 경우에는 Campaign 전체 HUD UX 개편, localization/data table 도입, Campaign 기록 저장 같은 별도 토픽으로 분리하는 편이 좋다.
