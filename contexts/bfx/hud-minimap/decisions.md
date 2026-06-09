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
