## SQL과 JPQL에서의 `DISTINCT` 차이점

- SQL의 DISTINCT : 필드 조합이 완전히 동일한 행(row)에 대한 중복 제거
- JPQL의 DISTINCT : JPA Entity 중복을 애플리케이션 단에서 제거
  - 데이터베이스에서의 중복 제거 + 루트 엔티티의 중복 제거
