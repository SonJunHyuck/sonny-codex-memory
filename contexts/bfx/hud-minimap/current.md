---
context_id: bfx.hud-minimap
project: Project-BFX
topic: Campaign HUD Minimap
status: active
memory_repo:
  repo_id: sonny-codex-memory
  remote: https://github.com/SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: project-bfx
    name: Project-BFX
    remote: https://github.com/redforce01/Project-BFX.git
---

# Project-BFX 캠페인 HUD 미니맵 컨텍스트

## 목적

`CampaignHUD`에 플레이어 현재 위치 중심으로 따라오는 캠페인 미니맵을 추가하기 위한 구현 맥락을 기록한다.

## 작업 기준

- 현재 대상은 전투 HUD인 `PlayerHUD`가 아니라 캠페인 화면의 `CampaignHUD` 미니맵이다.
- 실제 캠페인 노드/이동 시스템은 `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem` 아래 코드를 기준으로 한다.
- `BfxCampaignMapDefinition` / `BfxCampaignMapController`는 이번 기능의 실제 기준 시스템이 아니므로 새 구현의 중심으로 삼지 않는다.

## 현재 이해

- 선택한 방식: 캡처한 캠페인 지도 PNG 배경 + 동적으로 생성하는 UI 마커/노선 라인.
- 미니맵 동작: 플레이어 또는 `RegionNavigator`는 중앙에 고정하고, 큰 지도 Content가 그 아래에서 반대로 이동한다.
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.prefab`는 분리된 미니맵 프리팹이며, 현재 `UI.CampaignHUD.prefab` 안에 들어가 있다.
- `CampaignMinimap.png`와 `CampaignMinimap.json`은 `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap` 아래에 있다.
- 현재 JSON에는 월드 좌표를 미니맵 좌표로 변환할 때 쓰는 캡처 bounds가 들어 있다.
  - `minX`: -2363.5
  - `maxX`: 2827.5
  - `minZ`: -2500.0
  - `maxZ`: 2691.0
  - `orthographicSize`: 2595.5
- `CampaignMinimapView`는 A방식으로 동작한다: 고정된 viewport/current marker, 이동하는 content.
- `CampaignHUD`는 `CampaignMinimap` 자식 오브젝트를 찾아 `CampaignMinimapView`를 보장하고 초기화한다.
- 미니맵 클릭은 추후 전체 월드맵 UI를 여는 훅으로 남겨두었고, 현재는 예약 로그만 찍는다.

## 중요 파일

- `Assets/01_PROJECT BFX/Scripts/Game/UI/Panel/CampaignHUD.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/CampaignMinimapView.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/Editor/CampaignMinimapCaptureWindow.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/Region.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RouteMap.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RegionNavigator.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RegionWorldUiController.cs`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.prefab`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.png`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.json`

## 검증 메모

- Unity script refresh/compile 결과 컴파일 에러 0개.
- Play Mode 진입 확인 완료.
- `CampaignMinimap`로 필터링한 콘솔 에러는 없었다.
- 기존 씬/프리팹에서 보이는 별도 에러가 있었다: `The referenced script (Unknown) on this Behaviour is missing!` 반복.
- 로컬 `dotnet build`는 `dotnet`이 PATH에 없어 실행하지 못했다.

## 현재 구현 형태

- `CampaignMinimapView`는 `RegionNavigator`와 `RouteMap`을 할당받지 못하면 씬에서 찾아온다.
- 루트 viewport에 `RectMask2D`를 보장하고, 내부에 `Minimap Content`를 만든다.
- 기존 `RawImage` 지도 배경을 `Minimap Content` 아래로 옮기고 stretch 처리한다.
- route line layer와 region marker layer를 런타임에 만든다.
- `RouteMap.connections`를 읽어 노선 라인을 그린다.
- 씬의 `Region` 오브젝트를 읽어 노드 마커를 그린다.
- 현재 위치, 이동 가능 노드, 잠긴 노드를 색상으로 구분한다.
- 이동 가능한 미니맵 마커를 누르면 `RegionNavigator.MoveTo(region)`을 호출한다.
- 현재 위치 마커는 viewport 중앙에 고정되고, `RegionNavigator.transform.position` 기준으로 지도 content가 반대로 움직인다.

## 다음 작업

- 전체 월드맵 UI가 생기면 미니맵 클릭 훅을 `CampaignWorldMapView` 열기로 연결한다.
- `CampaignMinimapView`가 `CampaignMinimap.json`을 직접 읽어 bounds를 자동 적용할지 결정한다.
- Campaign 씬/프리팹의 기존 missing script 에러를 별도로 정리한다.
- 가능하면 `CampaignMinimapView`를 `CampaignMinimap.prefab`에 직접 저장해서 런타임 auto-add 의존을 줄인다.
- 최종 아트 방향이 정해지면 현재 생성형 사각 마커/라인을 실제 UI 스프라이트로 교체한다.

