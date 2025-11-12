# 컬렉션
## 계층 구조

- **Collection** (인터페이스)
    - **List** (인터페이스)
        - `ArrayList` (클래스)
        - `LinkedList` (클래스)
        - `Vector` (클래스)
            - `Stack` (클래스)
    - **Set** (인터페이스)
    - **Queue** (인터페이스)


## List

### ArrayList
추가
- add(e)
- add(index, e)

가져오기
- get(index)

수정
- set(index,e)

삭제
- remove(index)
- remove(e)

크기 확인
- size()

비어있는지
- isEmpty()

포함되어있는지
- contains(e)

여러 요소 추가
- addAll(collection)

첫 번째 인덱스 찾기
- indexOf(index)

마지막 인덱스 찾기
- lastIndexOf(index)

부분 리스트 (자르기)
- subList(index, index)
- retunr type이 List

모든 요소 삭제
- clear();

collection을 배열로 변환
- toArray(변환 타입)

#### ArrayList 순회
- for-each
- for문
- Iterator
- forEach 메서드
- Stream

### LinkedList
추가
- addFirst(e) // 맨 앞에 추가
- addLast(e) // 맨 마지막

나머지 ArrayList와 동일

### ArrayList vs LinkedList
- 마지막 데이터 삽입할 때 ArrayList 빠름
- 중간 데이터 넣을 때 LinkedList 가 빠름
- 인덱스 접근 LinkedList O(N)
