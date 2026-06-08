# 로컬 경로 힌트

이 경로들은 디바이스별 편의 정보다. PC와 Mac을 오갈 때 기준 정보로 취급하지 않는다.

## 현재 Mac

| Repo ID | 로컬 경로 |
| --- | --- |
| `sonny-codex-memory` | `/Users/sonny/Workspace/Dev/sonny-codex-memory` |
| `project-bfx` | `/Users/sonny/Workspace/Dev/ProjectBFX` |

## Windows PC

| Repo ID | 로컬 경로 |
| --- | --- |
| `sonny-codex-memory` | `C:\Users\ahkff\sonny-codex-memory` |
| `project-bfx` | `C:\Users\ahkff\ProjectBFX` |

## 사용법

어떤 디바이스에서든 작업을 시작할 때:

1. 현재 디바이스에 checkout된 위치에서 메모리 저장소를 pull한다.
2. 안정적인 저장소 식별자는 `_index/repositories.md`에서 확인한다.
3. 이 파일은 현재 디바이스의 로컬 체크아웃 경로를 찾을 때만 사용한다.
4. 적힌 경로가 오래됐거나 없으면 원격 주소로 저장소를 찾거나 Sonny에게 경로를 물어본다.
