# 🧠 TIL - Promise의 정체 (Promise는 무엇이며 왜 특별한가?)

## 📅 날짜
2025-11-18

---

## 📘 Promise는 무엇인가?
Promise는 **JavaScript 내장 객체(Object)** 중 하나이다.  
하지만 일반 객체와 다르게, **비동기 작업의 미래 결과를 표현하는 특별한 객체**다.

즉,

> **“지금은 결과가 없지만,  
> 나중에 작업이 끝나면 그 결과를 알려줄게”**  
>  
> 라는 약속(상태)을 담은 객체.

---

## 🔍 Promise는 단순 값이 아니다

Promise는 다음과 같은 정보를 가진 **상태 객체**다:

```js
Promise {
  [[State]]: "pending" | "fulfilled" | "rejected",
  [[Result]]: value or error
}
```

| 상태 | 의미 |
|------|------|
| **pending** | 작업 진행 중 (결과 아직 없음) |
| **fulfilled** | 작업 성공, 결과 있음 |
| **rejected** | 작업 실패, 에러 발생 |

---

## ✔ Promise가 “특별한 객체”인 이유

### 1) 비동기 결과를 담아둘 수 있음  
동기라면 값을 즉시 리턴하지만,  
비동기는 응답이 시간이 걸리므로 **즉시 결과를 줄 수 없음**.

Promise는 “완료되면 알려줄게”라는 상태를 유지한다.

### 2) 결과를 나중에 꺼낼 수 있음  
`.then()`, `.catch()` 또는 `await`을 통해  
작업 완료 후의 결과를 받을 수 있다.

### 3) 흐름 제어가 가능  
Promise는 비동기를 **동기처럼 읽히는 코드 흐름**으로 만들 수 있게 해줌.

```js
const data = await fetch(...);
```

### 4) 에러 처리 구조가 명확  
비동기 에러를 try/catch 또는 .catch로 관리 가능.

---

## 🔁 fetch와 Promise 관계

```js
const res = fetch('/api');  
console.log(res);  
// Promise { <pending> }
```

fetch는 **Promise 객체**를 반환한다.

응답이 도착해야 Promise가 “fulfilled” 상태로 바뀌고  
그 순간 Response 객체를 얻을 수 있다.

---

## ⚠️ await 없이 response.json()이 안 되는 이유

```js
const res = fetch('/api');
res.json(); // ❌ 에러
```

- 아직 응답이 오지 않았기 때문에  
- res는 **Response 객체가 아니라 Promise 객체**  
- json()은 Response 객체의 메서드 → Promise에는 없음

정상 흐름:

```js
const response = await fetch('/api'); // Promise 해제
const data = await response.json();   // JSON 추출
```

---

## 🧩 최종 요약

> **Promise는 JavaScript의 “비동기 상태를 담는 특별한 객체”다.  
> fetch는 Promise를 반환하고, await로 그 Promise가 완료되기를 기다려야  
> Response 객체와 JSON 데이터를 정확히 다룰 수 있다.**
