# 🧠 TIL – ErrorBoundary 완전 정리

React에서 컴포넌트 렌더링 중 오류가 나면  
앱 전체가 흰 화면으로 죽는다.  
이를 막기 위한 보호막이 **ErrorBoundary**다.

---

## 1. ErrorBoundary란?

React 컴포넌트 렌더링 단계에서 발생한 오류를 잡고  
앱이 전체 종료되지 않도록 대신 **fallback UI**를 보여주는 장치.

즉: 정상 렌더 → 원래 UI
렌더 오류 → fallback UI 
---

## 2. ErrorBoundary가 잡는 오류

✔ **렌더링 중 오류**  
- undefined 접근  
- props 오류  
- null 상태에서 map()  
- 컴포넌트 내부 throw new Error()

✔ **자식 컴포넌트에서 발생한 렌더 오류도 모두 캐치**

---

## 3. ErrorBoundary가 잡지 못하는 오류

❌ 이벤트 핸들러 오류  
❌ async/await 내부 오류  
❌ fetch/axios 요청 실패  
❌ 타이머(setTimeout) 내부 오류  
❌ 404, 잘못된 라우트  
❌ React Router의 loader/action 오류

이런 건 ErrorBoundary랑 전혀 관계 없음.

---

## 4. fallback UI란?

렌더 오류가 발생했을 때  
“원래 컴포넌트 대신 보여주는 대체 화면”.

예:

```jsx
<h2>문제가 발생했습니다. 잠시 후 다시 시도해주세요.</h2>

---

## 5. 왜 필요한가?

React는 하나의 렌더 오류 때문에
앱 전체 렌더가 중단될 수 있음 → 흰 화면.

ErrorBoundary가 이걸 막는다.

---

## 6. 실무배치하는곳

<Header />
<ErrorBoundary>
  <Outlet />   // 페이지 렌더 영역
</ErrorBoundary>
<Toaster />
