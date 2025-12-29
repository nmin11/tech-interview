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

## Promise의 상태

**개념**

Promise는 비동기 작업의 완료 또는 실패를 나타내는 객체로, 세 가지 상태 중 하나를 가진다.

- **Pending (대기)**: 초기 상태, 비동기 작업이 아직 완료되지 않은 상태
  - Promise가 생성된 직후의 상태
  - 아직 fulfilled나 rejected로 전환되지 않음
- **Fulfilled (이행)**: 비동기 작업이 성공적으로 완료된 상태
  - `resolve()` 함수가 호출되면 pending에서 fulfilled로 전환
  - `.then()` 메서드로 결과값을 처리
  - 한 번 fulfilled 상태가 되면 다시 변경되지 않음
- **Rejected (거부)**: 비동기 작업이 실패한 상태
  - `reject()` 함수가 호출되거나 에러가 발생하면 pending에서 rejected로 전환
  - `.catch()` 메서드나 `.then()`의 두 번째 인자로 에러를 처리
  - 한 번 rejected 상태가 되면 다시 변경되지 않음

**주요 특징**

- Promise의 상태는 **불변**: 한 번 settled(fulfilled / rejected)되면 변경 불가
- Pending 상태에서만 Fulfilled 또는 Rejected로 전환 가능

**예시**

```javascript
const promise = new Promise((resolve, reject) => {
  // pending 상태
  if (/* 성공 조건 */) {
    resolve(value);  // fulfilled 상태로 전환
  } else {
    reject(error);   // rejected 상태로 전환
  }
});

promise
  .then(value => {
    // fulfilled 상태일 때 실행
  })
  .catch(error => {
    // rejected 상태일 때 실행
  });
```

Reference

- [MDN - Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [JavaScript.info - Promises](https://javascript.info/promise-basics)
