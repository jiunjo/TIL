# 🧠 TIL – 클래스 컴포넌트 constructor / super

## ✅ 핵심 3줄 요약

```jsx
constructor(props) {
  super(props);           // 부모(React.Component) 기능 활성화
  this.state = { ... };   // 초기 state 설정
}
```

1. **constructor**: 컴포넌트 인스턴스 생성 시 **최초 1회 실행** (초기 준비)
2. **super(props)**: React.Component 상속 + `this.props` 사용 가능하게 만듦
3. **ErrorBoundary에서**: `this.state = { hasError: false }` 초기값 세팅만 함

---

## 🔍 constructor의 역할

constructor는 **컴포넌트 인스턴스가 생성될 때 가장 먼저 실행되는 초기 준비 공간**이다.

- props 초기화
- state 초기값 설정
- 인스턴스 생성 준비

---

## 🔑 super(props)의 역할

super(props)는 부모 클래스(React.Component)의 초기화 과정을 반드시 호출하는 코드다.

- 부모 컴포넌트 기능 상속
- this.props 초기화
- this 사용 가능하게 만듦

**즉:** super(props)는 "React.Component 기능을 켜주는 스위치"

**super를 호출하지 않으면:**  
this.props, this.state 등 인스턴스 기능을 사용할 수 없다.

---

## 🎯 ErrorBoundary에서 constructor의 실제 용도

ErrorBoundary는 constructor에서 오직 한 가지 작업만 한다:

```jsx
this.state = { hasError: false };
```

- "초기 상태 설정"
- "렌더링 준비"만 담당하며 그 외 로직은 거의 없음

---

## 💡 실무 현실

```
함수형 컴포넌트 (99%):
const [state, setState] = useState(초기값);  // constructor 불필요

클래스 컴포넌트 (1%):
constructor + super  // ErrorBoundary에서만 씀
```

---

## 🎯 한 줄 요약

constructor는 인스턴스 초기 준비 공간,  
super(props)는 부모(React.Component)의 기능을 활성화하는 코드이며,  
ErrorBoundary에서는 초기 state 세팅만 하면 충분하다.

**결론:**

- 함수형이 표준 → constructor 안 씀
- ErrorBoundary만 클래스 → constructor 필요
- "초기 state 세팅용" 그 이상도 이하도 아님 🎯

---

**작성일:** 2025-11-20  
**키워드:** React, constructor, super, 클래스 컴포넌트, ErrorBoundary
