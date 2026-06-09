# Notion 재료: BFX EventLog

이 파일은 나중에 Notion 문서로 옮길 수 있는 구조화된 재료를 모은다.

## 후보 Notion page

- BFX Campaign HUD EventLog
- BFX Campaign Event Trigger와 로그 표시 흐름
- 디바이스 간 Codex 인수인계 프로세스

## 시스템 의도

BFX Campaign HUD EventLog는 캠페인 이동 중 발생한 이벤트를 플레이어가 놓치지 않도록 HUD에 누적 표시하는 시스템이다.

Campaign 주행 경로에는 보이지 않는 `CampaignEventTrigger`들이 배치된다. Player가 트리거에 진입하면 발동 확률과 이벤트별 가중치로 최종 이벤트가 선택되고, 그 결과가 `BfxCampaignEventLogService`에 기록된다. HUD는 서비스가 발행하는 로그 추가 이벤트를 받아 GPM InfiniteScroll에 새 로그 row를 붙인다.

## 왜 필요한가

Campaign에는 전투, 환경 위협, 자원 발견, NPC 조우처럼 플레이 진행에 영향을 주는 사건이 발생한다. 이 사건들이 `Debug.Log`나 순간적인 연출로만 남으면 플레이어와 개발자가 흐름을 복기하기 어렵다.

HUD EventLog는 사건 발생 시점, 이벤트 종류, 위치, 설명을 한 줄로 남겨 Campaign 진행 기록 역할을 한다.

## 설계 원칙

- 이벤트 발생 로직과 HUD 표시 로직을 분리한다.
- 로그 목록은 서비스가 소유하고, View는 구독/표시만 담당한다.
- UI 표시에는 기존 GPM InfiniteScroll을 사용한다.
- 로그 entry는 값 타입으로 유지하고 표시 문자열 생성 책임을 entry에 둔다.
- Campaign 시간 정보는 `BfxGameTimeService`에서 가져와 로그에 포함한다.

## 구현 구성

- `CampaignEventTrigger`: Player 충돌 시 이벤트 발생 여부를 굴리고, 이벤트 풀의 가중치로 최종 이벤트를 선택한다.
- `BfxCampaignEventLogService`: 로그 entry를 최대 50개까지 보관하고 `OnLogAdded` 이벤트를 발행한다.
- `BfxCampaignEventLogEntry`: 이벤트 타입, 설명, 위치, 날짜/시간 정보를 가진다.
- `BfxCampaignEventLogView`: 서비스의 로그 목록을 GPM InfiniteScroll에 표시하고 새 항목이 들어오면 하단으로 이동한다.
- `InfiniteScrollDataEventLog`, `InfiniteScrollItemEventLog`, `BfxCampaignEventLog`: InfiniteScroll 데이터와 TMP row view 연결 계층이다.

## 미래 Notion 구조

Notion page에는 다음 section을 둘 수 있다.

1. Campaign HUD EventLog가 무엇인지
2. 플레이어 경험에서 왜 필요한지
3. 런타임 흐름
4. 주요 클래스 책임
5. 로그 표시 형식
6. 프리팹/HUD 연결
7. 알려진 제한
8. 다음 개선

## 열린 질문

이 토픽의 열린 질문은 2026-06-09에 현재 범위 기준으로 닫았다.

- `BfxCampaignEventLogService`는 scene 전환 후 유지하지 않는다.
- 로그 저장/로드는 이번 EventLog 범위에서 만들지 않는다.
- 이벤트 설명과 라벨은 현재 코드 기반 구현을 유지한다.
- `_maxEntryCount` 50을 유지한다.

나중에 Campaign 영구 기록, localization/data table, HUD UX 개편이 필요해지면 별도 토픽으로 다시 연다.
