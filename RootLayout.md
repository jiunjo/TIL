# 🧠 TIL - React Router의 RootLayout & Outlet 개념 정리

## 📅 날짜  
2025-11-18

---

## 📘 오늘 배운 내용 요약

React Router에서 **RootLayout**은 모든 페이지에 공통으로 들어가는 UI(예: Header, Footer, Layout 구조)를 한 번에 묶어 관리하기 위한 부모 레이아웃 컴포넌트다.  
RootLayout 내부에는 `<Outlet />`을 배치하는데, 이 위치에 현재 URL과 매칭된 **자식 라우트 페이지가 동적으로 렌더링**된다.  
즉, Outlet은 “children을 라우터가 자동으로 넣어주는 자리”라고 이해하면 쉽다.

---

## 🔍 핵심 개념 정리

- RootLayout은 공통 레이아웃(Header·Footer·Sidebar 등)을 한 번만 정의하도록 돕는 컴포넌트이다.  
- Outlet은 RootLayout 내부의 **자식 페이지가 꽂히는 슬롯(slot)** 역할을 한다.  
- `/` → RootLayout 렌더 → Outlet 자리에 HomePage가 들어간다.  
- `/write` → RootLayout 유지 → Outlet에 DiaryWritePage가 들어간다.  
- 공통 UI(Header 등)는 유지되고, 페이지 본문만 교체되기 때문에 SPA 구조와 매우 잘 맞는다.  
- RootLayout 사용 전에는 각 페이지에 Header를 반복해서 넣어야 했지만, 사용 후에는 RootLayout 한 곳에서만 관리하면 된다.  
- React Router는 부모 라우트 객체 안에 `children` 배열로 자식 라우트들을 정의하는데, Outlet이 이 children들을 실제 화면에 연결한다.  
- 라우팅 구조를 트리 형태로 구성할 수 있고, Outlet은 이 트리의 “자식 삽입 위치”로 동작한다.

---

## 🧩 한 줄 요약  
**RootLayout은 공통 틀, Outlet은 그 틀 안에서 현재 페이지가 동적으로 렌더링되는 구멍이다.**
