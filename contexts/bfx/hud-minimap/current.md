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

- 선택한 방식: 캡처한 캠페인 지도 PNG 배경 + 동적으로 생성하는 UI 지역 마커 + 중앙 고정 현재 위치 마커.
- 노선 라인 마커는 사용하지 않기로 했고, `Route Lines`, `RebuildRoutes()`, `RouteLineEntry`, 라인 색/두께 필드는 제거했다.
- 미니맵 동작: 플레이어 또는 `RegionNavigator`는 중앙에 고정하고, 큰 지도 Content가 그 아래에서 반대로 이동한다.
- `clampContentToViewport`는 현재 false가 기준이다. 따라서 지도 가장자리 근처에서는 빈 공간이 보일 수 있지만, 현재 위치 마커는 중앙 유지가 우선이다.
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.prefab`는 분리된 미니맵 프리팹이며, 현재 `UI.CampaignHUD.prefab` 안에 들어가 있다.
- `CampaignMinimap.prefab`에는 `CampaignMinimapView`가 직접 붙어 있고, `CampaignHUD`의 auto-add는 fallback 성격으로 남아 있다.
- 프리팹 계층은 고정 viewport(root), `Minimap Content`, 배경 `Raw Image`, `Region Markers`, viewport 직속 `Current Marker` 구조다.
- `CampaignMinimap.png`, `CampaignMinimap.json`, `CampaignMinimap.asset`은 `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap` 아래에 있다.
- `CampaignMinimap.asset`은 `CampaignMinimapCaptureData` ScriptableObject이며, 런타임 미니맵이 배경 texture와 월드 bounds를 읽는 기준 데이터다.
- `CampaignMinimap.json`은 사람이 확인하기 위한 보조 메타데이터로 남아 있다.
- 현재 캡처 bounds:
  - `minX`: -2363.5
  - `maxX`: 2827.5
  - `minZ`: -2500.0
  - `maxZ`: 2691.0
  - `orthographicSize`: 2595.5
- `CampaignMinimapView`는 A방식으로 동작한다: 고정된 viewport/current marker, 이동하는 content.
- `CampaignMinimapView.Clicked`는 추후 전체 월드맵 UI를 여는 훅으로 남겨둔다.

## 중요 파일

- `Assets/01_PROJECT BFX/Scripts/Game/UI/Panel/CampaignHUD.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/CampaignMinimapView.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/HUD/CampaignMinimapCaptureData.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/Editor/CampaignMinimapCaptureWindow.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/Region.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RouteMap.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RegionNavigator.cs`
- `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem/RegionWorldUiController.cs`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.prefab`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.png`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.json`
- `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.asset`

## 검증 메모

- Unity MCP 서버는 `http://127.0.0.1:8080/mcp`에서 정상 응답한다.
- Codex native tool discovery에는 Unity MCP namespace가 자동 노출되지 않았지만, HTTP MCP 프로토콜로 `initialize`, `tools/list`, `read_console` 호출은 성공했다.
- `read_console` 기준 Unity Console error 0개를 확인했다.
- `git diff --check`는 미니맵 관련 변경 파일 기준 통과했다.
- 기존 씬/프리팹에서 보이는 별도 에러가 있었다: `The referenced script (Unknown) on this Behaviour is missing!` 반복. 이번 미니맵 작업 범위에서는 처리하지 않는다.
- 로컬 `dotnet build Assembly-CSharp.csproj --no-restore`는 Unity가 생성한 csproj에 신규 `CampaignMinimapCaptureData.cs`가 아직 포함되지 않은 상태에서 실패했다. Unity refresh 후 확인하는 것이 적절하다.

## 현재 구현 형태

- `CampaignMinimapView`는 `RegionNavigator`와 `RouteMap`을 할당받지 못하면 씬에서 찾아온다.
- 루트 viewport에 `RectMask2D`를 보장하고, 내부에 `Minimap Content`를 보장한다.
- 배경 `RawImage`는 `Minimap Content` 아래 첫 자식으로 유지하며 stretch 처리한다.
- `Region Markers` 레이어를 보장하고, 씬의 `Region` 오브젝트를 읽어 지역 마커를 만든다.
- 현재 위치, 이동 가능 노드, 잠긴 노드를 색상으로 구분한다.
- 이동 가능한 미니맵 마커를 누르면 `RegionNavigator.MoveTo(region)`을 호출한다.
- `RouteMap`은 노선 라인을 그리기 위해서가 아니라, 클릭 가능한 연결 Region 판단을 위해 계속 사용한다.
- 현재 위치 마커는 viewport 중앙에 고정되고, `RegionNavigator.transform.position` 기준으로 지도 content가 반대로 움직인다.
- `CampaignMinimapCaptureWindow`는 PNG 캡처 후 `CampaignMinimap.asset`을 생성/갱신할 수 있다.
- `CampaignMinimapView`, `CampaignMinimapCaptureData`, `CampaignMinimapCaptureWindow`에는 한국어 주석을 추가했다. 변수 주석은 짧은 명사형 인라인 주석으로 정리했다.

## 다음 작업

- 1순위: `FindObjectsOfType<Region>()`, `FindObjectOfType<RegionNavigator>()`, `FindObjectOfType<RouteMap>()` 의존을 줄인다.
  - `CampaignHUD` 또는 Expedition 초기화 흐름에서 명시적으로 주입하는 방향을 우선 검토한다.
  - Region 목록은 장기적으로 `RouteMap.connections` 또는 별도 provider를 통해 미니맵이 사용할 목록을 공급받는 방향이 좋다.
- 2순위: Campaign 씬에서 실제 런타임 위치 정합성을 계속 확인한다.
  - 배경 PNG 좌표계와 Region 월드 좌표가 정확히 맞는지 확인한다.
  - 가장자리 근처에서 `clampContentToViewport = false` 상태의 빈 공간 노출이 의도와 맞는지 확인한다.
- 3순위: 전체 월드맵 UI가 생기면 미니맵 클릭 훅을 `CampaignWorldMapView` 열기로 연결한다.
- 4순위: 최종 아트 방향이 정해지면 현재 생성형 사각 마커를 실제 UI 스프라이트로 교체한다.
- Campaign 씬/프리팹의 기존 missing script 에러는 별도 작업으로 정리한다.
