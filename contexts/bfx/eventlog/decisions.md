# BFX EventLog 결정 기록

이 파일은 `bfx.eventlog` 컨텍스트에 대한 오래 남길 결정을 기록한다. 짧게 유지하되, 현재 구조가 왜 존재하는지 설명해야 한다.

## 2026-06-05: Memory Repo와 Project Repo 분리

결정: `sonny-codex-memory`는 장기 메모리 저장소로 사용하고, 실제 구현 작업은 `ProjectBFX`에서 진행한다.

이유:

- Codex 프로젝트 채팅은 개별 프로젝트에 scoped되지만, Sonny는 디바이스와 프로젝트를 넘어 컨텍스트를 이어가고 싶다.
- 메모리 저장소는 컨텍스트 인수인계의 시작점과 종료점 역할을 할 수 있다.
- 프로젝트 저장소는 코드, 테스트, 구현 산출물에 집중해야 한다.

버린 대안:

- 모든 Codex chat context를 구현 저장소 안에 저장한다. 이 방식은 개인 인수인계 note와 프로젝트 코드를 섞어버리고, 여러 project/topic으로 확장하기 어렵다.

영향:

- BFX 프로젝트 채팅은 이어가기가 필요할 때 이 메모리 저장소를 읽고 쓴다.
- 구현 commit은 실제 프로젝트 저장소에 남긴다.

## 2026-06-05: Memory를 Project와 Topic 단위로 정리

결정: 컨텍스트는 `contexts/<project>/<topic>/` 아래에 저장한다.

BFX EventLog의 경우:

```text
contexts/
  bfx/
    eventlog/
      current.md
      decisions.md
      timeline.md
      notion.md
```

이유:

- 하나의 프로젝트에도 여러 독립 topic이 있을 수 있다.
- 토픽마다 현재 상태, 오래 남길 결정, 시간순 타임라인이 필요하다.
- "A project topic 1 어디까지 했는지 pull 받고 이어가자" 같은 문장으로 쉽게 작업을 시작할 수 있다.

영향:

- Codex는 `bfx.eventlog`를 `contexts/bfx/eventlog/`로 해석할 수 있다.

## 2026-06-05: Session 기록은 가볍게 유지

결정: `sessions/`는 full chat log가 아니라 선택적인 가벼운 원천 기록으로 유지한다.

이유:

- 일상적인 이어가기는 `current.md`, `decisions.md`, `timeline.md`만으로 충분하다.
- 나중에 Notion 문서를 만들 때 특정 세션에서 무슨 일이 있었는지 알면 도움이 된다.
- 전체 transcript는 너무 시끄럽고 불필요하다.

영향:

- 세션 기록은 의도, 변경, 결정, Notion 재료, 다음 작업을 기록한다.
- raw chat log는 기본적으로 저장하지 않는다.

## 2026-06-05: Notion Seed 추가

결정: 토픽마다 `notion.md`를 추가해서 미래 Notion 문서화를 위한 준비 공간으로 사용한다.

이유:

- Sonny는 나중에 어떤 system이 어떤 의도로 어떻게 만들어졌는지 Notion에 기록할 계획이다.
- `current.md`는 현재 상태를 설명하지만, Notion page에는 독자를 향한 구조와 이유가 필요하다.
- `sessions/`는 원천 기록을 제공하고, `notion.md`는 그것을 문서화 재료로 정리한다.

영향:

- 토픽이 충분히 성숙하면 Codex는 `notion.md`, `decisions.md`, `timeline.md`를 이용해 Notion page를 만들거나 업데이트할 수 있다.

## 2026-06-09: Campaign HUD EventLog는 런타임 서비스와 View로 분리

결정: Campaign EventLog는 이벤트 발생 지점(`CampaignEventTrigger`), 로그 저장/발행 서비스(`BfxCampaignEventLogService`), HUD 표시 View(`BfxCampaignEventLogView`)를 분리해서 구현한다.

이유:

