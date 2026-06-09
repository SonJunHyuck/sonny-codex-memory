---
context_id: <project.topic>
project: <project>
topic: <topic>
status: active
memory_repo:
  repo_id: sonny-codex-memory
  remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: <repo-id>
    name: <repo-name>
    remote: <remote-url>
---

# <Project Topic> 현재 컨텍스트

## 목적

이 컨텍스트가 왜 존재하는지, 어떤 작업/의사결정/문제를 이어가기 위한 기록인지 적는다.

## 작업 기준

Codex가 이 컨텍스트 작업을 시작할 때 알아야 할 기준을 적는다.

- 로컬 체크아웃 경로는 이 파일에 반복해서 쓰지 않는다.
- 구현 저장소와 메모리 저장소의 로컬 경로는 `_index/local-paths.md`를 참조한다.
- 저장소의 안정적인 식별 정보는 front matter의 `repo_id`, `name`, `remote`를 기준으로 한다.

## 현재 이해

- <현재 사실 또는 요약>

## 중요 파일

- <경로 또는 파일 메모>

## 다음 작업

- <다음 작업>
