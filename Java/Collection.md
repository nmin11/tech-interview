## ArrayList vs LinkedList

**ArrayList**

- 내부적으로 동적 배열 사용
- 요소들은 연속된 메모리 공간에 저장됨

**LinkedList**

- 이중 연결 리스트 기반
- 각 노드가 이전/다음 노드에 대한 참조를 가짐

**시간 복잡도**

| 연산                    | ArrayList        | LinkedList       | 비고 |
|:----------------------:|:----------------:|:----------------:|:------|
| `get(i)`               | O(1)             | O(N)             | 인덱스 직접 접근 vs 노드를 처음부터 탐색 |
| `set(i, e)`            | O(1)             | O(N)             | 바로 접근해서 수정 vs 해당 노드 탐색 후 수정 |
| `add(e)`               | O(1) / O(N)      | O(1)             | (끝 삽입) 끝에 추가 or 공간이 없을 때 리사이징 vs tail 포인터 활용 |
| `add(i, e)`            | O(N)             | O(N)             | (중간 삽입) 뒷 부분 shift 작업 vs 탐색 + 연결 포인터 조정 |
| `remove(i)`            | O(N)             | O(N)             | 뒷 부분 shift 작업 vs 탐색 + 연결 포인터 조정 |
| `contains(e)`          | O(N)             | O(N)             | 둘 다 순차 탐색 필요 |
| `indexOf(e)`           | O(N)             | O(N)             | 둘 다 순차 탐색 필요 |

**실용 Tip**

- 읽기 중심, 랜덤 접근이 많은 경우 ⇒ `ArrayList`
- 중간 삽입/삭제가 많은 경우 ⇒ `LinkedList`
- 사실 대부분의 경우 `ArrayList`가 빠르고 메모리 효율적
