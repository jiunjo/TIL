# 🧠 TIL – React Router `errorElement` 정리 

`errorElement`는 React Router v6.4+의 기능으로,  
라우터의 **loader / action 단계에서 발생한 오류**를 처리하는 전용 에러 UI이다.  
즉, “라우트는 맞는데 데이터를 로드하는 과정에서 실패한 경우”에 실행된다.

errorElement는 컴포넌트 렌더링 오류가 아니라  
라우터가 페이지 진입을 위해 데이터를 준비하는 동안 발생한 오류만 잡는다.

사용 예시는 다음과 같다:

```jsx
{
  path: '/diary/:id',
  element: <DiaryPage />,
  loader: async ({ params }) => {
    const res = await fetch(`/api/diary/${params.id}`);
    if (!res.ok) throw new Error("데이터 없음");
    return res.json();
  },
  errorElement: <LoaderErrorPage />
}
```
여기서 loader에서 throw하면 컴포넌트는 렌더되지 않고
바로 LoaderErrorPage가 렌더된다.
즉, errorElement는 “데이터가 준비되기 전에” 일어나는 오류를 처리하는 UI이다.

errorElement가 잡는 오류는 다음과 같다:
	•	loader 내부에서 throw된 Error
	•	action 내부에서 throw된 Error
	•	loader/action에서 fetch 실패 후 throw
	•	loader 함수가 데이터를 return하지 못하는 상황(null 접근 등)

반대로 errorElement가 잡지 못하는 오류는 다음과 같다:
	•	컴포넌트 렌더링 오류 (ErrorBoundary가 담당)
	•	onClick 등 이벤트 핸들러 오류
	•	컴포넌트 내부의 fetch/axios 실패 (try/catch + toast 필요)
	•	404(라우트 매칭 실패) → path: “*“로 처리
	•	setTimeout 같은 비동기 오류

errorElement는 loader/action과 함께 설계된 기능이며
Next.js나 CSR(fetch/axios 중심 개발)에서는 사용할 일이 거의 없다.
실제로 많은 프로젝트는 loader/action을 사용하지 않기 때문에
errorElement도 자연스럽게 쓰지 않는 경우가 많다.

핵심은 다음과 같다:
errorElement는 React 렌더링이 시작되기 전
“라우트 데이터 로딩 단계에서 발생한 오류를 처리하는 UI”이다.
렌더링이 시작된 후의 오류는 ErrorBoundary가 담당한다.

errorBoundary != errorElement
렌더링 오류 != 로더 오류
이 둘을 구분하는 것이 가장 중요하다.
