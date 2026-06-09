# 저장소 지도

이 파일은 안정적인 저장소 식별자를 기록한다. 로컬 체크아웃 경로는 디바이스별 힌트일 뿐 기준 정보가 아니다.

## 규칙

- `context_id`, `repo_id`, `remote`를 안정적인 식별자로 사용한다.
- `local_paths`는 현재 디바이스에서 쓰는 선택적 힌트로 취급한다.
- 어떤 디바이스에서 경로가 존재하지 않으면 원격 주소로 저장소를 찾거나 Sonny에게 그 디바이스의 체크아웃 경로를 물어본다.

## 저장소 목록

| Repo ID | 역할 | Remote | Local 경로 힌트 |
| --- | --- | --- | --- |
| `sonny-codex-memory` | Codex 메모리와 인수인계 저장소 | `git@github.com:SonJunHyuck/sonny-codex-memory.git` | `_index/local-paths.md` 참고 |
| `project-bfx` | BFX 구현 저장소 | `https://github.com/redforce01/Project-BFX.git` | `_index/local-paths.md` 참고 |
