# TanStack Query

- 서버로부터 데이터 가져오기, 데이터 캐싱, 캐시 제어 등 데이터를 쉽고 효율적으로 관리할 수 있는 라이브러리

## 대표 기능

- 데이터 가져오기 및 캐싱
- 동일한 API 요청 제거(캐싱)
- 데이터 신선도 판단
- 무한 스크롤, 페이지네이션 등 성능 최적화
- 네트워크 재연결(백그라운드 상태에서 재접속), 요청 실패 등 상황에서 자동 갱신

## 데이터 캐싱

- 데이터를 가져올 때 `querykey`를 사용
- `querykey`가 존재하면
    - 신선한지 여부에 따라 데이터를 가져옴
- 존재하지 않으면
    - 서버에서 새로운 데이터를 가져옴

## 데이터 신선도

- staleTime 옵션으로 지정
- staleTime: 1000 * 10 // 10초 동안 신선한 데이터, 10초 후 데이터 갱신

## 설치

- npm i @tanstack/react-query
- npm i -D @tanstack/eslint-plugin-query
- ESLint 플로그인 사용 권장
- plugin:@tanstack/eslint-plugin-query/recommended

```javascript
rules: {
    '@tanstack/query/exhaustive-deps'
:
    'error', '@tanstack/query/stable-query-client'
:
    'error', '@tanstack/query/no-rest-destructuring'
:
    'warn'
}
```

- @tanstack/query/exhaustive-deps
    - 쿼리 함수(queryFn) 안에서 외부 변수를 쓰는데, 쿼리 키(queryKey)에 그 변수를 적지 않아 변수 값이 바뀌어도 동일한 값을 보여주는 문제
- @tanstack/query/stable-query-client
    - new QueryClient() 인스턴스를 리액트 컴포넌트 함수 내부에서 만들었을 때 리액트 컴포넌트가 리렌더링 될때마다 캐시 저장소가 날라가는 문제
- @tanstack/query/no-rest-destructuring
    - useQuery의 결과값을 받을 때 ...rest (나머지 매개변수) 문법을 사용하여 안 쓰는 컴포넌트가 변하면 불필요하게 다시 그리는 문제

## 핵심 기능

### useQuery

- 컴포넌트에서 데이터를 가져올 때 사용

```javascript
  const 반환 = useQuery < 데이터타입 > (옵션)
  ```

### queryKey

- 쿼리를 식별하는 고유한 값, 배열 형태로 지정
- 데이터마다 고유한 queryKey 를 붙여줘야 헷갈리지 않고 데이터를 관리 할 수 있음
    - 배열 형태: 반드시 ['이름', 변수] 같은 배열로 써야 함
    - 순서 중요: ['a', 'b']와 ['b', 'a']는 완전히 다른 데이터로 취급
    - 객체 내용: 배열 안에 들어가는 객체는 순서가 달라도 내용물만 같으면 같은 키로 인정 ({a:1, b:2} == {b:2, a:1})

### queryFn
- 쿼리 함수(queryFn)는 Promise를 반환하는 비동기 함수
- 데이터를 Fetch하거나 에러를 발생시키는 책임이 있음
- 반환된 Promise의 상태(Resolved/Rejected)에 따라 useQuery의 내부 상태(data, error, status 등)가 결정

### select
- 캐시된 원본 데이터를 구독하는 과정에서 실행되는 중간 데이터 처리 파이프라인
  - queryFn을 통해 서버에서 새로운 원본 데이터(Raw Data)가 도착하여 캐시가 업데이트
  - select의 새 결과값과 이전 결과값을 비교
  - 두 값이 논리적으로 같다면 새 결과값을 버리고 `이전 결과값의 메모리 주소(Reference)`를 그대로 사용
  - 컴포넌트가 구독 중인 data의 참조가 변하지 않았으므로, 리액트의 Virtual DOM 비교 단계에서 해당 컴포넌트는 업데이트 대상에서 제외

### placeholderData
- 새로운 데이터를 가져오는 과정에서 일시적으로 데이터가 없는 상태일 때 출력 화면이 깜빡이는 현상 방지
- placeholderData: keepPreviousData // 이전 데이터 출력

### structuralSharing
- 새로운 데이터를 가져올 때 이전 데이터와 비교해 변경되지 않은 부분은 이전 데이터를 재사용하도록
- 발생하는 비교 연산 비용보다, 리액트의 리렌더링 및 useMemo/useEffect 재실행으로 인한 비용이 일반적으로 훨씬 크기 때문에 기본값(true)을 유지하는 것이 유리
- 데이터가 수만 개 이상의 거대한 배열이어서 비교 연산 자체가 메인 스레드를 수 초간 점유하는 특이 케이스가 아니라면, 항상 true로 두는 것이 리액트 아키텍처 관점에서 훨씬 효율적

### meta
- 쿼리 함수의 결과에 대한 임의의 부가 정보를 저장하는 객체
- 조건부 전역 알림
  - 특정 쿼리에서만 에러 토스트를 띄우고 싶을 때 meta: { showToast: true }와 같은 플래그를 설정합니다. 
- 권한 기반 처리
  - 특정 API 호출 실패 시 리다이렉트 경로를 meta: { redirectUrl: '/login' }으로 지정하여 전역에서 처리합니다. 
- 로깅 세분화 
  - 분석 로깅을 위해 쿼리별 식별 명칭을 meta: { logId: 'fetch_user_list' } 형태로 전달합니다.

### isFetching
- 비동기 쿼리 함수의 실행 상태를 나타내는 Boolean
- status와 별개로, 현재 네트워크 요청이 진행 중인지 여부를 실시간으로 반영

### refetch
- 캐시된 데이터의 유효성이나 staleTime 설정과 관계없이, 명시적으로 queryFn을 다시 실행하여 서버로부터 최신 데이터를 가져옴
- invalidateQueries 는 stale 상태를 마킹하여 active query면 리프레쉬, inactive query면 리프레쉬 x

### useInfiniteQuery
- 추가 데이터를 가져오거나, 무한스크롤 기능

### useMutation
- 데이터 변경 작업(생성, 수정, 삭제 등)을 위한 훅
- 데이터를 조회하여 캐싱하는 useQuery와 달리, useMutation은 실행 시점의 제어와 성공/실패에 따른 후속 캐시 전략에 초점





## 출처

- https://www.heropy.dev/p/HZaKIE