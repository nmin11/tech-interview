## `==` 연산과 `===` 연산의 차이

### 원시 타입끼리 비교

- `==` 연산자
  - 실제 값을 서로의 타입에 맞게 변환하여 비교
- `===` 연산자
  - 타입 변환 과정이 없으며, 타입과 값이 일치하는 경우에만 `true` 반환

### 참조 타입끼리 비교

- `==` 연산자와 `===` 연산자 모두 참조하는 메모리 주소를 비교

Reference

- [MDN - Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness)
- [JavaScript.info - Comparisons](https://javascript.info/comparison)

## null vs undefined

- null: 개발자가 의도적으로 할당하는 "값 없음" 명시
- undefined: 값이 할당되지 않은 상태
- 값이 없음을 명시할 때 null을 사용하고, undefined는 가급적이면 직접 사용하는 것을 피하는 것이 best practice
