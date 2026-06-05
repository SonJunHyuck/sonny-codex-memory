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
