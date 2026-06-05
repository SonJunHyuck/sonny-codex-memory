# Notion Seed: Codex 메모리 운영 흐름

이 파일은 Sonny의 Codex 메모리 운영 흐름을 나중에 Notion page로 정리하기 위한 구조화된 재료를 모은다.

## 후보 Notion Page

Codex 메모리 운영 흐름

## 독자

Sonny, 미래의 Codex 세션, 그리고 프로젝트 컨텍스트가 디바이스를 넘어 어떻게 이어지는지 이해해야 하는 협업자.

## 핵심 설명

Codex 메모리 운영 흐름은 Git 기반 인수인계 시스템이다. raw local chat history에 의존하지 않고, 오래 남길 작은 Markdown 파일 묶음을 읽어서 Codex가 여러 디바이스와 프로젝트 채팅을 오가며 작업을 이어갈 수 있게 한다.

## 왜 필요한가

Sonny는 PC와 Mac을 오가며 작업하고, 여러 프로젝트 또는 한 프로젝트 안의 여러 토픽을 다룰 수 있다. Codex project chat은 실제 작업에는 유용하지만, 디바이스와 프로젝트를 넘는 메모리 계층으로는 혼자 충분하지 않다.

## 핵심 개념

- 메모리 저장소: `sonny-codex-memory`
- Context id: `bfx.eventlog` 같은 안정적인 project/topic key
- Repo id: `project-bfx` 같은 안정적인 저장소 identity
- 로컬 경로 힌트: 디바이스별 checkout 경로
- 현재 컨텍스트: 최신 인수인계 상태
- 결정 기록: 오래 남길 결정 이유
- 타임라인: milestone history
- 세션 기록: 가벼운 원천 기록

## 시스템 형태

```text
_index/
  active.md
  repositories.md
  local-paths.md

contexts/
  <project>/
    <topic>/
      current.md
      decisions.md
      timeline.md
      notion.md

sessions/
  YYYY/
    MM/
      YYYY-MM-DD-<context-id>.md

templates/
```

## 문서화 관점

최종 Notion page는 다음을 설명하면 좋다.

1. 이어가기 문제
2. 왜 Git을 선택했는지
3. 메모리 저장소와 구현 저장소가 어떻게 다른지
4. 세션을 어떻게 시작하는지
5. 세션을 어떻게 끝내는지
6. PC/Mac 로컬 경로를 어떻게 다루는지
7. 컨텍스트 파일이 어떻게 미래 문서가 되는지

## 열린 질문

- Notion page를 컨텍스트별로 만들지, 프로젝트별로 만들지, 둘 다 만들지?
- 세션 기록을 자동으로 `notion.md`에 roll-up할지?
- 새 컨텍스트를 만들기 위한 script나 checklist가 필요할지?
