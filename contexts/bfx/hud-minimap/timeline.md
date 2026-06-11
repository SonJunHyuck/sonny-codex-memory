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
