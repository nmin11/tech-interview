## SQL 명령어의 실행 순서

### 1. FROM / JOIN

데이터를 가져올 테이블 및 테이블 간 JOIN 작업을 우선 수행

### 2. WHERE

조건에 맞도록 데이터를 필터링, **그룹화 전에 진행**

### 3. GROUP BY

남은 데이터 그룹화, 그룹 단위 집계 준비

### 4. HAVING

그룹화 결과에 대해 집계 함수를 적용한 필터링 수행, **그룹화 후에 동작**

### 5. SELECT

필요한 컬럼을 선택하고 연산 및 alias 등 적용

### 6. DISTINCT

SELECT 이후 중복된 행 제거

### 7. ORDER BY

최종 결과 정렬, alias 사용 가능

### 8. LIMIT / OFFSET

행 개수를 제한하거나 범위를 특정

Reference

- [Order of execution of a Query](https://sqlbolt.com/lesson/select_queries_order_of_execution)
