# useSearchParams()

## 개념 정리

- React Router 훅.
- 내부적으로 배열을 반환함.
  [URLSearchParams객체, setSearchParams함수]
- 첫 번째 요소(URLSearchParams)가 객체이므로
  searchParams.get('key') 형식으로 접근 가능.

## 코드 예시

```js
const [searchParams] = useSearchParams();
const mode = searchParams.get('mode');
```
