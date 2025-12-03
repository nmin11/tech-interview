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
