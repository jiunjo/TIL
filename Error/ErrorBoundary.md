# 🧠 React 에러 처리 완전 정리 

---

# 1) ErrorBoundary (React 렌더링 오류)

1. ErrorBoundary는 React 컴포넌트 **렌더링 중 발생하는 오류**를 잡는 보호막이다.  
2. 렌더 오류가 발생해도 앱 전체가 흰 화면으로 죽지 않고 fallback UI를 보여준다.  
3. 예: undefined 접근, 잘못된 props, null 상태에서 map 돌릴 때 등.  
4. ErrorBoundary는 클래스 컴포넌트로만 만들 수 있다(getDerivedStateFromError, componentDidCatch).  
5. ErrorBoundary는 렌더 단계에서만 작동하고, 비동기/이벤트/서버 오류는 잡지 못한다.  
6. 즉 “React 화면을 그리다가 터지는 오류”만 담당한다.  
7. RootLayout에서 Outlet을 ErrorBoundary로 감싸는 패턴이 가장 실무적이다.  
8. 이 구조는 Header/Toaster 등 공통 UI는 유지되고 페이지 부분만 fallback으로 바뀐다.  
9. main.jsx에서도 전역 비상용 ErrorBoundary를 둘 수 있다.  
10. 이중 구조(전역 + 레이아웃)는 안정성과 UX 모두 챙기는 패턴이다.

---

# 2) errorElement (React Router 전용 에러)

11. errorElement는 **React Router의 라우팅 단계에서 발생한 오류**를 처리한다.  
12. loader/action/useLoaderData 실행 중 발생한 오류를 잡는다.  
13. 렌더링 오류는 errorElement가 아닌 ErrorBoundary가 처리해야 한다.  
14. 즉 errorElement = Router 엔진 에러, ErrorBoundary = React 렌더링 에러.  
15. 둘은 겹치지 않는다. 역할이 완전히 다르다.  
16. SPA에서 데이터 패칭을 loader로 할 경우 errorElement가 유용하다.  
17. 하지만 대부분의 UI 오류는 loader/action이 아니라 컴포넌트 내부에서 발생한다.  
18. 그래서 실무는 errorElement보다 ErrorBoundary 중심이다.

---

# 3) 404 (NotFoundPage) – 라우트 매칭 실패

19. 404는 “오류”가 아니라 **URL 매칭에 실패한 정상적인 상태**다.  
20. 따라서 ErrorBoundary나 errorElement가 처리하지 않는다.  
21. 반드시 Router에서 `path: "*"`로 처리해야 한다.  
22. 404가 뜬다는 건 “라우트를 못 찾았다”는 뜻이지 오류가 난 게 아니다.  
23. 실무에서는 NotFoundPage를 따로 두고 UI를 자유롭게 구성한다.

---

# 4) API(fetch/axios) 오류

24. API 통신 실패는 렌더 오류가 아니라 **네트워크/요청 실패**이다.  
25. 따라서 ErrorBoundary가 절대 잡지 못한다.  
26. `try/catch`로 직접 핸들링해야 한다.  
27. 실무에서는 catch에서 `toast.error()`로 사용자에게 안내하는 방식이 일반적이다.  
28. fetch의 res.ok가 false면 Error 객체를 수동으로 throw해야 한다.  
29. axios는 status가 400/500이면 자동으로 에러를 throw 한다.  
30. 그래서 axios가 에러 핸들링에 더 편하다는 의견도 많다.

---

# 5) 어디에 ErrorBoundary를 배치할까? (실무 구조)

31. RootLayout: 페이지/Outlet 렌더링 오류 처리 (가장 많이 쓰는 위치).  
32. main.jsx: 앱 전체 비상용 보호막.  
33. 두 개를 동시에 두면 안정성과 유지보수가 확 올라간다.  
34. 컴포넌트별로 ErrorBoundary를 따로 두어도 된다(특정 위험 컴포넌트).  
35. 예: 차트 라이브러리, 복잡한 에디터 등 에러 가능성이 높은 UI.

---

# 6) 정리 – 어떤 에러를 어디서 처리?

36. **404** → Router에서 `path: "*"`.  
37. **loader/action 오류** → errorElement.  
38. **API(fetch/axios) 오류** → try/catch + toast.  
39. **컴포넌트 렌더 오류** → ErrorBoundary.  
40. 이 네 가지를 분리하면 SPA 전체 안정성과 디버깅 속도가 극적으로 좋아진다.
