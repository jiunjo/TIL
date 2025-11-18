# 🧠 TIL - React Router의 RootLayout & Outlet 개념 + 라우트 배열 구조 이해

## 📅 날짜  
2025-11-18

---

## 📘 오늘 배운 내용 요약

React Router에서 **RootLayout**은 모든 페이지의 공통 레이아웃(Header·Footer 등)을 한 번에 정의하는 부모 컴포넌트다.  
그 안에 `<Outlet />`을 배치하면, 현재 URL에 매칭된 **자식 라우트 페이지가 동적으로 렌더링**된다.  
Outlet은 “children을 라우터가 자동으로 꽂아주는 슬롯(slot)” 역할을 한다.  
React Router는 라우트를 **배열로 정의**하는데, 이는 라우트가 여러 개일 수도 있고 순서가 중요한 “규칙 목록(list)”이기 때문이다.

---

## 🔍 핵심 개념 정리

- RootLayout은 전역 UI(예: Header, ThemeProvider)를 한 번만 작성하게 만들어주는 부모 레이아웃이다.  
- Outlet은 RootLayout 안의 비워둔 자리로, 현재 경로에 맞는 자식 페이지가 이 위치에 들어간다.  
- `/` → RootLayout 렌더 → Outlet에 HomePage 삽입  
- `/write` → RootLayout 유지 → Outlet에 DiaryWritePage 삽입  
- 페이지가 바뀌어도 공통 UI는 그대로 유지되고, Outlet만 교체되기 때문에 SPA UX가 자연스럽게 구현된다.  
- RootLayout 사용 전에는 모든 페이지에 Header를 직접 넣어야 해서 코드 중복이 발생한다.  
- 부모 라우트는 객체 하나이지만, React Router는 여러 라우트 규칙을 다루기 위해 **항상 배열 형태**로 라우트 리스트를 받는다.  
- 배열이 필요한 이유: (1) 라우트 개수가 유동적이기 때문, (2) 매칭 우선순위가 중요하기 때문, (3) 중첩 라우트(children)를 트리로 표현하기 편하기 때문.  
- 객체는 “각 라우트의 정보 묶음(path, element, children 등)”이고, 배열은 “라우트 규칙들의 목록(list)”을 표현한다.

---

## 🧩 한 줄 요약  
**RootLayout은 공통 레이아웃 틀, Outlet은 그 틀 안에서 현재 페이지가 꽂히는 자리이며, 라우트 배열은 라우팅 규칙 자체가 ‘순서 있는 리스트’이기 때문에 사용된다.**
