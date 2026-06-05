# Notion 재료: BFX EventLog

이 파일은 나중에 Notion 문서로 옮길 수 있는 구조화된 재료를 모은다.

## 후보 Notion page

- Codex 메모리 운영 흐름
- BFX EventLog 개발 컨텍스트
- 디바이스 간 Codex 인수인계 프로세스

## 시스템 의도

메모리 저장소는 Codex가 하나의 local chat history에 의존하지 않고, 여러 디바이스와 프로젝트 채팅을 오가며 작업을 이어갈 수 있게 하기 위해 존재한다.

이 system은 두 역할을 분리한다.

- `sonny-codex-memory`: 장기 컨텍스트, 인수인계 요약, 결정, 문서화 재료.
- `ProjectBFX`: 실제 구현 코드, 테스트, 프로젝트 산출물.

저장소 identity는 디바이스를 넘어 안정적이어야 하고, 로컬 체크아웃 경로는 디바이스별 정보로 다룬다. Notion 문서에서는 한 기기의 절대 filesystem path보다 repo id, 원격 주소, context id를 우선적으로 언급해야 한다.

## 왜 필요한가

Sonny는 여러 디바이스에서 작업하고, 여러 프로젝트 또는 한 프로젝트 안의 여러 토픽을 논의할 수 있다. 하나의 chat thread나 하나의 project workspace만으로는 모든 곳에서 필요한 컨텍스트가 자연스럽게 보존되지 않는다.

메모리 저장소는 Codex에게 세션 시작 시 pull하고 종료 시 push할 수 있는, Git 기반의 오래 남는 기준 자료를 제공한다.

## 설계 원칙

- raw chat transcript는 기본적으로 저장소에 저장하지 않는다.
- 오래 남길 컨텍스트만 저장한다.
- 컨텍스트는 프로젝트와 토픽 단위로 정리한다.
- 일일 세션 기록은 가볍게 유지한다.
- 나중 문서화를 위한 기준 자료로 decisions와 timeline 파일을 사용한다.
- `notion.md`는 작업 기억과 독자를 위한 Notion page 사이의 다리로 사용한다.
- 로컬 체크아웃 경로는 별도의 디바이스별 index에 둔다.

## 미래 Notion 구조

Notion page에는 다음 section을 둘 수 있다.

1. 이 운영 흐름이 무엇인지
2. 왜 만들었는지
3. 저장소별 역할
4. 세션 시작 흐름
5. 세션 종료 흐름
6. 컨텍스트 파일 구조
7. 결정 기록
8. 알려진 제한
9. 다음 개선

## 열린 질문

- `ProjectBFX`의 원격 주소는 무엇인가?
- 메모리 저장소와 ProjectBFX의 Windows PC 체크아웃 경로는 무엇인가?
- 성숙한 컨텍스트마다 Notion page를 만들지, 여러 컨텍스트를 project-level page로 roll-up할지?
- Notion publish는 수동, 반자동, Codex 요청 기반 중 어떤 방식으로 할지?