- 이벤트 발생 로직과 UI 표시 로직이 직접 묶이면 Campaign 트리거를 수정할 때 HUD 구현까지 같이 건드리게 된다.
- 서비스가 로그 목록과 `OnLogAdded` 이벤트를 가지면, HUD는 현재 로그를 다시 그리거나 새 로그만 추가할 수 있다.
- GPM InfiniteScroll 기반 UI를 유지하면서도 로그 데이터 모델은 별도로 관리할 수 있다.

버린 대안:

- `CampaignEventTrigger`가 HUD prefab이나 TMP text를 직접 찾아서 쓰는 방식.
- 로그를 단순 `Debug.Log`로만 남기고 플레이어 HUD에는 표시하지 않는 방식.

영향:

- `CampaignEventTrigger.TriggerFinalEvent(...)`는 최종 이벤트를 고른 뒤 `BfxCampaignEventLogService.Instance.AddLog(...)`만 호출한다.
- `BfxCampaignEventLogService`는 최대 50개 로그를 보관하고, 초과분은 오래된 항목부터 제거한다.
- `BfxCampaignEventLogView`는 `OnEnable`에서 서비스 이벤트를 구독하고 `OnDisable`에서 해제한다.

## 2026-06-09: EventLog 표시 형식은 현재 게임 시간과 이벤트 라벨을 포함

결정: 로그 표시 문자열은 `BfxCampaignEventLogEntry.DisplayText`에서 `[Day N  HH:MM] [라벨] 위치 설명` 형식으로 만든다.

이유:

- Campaign 로그는 단순 알림보다 "언제, 어디서, 어떤 종류의 이벤트가 발생했는지"를 빠르게 복기할 수 있어야 한다.
- `BfxGameTimeService`의 Day/Hour/Minute 값을 같이 넣으면 Campaign 진행 흐름과 로그가 연결된다.

버린 대안:

- 이벤트 설명만 표시하는 단순 문자열 로그.
- View에서 표시 형식을 조합하는 방식.

영향:

- 로그 entry는 `CampaignEventTrigger.EventType`, description, locationName, day/hour/minute를 가진 값 타입으로 유지된다.
- EventType 라벨은 현재 코드 switch로 관리되며, 나중에 현지화나 데이터 테이블로 빼야 할 수 있다.

## 2026-06-09: EventLog는 Campaign scene 수명주기를 따른다

결정: `BfxCampaignEventLogService`는 `DontDestroyOnLoad`로 보존하지 않고, Campaign scene이 사라지면 EventLog도 함께 사라지는 정책으로 종결한다.

이유:

- EventLog는 현재 Campaign HUD에서 진행 중 이벤트를 복기하기 위한 런타임 UI 기록이다.
- scene 전환 이후까지 보존할 명확한 UX 요구가 아직 없다.
- 현재 `OnDestroy()`에서 static `_instance`를 null로 정리하므로 scene unload 뒤 낡은 singleton 참조가 남는 위험도 낮다.

영향:

- `DontDestroyOnLoad`는 추가하지 않는다.
- 저장/로드 기능도 이번 EventLog 범위에서는 만들지 않는다.
- 나중에 Campaign 영구 기록이 필요해지면 EventLog가 아니라 별도 Campaign history/save topic으로 다룬다.

## 2026-06-09: 현지화/데이터화와 로그 보관 정책은 현재 구현 유지

결정: 이벤트 라벨/문구의 localization 또는 data table 분리는 이번 작업 범위에서 진행하지 않는다. `_maxEntryCount` 50과 저장/로드 미지원 상태도 유지한다.

이유:

- Play 흐름에서 EventLog 정상작동이 확인되었고, 현재 목표는 HUD 로그 기능 완성이다.
- 문구 데이터화, 보관 개수 조정, 저장/로드는 더 큰 Campaign UX/데이터 정책과 함께 결정하는 편이 낫다.

영향:

- `BfxCampaignEventLogEntry.DisplayText`와 현재 EventType label switch 구조를 유지한다.
- `_maxEntryCount` 기본값 50을 유지한다.
- EventLog 토픽은 완료 상태로 닫는다.
