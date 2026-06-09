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
