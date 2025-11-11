# 🧠 TIL - React onChange / 상태 관리 흐름 정리

## 📅 날짜

2025-11-11

---

## 📘 오늘 배운 내용 요약

React에서 `input`을 제어할 때는 **상태(state)** 와 **이벤트(onChange)** 를 함께 사용해야 한다.  
`title`만 주면 read-only 경고가 발생한다 왜냐면 컴퓨터가 언제 setTitle을 실행시켜하지는몰라서, `onChange`로 상태를 업데이트하는 함수로 전달해야 한다.
그리고 e라는 리액트의 특별한 이벤트 객체로 전달해서 target하고 value를
setTitle을 이용해 title이 업데이트되게 만들어야함.

---

## 🔍 핵심 코드

```jsx
const [title, setTitle] = useState('');

<input value={title} onChange={(e) => setTitle(e.target.value)} />;
```
