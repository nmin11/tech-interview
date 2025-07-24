## Entity 생명주기

- **비영속** : Entity 객체를 생성했지만 영속성 컨텍스트에 저장하지 않은 상태
- **영속** : Entity 객체를 영속성 컨텍스트에 저장한 상태
- **준영속** : Entity 객체가 영속성 컨텍스트에서 분리된 상태
- **삭제** : Entity 객체가 영속성 컨텍스트 및 DB에서 모두 삭제된 상태

## N+1 Select 문제

**정의**

- 부모 Entity를 조회했을 때, 자식 Entity 수만큼 추가 쿼리가 발생하는 현상

**발생 원인**

- 주로 Lazy Loading 방식 때문에 발생
  - Lazy Loading 방식은 JPA의 기본 fetch 방식

**해결 방법 1 - JPQL JOIN FETCH**

```java
@Query("SELECT DISTINCT t FROM Team t JOIN FETCH t.users")
List<Team> findAllWithUsers();
```

장점

- 쿼리 1회로 모든 Entity 로딩

단점

- join 된 테이블의 모든 데이터를 반환하는 cartesian product 문제 발생 가능
- 중복된 결과를 방지하기 위해 `DISTINCT` 필요

**해결 방법 2 - EntityGraph**

```java
@EntityGraph(attributePaths = {"users"})
List<Team> findAll();
```

장점

- Eager fetching 방식에 비해 유연함
- 프로그래밍 방식 혹은 annotation 방식으로 정의 가능

단점

- 더 복잡해진 설정
- 복잡한 연관관계가 있을 경우 cartesian product 문제 발생 가능

**해결 방법 3 - BatchSize**

```java
@BatchSize(size = 20)
private List<Comment> comments;
```

```yml
spring:
  jpa:
    properties:
      hibernate:
        default_batch_fetch_size: 100
```

N+1 문제를 제거하는게 아닌, (N / batch_size) + 1 문제로 줄여줌

장점

- 구현이 쉬움
- 쿼리 수를 줄이는 목적은 달성
- Lazy Loading 방식에서 잘 작동

단점

- 여전히 여러 건의 쿼리를 사용

Reference

- [Arvind Kumar - Understanding and Solving the N+1 Select Problem in JPA](https://codefarm0.medium.com/understanding-and-solving-the-n-1-select-problem-in-jpa-907c940ad6d7)
