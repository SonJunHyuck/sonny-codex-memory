# Project-BFX HUD 미니맵 타임라인

## 2026-06-09

- 메모리 저장소에 `contexts/bfx/hud-minimap` 컨텍스트를 생성했다.
- `current.md`, `decisions.md`, `timeline.md` 초기 파일을 추가했다.
- 미니맵 대상이 전투 `PlayerHUD`가 아니라 `CampaignHUD`임을 확인했다.
- 실제 캠페인 노드 시스템이 `BfxCampaignMapDefinition` / `BfxCampaignMapController`가 아니라 `ExpeditionSystem`임을 확인했다.
- 정적 캡처 지도 배경 + 동적 route/region/current marker 방식을 선택했다.
- 플레이어 중심 A방식을 선택했다: 고정 viewport/current marker, 이동하는 content.
- 에디터 캡처 툴 `CampaignMinimapCaptureWindow.cs`를 추가했다.
- 사용자가 `Assets/01_PROJECT BFX/Art/UI/Campaign/Minimap/CampaignMinimap.prefab`로 미니맵 프리팹을 분리했다.
- 런타임 제어용 `CampaignMinimapView.cs`를 추가했다.
- `CampaignHUD.cs`가 `CampaignMinimap` 자식을 찾아 `CampaignMinimapView`를 보장하고 초기화하도록 수정했다.
- Unity 스크립트 컴파일 결과 에러 0개를 확인했다.
- Play Mode에 진입했고 `CampaignMinimap` 관련 콘솔 에러가 없음을 확인했다.
- 기존 별도 이슈로 보이는 missing script 참조 에러가 반복되는 것을 기록했다.

## 2026-06-11

- `CampaignMinimapView` 개선 방향을 논의했다.
- 현재 A방식 구현은 유지하되, 다음 작업은 구조 재작성보다 좌표계/프리팹/데이터 소유권 정리에 집중하기로 정리했다.
- 특히 `clampContentToViewport`와 중앙 고정 `Current Marker`가 지도 끝에서 의미 충돌을 만들 수 있음을 기록했다.
- `CampaignMinimapView`는 가능하면 `CampaignMinimap.prefab`에 직접 저장하고, `CampaignHUD`의 runtime auto-add는 fallback으로 남기는 방향을 제안했다.
- 캡처 bounds는 코드 기본값이나 JSON 직접 로드보다 ScriptableObject 데이터 에셋으로 분리하는 방안을 우선 검토하기로 했다.
- `Minimap Content`, `Route Lines`, `Region Markers`, `Current Marker`는 프리팹에 명시하고 런타임 생성은 복구 안전망으로 낮추는 방향을 기록했다.

## 2026-06-12

- `clampContentToViewport`는 false 기준으로 진행했다. 현재 위치 마커 중앙 고정을 우선하며, 지도 끝에서는 빈 공간이 보일 수 있다.
- `CampaignMinimapView`를 `CampaignMinimap.prefab`에 직접 붙인 구조를 기준으로 정리했다.
- `CampaignMinimap.prefab`에 `Minimap Content`, `Region Markers`, `Current Marker`를 명시했다.
- 캡처 bounds와 배경 texture 참조를 담는 `CampaignMinimapCaptureData.cs`를 추가했다.
- `CampaignMinimap.asset`을 추가해 `CampaignMinimap.png`와 캡처 bounds를 런타임 기준 데이터로 묶었다.
- `CampaignMinimapCaptureWindow`가 PNG 캡처 후 `CampaignMinimap.asset`도 생성/갱신하도록 바꿨다.
- 사용자가 Line 마커가 필요 없다고 결정해서 `Route Lines` 프리팹 계층과 라인 생성/갱신 코드를 제거했다.
- `RouteMap`은 라인 렌더링용이 아니라, 이동 가능 Region 판단과 마커 클릭 처리용으로 계속 사용한다.
- `CampaignMinimapView`, `CampaignMinimapCaptureData`, `CampaignMinimapCaptureWindow`에 한국어 주석을 추가했다.
- 변수 주석은 짧은 명사형 인라인 주석으로 정리했고, 메서드에는 역할 설명과 핵심 흐름 주석을 남겼다.
- Unity MCP 서버는 HTTP endpoint로는 정상 동작한다. `initialize`, `tools/list`, `read_console` 호출이 성공했고 콘솔 error 0개를 확인했다.
- Codex native tool discovery에는 Unity MCP namespace가 자동 노출되지 않아, 필요하면 raw HTTP 방식으로 MCP를 호출해야 한다.
- 기존 missing script 에러는 이번 작업 범위에서 처리하지 않기로 했다.
