## 비동기 로직들의 순차 실행 방법

### `async/await`

```ts
async function sequentialExecution() {
  const result1 = await asyncTask1();
  const result2 = await asyncTask2(result1);
  const result3 = await asyncTask3(result2);
  return result3;
}
```

- 비동기 로직들을 동기 코드처럼 작성하는 방식
- `await` 키워드는 실행이 완료될 때까지 대기를 시키므로 실행 순서 보장

### `Promise` 체이닝

```ts
asyncTask1()
  .then((result1) => asyncTask2(result1))
  .then((result2) => asyncTask3(result2))
  .catch((error) => console.error(error));
```

### 순차 실행이 아닌 병렬 실행을 한다면

```ts
const results = await Promise.all([asyncTask1(), asyncTask2(), asyncTask3()]);
```

- `Promise.all()`을 활용하면 결과는 정렬되지만 실행 순서는 병렬적
