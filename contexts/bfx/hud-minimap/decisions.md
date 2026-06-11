# Project-BFX HUD 미니맵 결정 기록

이 파일은 `bfx.hud-minimap` 컨텍스트의 주요 결정을 기록한다.

## 2026-06-09: HUD 미니맵 컨텍스트 생성

결정:
구현을 시작하기 전에 `contexts/bfx/hud-minimap` 아래에 전용 메모리 컨텍스트를 만든다.

이유:
HUD 미니맵은 UI 레이아웃, 렌더링 방식, 플레이어/마커 추적, 에셋 세팅까지 여러 영역을 건드릴 가능성이 있어서 별도 기록 공간이 필요하다.

기각한 대안:
- 기존 eventlog 컨텍스트를 재사용하는 방식. 서로 다른 기능 히스토리가 섞이기 때문에 제외했다.

영향:
이후 미니맵 작업의 구현 메모와 결정 사항을 한곳에 이어서 기록할 수 있다.

## 2026-06-09: 대상은 CampaignHUD와 ExpeditionSystem

결정:
미니맵은 `CampaignHUD`에 구현하고, 실제 데이터/이동 기준은 `Assets/01_PROJECT BFX/Scripts/Game/Campaign/ExpeditionSystem` 아래 시스템을 사용한다.

이유:
전투용 `PlayerHUD`가 아니라 캠페인 화면의 HUD가 대상이다. 또한 사용자가 `BfxCampaignMapDefinition` / `BfxCampaignMapController`가 실제 노드 시스템이 아니라고 명확히 알려줬다. 실제 기준은 씬에 배치된 `Region`, `RouteMap.connections`, `RegionNavigator.currentRegion` 및 `RegionNavigator.transform.position`이다.

기각한 대안:
- `PlayerHUD` 기준 구현. 전투 HUD라 현재 요구와 맞지 않는다.
- `BfxCampaignMapDefinition` / `BfxCampaignMapController` 기준 구현. 실제 사용 시스템이 아니므로 제외한다.

영향:
미니맵은 전투 레이더가 아니라 캠페인 이동/거점 네비게이션 뷰로 구현한다.

## 2026-06-09: 정적 지도 배경 + 동적 마커 방식 선택

결정:
캡처한 PNG 지도 이미지를 배경으로 사용하고, route line, region marker, current marker는 런타임 UI로 동적 생성한다.

이유:
성능 부담이 작고, UI 계층에서 상태 표현을 안정적으로 제어할 수 있다. RouteMap/Region 상태가 바뀌어도 마커와 라인만 갱신하면 된다.

기각한 대안:
- RenderTexture 탑다운 카메라 방식. 렌더 비용, 레이어, 천장/가림, 카메라 세팅 이슈가 커질 수 있다.
- 배경 없는 마커 전용 레이더 방식. 사용자가 원하는 캠페인 지도 경험과 맞지 않는다.

영향:
지도 캡처 툴과 월드 좌표를 지도 좌표로 바꾸는 bounds 데이터가 필요하다.

## 2026-06-09: 플레이어 중심 A방식 미니맵 추적

결정:
미니맵 viewport와 current marker는 고정하고, 큰 `Minimap Content`를 반대로 이동시켜 `RegionNavigator`가 항상 중앙에 보이게 한다.

이유:
사용자는 작은 미니맵에서 전체 지도를 항상 보는 것이 아니라 현재 위치를 중심으로 움직이는 미니맵을 원했다. 배경, 노드, 라인을 한 content 아래에 묶고 content만 움직이면 정렬이 깨지지 않는다.

기각한 대안:
- 배경/마커/라인을 각각 따로 이동시키는 방식. 관리가 복잡하고 어긋날 위험이 크다.
- 작은 HUD 미니맵에서 전체 지도를 항상 보여주는 방식. 현재 위치 중심 요구와 맞지 않는다.

영향:
UI 계층은 mask가 있는 고정 viewport, 큰 content, 배경 이미지, route line layer, region marker layer, 중앙 고정 current marker 구조가 된다. 추후 전체 월드맵도 같은 좌표 변환과 에셋을 재사용할 수 있다.

## 2026-06-09: 미니맵 클릭은 전체 월드맵 훅으로 예약

결정:
현재는 미니맵 클릭 이벤트만 노출하고, 실제 전체 월드맵 열기는 나중에 `CampaignWorldMapView`를 만들 때 연결한다.

이유:
사용자는 미니맵 클릭 시 월드맵이 보이는 시스템까지 고려하고 있다. 다만 이번 구현 범위는 HUD 미니맵 동작이므로 월드맵 UI 자체는 후속 작업으로 둔다.

기각한 대안:
- 지금 바로 전체 월드맵까지 구현하는 방식.
- 클릭 처리를 아예 빼고 나중에 다시 붙이는 방식.

영향:
`CampaignMinimapView.Clicked` 이벤트가 있고, `CampaignHUD`는 현재 예약 로그를 찍는다. 추후 이 부분을 월드맵 UI 열기로 교체하면 된다.

## 2026-06-11: 다음 개선 방향은 좌표계와 프리팹 소유권 정리

결정 후보:
현재 `CampaignMinimapView` 구현 방향은 유지하되, 다음 작업은 좌표계 의미와 프리팹 소유권을 먼저 정리한다.

이유:
현재 구현은 프로토타입으로는 닫혀 있지만, `clampContentToViewport`가 켜진 상태에서 `Current Marker`가 무조건 중앙에 고정되면 지도 끝 근처에서 실제 navigator 위치와 중앙 마커가 어긋날 수 있다. 또한 `CampaignHUD`가 런타임에 `CampaignMinimapView`를 auto-add하는 구조는 안전망으로는 좋지만, Inspector에서 설정과 참조를 관리하기 어렵다.

우선순위:
- `clampContentToViewport`와 중앙 고정 marker 정책을 먼저 결정한다.
- `CampaignMinimapView`를 `CampaignMinimap.prefab`에 직접 붙이고, `CampaignHUD` auto-add는 fallback으로 남긴다.
- 캡처 bounds는 `CampaignMinimap.json` 직접 런타임 로드보다 ScriptableObject 데이터 에셋으로 굳히는 방안을 우선 검토한다.
- UI 계층(`Minimap Content`, `Route Lines`, `Region Markers`, `Current Marker`)은 프리팹에 명시하고, 런타임 생성은 누락 복구 용도로 낮춘다.
- region 목록은 장기적으로 `FindObjectsOfType<Region>()`보다 `RouteMap.connections` 또는 별도 provider에서 명시적으로 공급받는 방향을 검토한다.

영향:
다음 구현은 기능 추가보다 안정화 성격이 강하다. 좌표 변환, 프리팹 세팅, 런타임 fallback 경계를 먼저 정리하면 추후 전체 월드맵 UI와 실제 마커 아트 교체가 더 안전해진다.
