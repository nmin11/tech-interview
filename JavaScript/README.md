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

## Promise.all vs Promise.allSettled

### Promise.all

- 모든 Promise가 fulfilled 될 때까지 대기
- **하나라도 rejected되면 즉시 전체가 reject**되고 나머지 Promise는 무시
- 반환값: 모든 Promise의 결과값을 담은 배열

```javascript
Promise.all([promise1, promise2, promise3])
  .then((results) => {
    // 모든 Promise가 성공한 경우
    // results = [result1, result2, result3]
  })
  .catch((error) => {
    // 하나라도 실패하면 즉시 여기로
  });
```

**사용 케이스**

- 모든 비동기 작업이 성공해야만 다음 단계로 진행 가능한 경우
- 예: 여러 API 호출 결과를 모두 받아야 페이지를 렌더링하는 경우

### Promise.allSettled

- 모든 Promise가 settled(fulfilled 또는 rejected)될 때까지 대기
- **각 Promise의 성공/실패 여부와 관계없이 모든 결과를 반환**
- 반환값: 각 Promise의 상태와 결과/에러를 담은 객체 배열

```javascript
Promise.allSettled([promise1, promise2, promise3]).then((results) => {
  // 항상 여기로 옴 (실패한 Promise가 있어도)
  // results = [
  //   { status: 'fulfilled', value: result1 },
  //   { status: 'rejected', reason: error2 },
  //   { status: 'fulfilled', value: result3 }
  // ]
});
```

**사용 케이스**

- 일부 비동기 작업이 실패해도 나머지 결과를 받아야 하는 경우
- 예: 여러 API 호출 중 일부가 실패해도 성공한 데이터는 표시해야 하는 경우

### 동작 방식 비교 예시

```javascript
const promises = [
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3),
];

// Promise.all - 즉시 reject
Promise.all(promises)
  .then((results) => console.log(results)) // 실행 안됨
  .catch((error) => console.log(error)); // 'error' 출력

// Promise.allSettled - 모든 결과 반환
Promise.allSettled(promises).then((results) => console.log(results));
// [
//   { status: 'fulfilled', value: 1 },
//   { status: 'rejected', reason: 'error' },
//   { status: 'fulfilled', value: 3 }
// ]
```

Reference

- [MDN - Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [MDN - Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
