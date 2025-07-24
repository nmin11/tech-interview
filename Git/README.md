## reset과 revert의 차이

공통점

- 과거의 커밋을 건드리는 작업

차이점

- reset
  - 과거 커밋으로 돌아가고 이후 커밋 기록 삭제
  - 세부 옵션들
    - `--soft` : 과거 커밋 이후 모든 파일 보존
    - `--mixed` : 과거 커밋 이후 모든 파일 보존하지만 unstaged 상태의 파일들은 삭제
    - `--hard` : 과거 커밋 이후 모든 파일 삭제
- revert
  - 과거 커밋 이후 특정 부분들만 없애고 커밋 이력을 남김
  - 이미 push된 커밋을 취소하고 싶을 때 사용

Reference

- [막무가내막내 - Reset, Revert 차이 간략정리](https://youngest-programming.tistory.com/220)
