# 🧠 TIL – ErrorBoundary 완전 정리

React에서 컴포넌트를 렌더링하는 도중 오류가 발생하면  
앱 전체가 흰 화면으로 죽을 수 있다.  
이를 방지해 오류 구간만 안전하게 대체 UI로 바꿔주는 장치가 **ErrorBoundary**다.

정상 렌더 → 원래 UI
렌더 오류 → fallback UI

---

## 1. ErrorBoundary란?

ErrorBoundary는 **React 렌더링 단계에서 발생한 오류를 감지**하고  
앱 전체가 중단되지 않도록 오류가 발생한 부분만 **fallback UI(대체 화면)**로 렌더링하는 보호막이다.

---

## 2. ErrorBoundary가 잡을 수 있는 오류

✔ 렌더링 과정에서 발생한 예외  
- undefined.xxx 접근  
- 잘못된 props 구조  
- null 상태에서 map() 호출  
- 컴포넌트 내부 throw new Error()  
- 자식 컴포넌트 렌더 오류도 상위 ErrorBoundary가 모두 잡는다

---

## 3. ErrorBoundary가 잡지 못하는 오류

❌ 이벤트 핸들러(onClick) 오류  
❌ async/await, Promise 내부 오류  
❌ fetch/axios 같은 API 요청 실패  
❌ setTimeout/setInterval 내부 오류  
❌ 404(라우트 매칭 실패)  
❌ React Router loader/action 오류  

➡️ 이런 오류는 ErrorBoundary 영역이 아니므로  
try/catch, toast, 라우터 설정 등으로 각각 처리해야 한다.

---

## 4. fallback UI란?

fallback UI는 렌더 오류가 발생했을 때  
오류가 난 컴포넌트 대신 보여주는 **안전한 대체 화면**이다.

예시:

```jsx
<h2>문제가 발생했습니다. 잠시 후 다시 시도해주세요.</h2>
```

이 UI는 오류가 난 부분만 대체하며,
Header/Sidebar 등 나머지 UI는 그대로 유지된다.

⸻

## 5. ErrorBoundary가 필요한 이유

React는 렌더 트리 어딘가에서 오류가 발생하면
그 아래 전체 UI 렌더가 중단된다.
그래서 작은 오류 하나로도 앱 전체가 흰 화면이 되는 문제가 발생한다.

ErrorBoundary를 사용하면:
	•	오류난 부분만 fallback UI로 교체
	•	나머지 화면은 정상 유지
	•	사용자 경험(UX) 개선
	•	디버깅 시 어떤 위치에서 오류가 났는지 명확해짐

⸻

## 6. 실무에서의 배치 위치 (추천 패턴)

대부분의 렌더 오류는 페이지(Outlet) 영역에서 발생한다.
따라서 실무에서는 RootLayout에서 Outlet만 ErrorBoundary로 감싸는 구조를 가장 많이 사용한다.

예:
```jsx
<Header />
<ErrorBoundary>
  <Outlet />   {/* 페이지 렌더 영역 */}
</ErrorBoundary>
<Toaster />
```

장점:
	•	Header, Footer, Toaster 같은 공통 UI는 그대로 유지됨
	•	페이지 부분에서 오류가 나면 fallback UI만 교체됨
	•	UX 안정성 높음
	•	유지보수가 매우 쉬운 구조

또한 필요하면 main.jsx에 전역 ErrorBoundary를 추가하여
앱 전체 렌더 자체가 실패할 경우에도 대비할 수 있다.

⸻

📌 최종 요약
	•	ErrorBoundary는 렌더링 오류 전용 보호막이다.

	# 🧠 TIL - 클래스 컴포넌트 vs 함수형 컴포넌트 + ErrorBoundary가 여전히 클래스인 이유

## 📅 날짜
2025-11-20

---

## 📘 오늘 배운 내용 요약

React에서는 화면 컴포넌트를 만들 때 과거에는 클래스 컴포넌트를 사용했지만,  
React 16.8 이후 Hooks가 도입되면서 **함수형 컴포넌트가 사실상 표준이 되었다.**

일반적인 UI 컴포넌트 제작은 함수형만으로 충분하며,  
상태 관리·생명주기·이벤트 처리 등 모든 기능이 Hooks로 대체 가능하다.

하지만 단 하나, **ErrorBoundary(에러 경계)** 기능만큼은  
현재도 클래스 컴포넌트로만 구현 가능하다.  
React 공식 문서에서도 "Error Boundaries must be class components"라고 명시하고 있다.

---

## 🔍 핵심 개념 정리

### 1. 클래스 컴포넌트
- 문법 형태:
  ```js
  class MyComponent extends React.Component {
    render() {
      return <div>Hello</div>;
    }
  }
	•	정상 렌더 → 원래 UI
	•	렌더 오류 → fallback UI
	•	async/await, API 오류, 404는 ErrorBoundary 감지 범위 밖
	•	RootLayout에서 Outlet을 감싸는 방식이 실무 최강
	•	main.jsx에 전역 ErrorBoundary 추가 가능

