# useSearchParams()

## 개념 정리

- React Router 훅.
- 내부적으로 배열을 반환함.
  [URLSearchParams객체, setSearchParams함수]
  쿼리스트링값을 읽을때, 수정할 때
- 첫 번째 요소(URLSearchParams)가 객체이므로
  searchParams.get('key') 형식으로 접근 가능.

이런구조라서 get사용가능
  class URLSearchParams {
  get(name) { /* ... */ }
  set(name, value) { /* ... */ }
  has(name) { /* ... */ }
}

-함수에는 map()이 없고,
배열엔 map()이 있다. 그래서 “객체라서 쓸 수 있다”는 말은,
그 객체의 프로토타입 체인 안에 해당 메서드가 정의되어 있어서
상속받아 사용할 수 있다는 뜻이다.

## 코드 예시

```js
const [searchParams] = useSearchParams();
const mode = searchParams.get('mode');
```
