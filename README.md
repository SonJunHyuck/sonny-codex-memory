# sonny-codex-memory

PC, Mac, 여러 프로젝트, 여러 세션을 오가며 Codex 작업을 이어가기 위한 개인 컨텍스트 메모리 저장소.

## 목적

이 저장소는 여러 디바이스에서 Codex와 이어서 작업하기 위한 인수인계 요약, 프로젝트 컨텍스트, 결정 기록, 다음 작업을 저장한다.

전체 채팅 로그를 보관하는 archive가 아니다. 다음 작업에 필요한 오래 남길 컨텍스트만 저장한다.

## 작업 흐름

세션을 시작할 때:

```bash
git pull
```

그다음 Codex에게 필요한 컨텍스트를 읽어달라고 요청한다.

세션을 끝낼 때는 Codex에게 관련 컨텍스트를 업데이트하고, 필요한 세션 요약을 만들고, commit/push까지 해달라고 요청한다.

## 구조

```text
_index/
  active.md              # 활성 컨텍스트와 빠른 링크
  repositories.md        # 안정적인 저장소 식별자와 원격 주소
  local-paths.md         # 디바이스별 체크아웃 경로 힌트

contexts/
  <project-or-area>/
    <topic>/
      current.md         # 최신 작업 컨텍스트
      decisions.md       # 오래 남길 결정 기록
      timeline.md        # 중요한 마일스톤
      notion.md          # 나중에 Notion 문서로 옮길 재료

sessions/
  YYYY/
    MM/
      YYYY-MM-DD-<context-id>-<device>.md

templates/
  current.md
  decisions.md
  timeline.md
  session.md
```

## 컨텍스트 모델

컨텍스트는 프로젝트와 토픽 단위로 정리한다.

```text
contexts/
  bfx/
    eventlog/
      current.md
      decisions.md
      timeline.md
      notion.md
```

일상적인 이어가기는 `current.md`, `decisions.md`, `timeline.md`를 기준으로 한다. 어떤 세션에서 결정, 유용한 이유, 미래 문서화 재료가 생겼다면 `sessions/`에 가벼운 원천 기록을 남긴다. `notion.md`는 무엇을 만들었고, 왜 필요했고, 어떻게 동작하는지를 나중에 Notion 문서로 옮기기 위한 준비 공간이다.

한 디바이스의 절대 경로를 기준 경로로 취급하지 않는다. PC와 Mac의 체크아웃 위치는 서로 다를 수 있다. `context_id`, `repo_id`, Git 원격 주소 같은 안정적인 식별자를 우선하고, 로컬 경로는 `_index/local-paths.md`에 힌트로만 둔다.

## 안전 규칙

- secret, API key, token, password, private credential을 저장하지 않는다.
- raw chat transcript는 기본적으로 저장하지 않는다.
- 다음 작업을 이어가는 데 필요한 컨텍스트만 요약한다.
- 독점적인 내용을 길게 복사하기보다 링크와 파일 경로를 우선한다.
- 프로젝트 민감 정보는 의도적으로, 짧게만 남긴다.
