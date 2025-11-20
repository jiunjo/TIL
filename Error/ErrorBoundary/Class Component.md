# TIL: React 클래스 컴포넌트 (Class Component)

> ErrorBoundary 구현을 통해 배운 클래스 컴포넌트 핵심 개념

## 📌 클래스 컴포넌트 vs 함수형 컴포넌트

### 함수형 컴포넌트 (현대적, 주류)

```jsx
function MyComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // 사이드 이펙트
  }, []);

  return <div>{count}</div>;
}
```

### 클래스 컴포넌트 (레거시, 특수 목적)

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    // 사이드 이펙트
  }

  render() {
    return <div>{this.state.count}</div>;
  }
}
```

**현재 추세:**

- 함수형 컴포넌트 + Hooks가 표준
- 클래스 컴포넌트는 특수한 경우만 사용 (ErrorBoundary)
- React 공식 문서도 함수형 권장

---

## 🏗️ 클래스 컴포넌트 구조

### 1. Constructor (생성자)

```jsx
constructor(props) {
  super(props);  // ⭐ 반드시 먼저 호출
  this.state = { hasError: false };
}
```

**역할:**

- 컴포넌트 초기화
- state 초기값 설정
- 메서드 바인딩 (필요 시)

**super(props)가 필요한 이유:**

```jsx
// ✅ super(props) 호출
constructor(props) {
  super(props);  // 부모(React.Component) 초기화
  console.log(this.props);  // ✅ 작동
}

// ❌ super(props) 없으면
constructor(props) {
  console.log(this.props);  // ❌ undefined
}
```

---

### 2. State 관리

**함수형 vs 클래스형:**

```jsx
// 함수형
const [hasError, setHasError] = useState(false);
setHasError(true);

// 클래스형
this.state = { hasError: false };
this.setState({ hasError: true });
```

**주의사항:**

```jsx
// ❌ 직접 수정 금지
this.state.hasError = true; // 렌더링 안 됨

// ✅ setState 사용
this.setState({ hasError: true }); // 렌더링 트리거
```

---

### 3. 라이프사이클 메서드

#### 클래스 컴포넌트의 생명주기:

```
Mount (생성)
  ↓
constructor()
  ↓
render()
  ↓
componentDidMount()  ← useEffect(() => {}, [])와 유사

Update (업데이트)
  ↓
render()
  ↓
componentDidUpdate()  ← useEffect(() => {})와 유사

Unmount (제거)
  ↓
componentWillUnmount()  ← useEffect cleanup과 유사
```

#### ErrorBoundary 전용 메서드:

```jsx
// 1. getDerivedStateFromError (에러 감지)
static getDerivedStateFromError(error) {
  // state 업데이트만 (순수 함수)
  return { hasError: true };
}

// 2. componentDidCatch (에러 처리)
componentDidCatch(error, errorInfo) {
  // 부작용 허용 (toast, console, API 호출)
  toast.error('에러 발생!');
  console.error(error);
}
```

**⚠️ 중요:** 이 두 메서드는 함수형 컴포넌트에서 Hook으로 대체 불가능!
→ ErrorBoundary는 반드시 클래스 컴포넌트여야 함

---

### 4. this 바인딩

클래스 컴포넌트에서 `this`는 까다로움:

#### 문제 상황:

```jsx
class MyComponent extends React.Component {
  handleClick() {
    console.log(this); // ❌ undefined (이벤트 핸들러에서)
  }

  render() {
    return <button onClick={this.handleClick}>클릭</button>;
  }
}
```

#### 해결 방법 1: 화살표 함수 (권장 ⭐)

```jsx
class MyComponent extends React.Component {
  handleClick = () => {
    console.log(this); // ✅ MyComponent 인스턴스
  };

  render() {
    return <button onClick={this.handleClick}>클릭</button>;
  }
}
```

#### 해결 방법 2: Constructor에서 바인딩

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    console.log(this); // ✅ 작동
  }
}
```

#### 해결 방법 3: render에서 화살표 함수 (비권장)

```jsx
render() {
  return <button onClick={() => this.handleClick()}>클릭</button>;
  // ⚠️ 매 렌더링마다 새 함수 생성 → 성능 저하
}
```

---

## 🛡️ ErrorBoundary: 클래스 컴포넌트가 필수인 이유

### ErrorBoundary의 특별함:

```jsx
// ✅ 가능 (클래스)
class ErrorBoundary extends React.Component {
  static getDerivedStateFromError(error) { ... }
  componentDidCatch(error, errorInfo) { ... }
}

// ❌ 불가능 (함수형)
function ErrorBoundary() {
  // getDerivedStateFromError의 Hook 버전이 없음
  // componentDidCatch의 Hook 버전이 없음
}
```

**이유:**

- React 팀이 아직 에러 처리 Hook을 만들지 않음
- 클래스 컴포넌트만 렌더링 에러를 잡을 수 있음
- 함수형으로 전환 계획은 있지만, 현재는 클래스만 가능

---

## 📊 라이프사이클 메서드 vs Hooks 비교

| 클래스 컴포넌트              | 함수형 컴포넌트 (Hooks)                |
| ---------------------------- | -------------------------------------- |
| `constructor()`              | `useState()` 초기값                    |
| `componentDidMount()`        | `useEffect(() => {}, [])`              |
| `componentDidUpdate()`       | `useEffect(() => {})`                  |
| `componentWillUnmount()`     | `useEffect(() => { return () => {} })` |
| `this.state`                 | `useState()`                           |
| `this.setState()`            | `setState()`                           |
| `getDerivedStateFromError()` | ❌ 없음 (클래스만 가능)                |
| `componentDidCatch()`        | ❌ 없음 (클래스만 가능)                |

---

## 🎯 실무에서 클래스 컴포넌트를 만나는 경우

### 1. ErrorBoundary (현재도 사용)

```jsx
class ErrorBoundary extends React.Component {
  // 유일하게 클래스가 필수
}
```

### 2. 레거시 코드 유지보수

```jsx
// 오래된 프로젝트에서 발견
class OldComponent extends React.Component {
  // 점진적으로 함수형으로 마이그레이션
}
```

### 3. 라이브러리/패키지

```jsx
// 예전에 만들어진 npm 패키지
import { SomeClassComponent } from 'old-library';
```

---

## 💡 핵심 정리

### 클래스 컴포넌트의 핵심 개념:

1. **constructor**: 초기화, `super(props)` 필수
2. **this.state**: 상태 저장
3. **this.setState()**: 상태 업데이트
4. **render()**: JSX 반환 (필수 메서드)
5. **라이프사이클**: mount → update → unmount
6. **this 바인딩**: 화살표 함수 권장

### ErrorBoundary 특수성:

- 함수형으로 대체 불가능
- `getDerivedStateFromError`: 에러 감지 (순수 함수)
- `componentDidCatch`: 에러 처리 (부작용 허용)
- Fallback UI 제공으로 앱 전체 크래시 방지

### 실무 팁:

```
✅ 새 프로젝트: 함수형 컴포넌트 + Hooks
✅ ErrorBoundary만: 클래스 컴포넌트
✅ 레거시 유지보수: 이해 필요
❌ 새로운 일반 컴포넌트: 클래스 사용 지양
```

---

## 📚 참고 자료

- [React 공식 문서 - Error Boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)
- [React 공식 문서 - Legacy API](https://react.dev/reference/react/Component)
- ErrorBoundary 구현: `src/components/common/ErrorBoundary.jsx`

---

**작성일:** 2025-11-20  
**키워드:** React, Class Component, ErrorBoundary, Lifecycle, this binding
