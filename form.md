## 📝 React Form 제출 방식 + preventDefault + 새로고침 이유 (TIL)

- **Form(form)**은 원래 HTML 시대부터 있던 태그로, “서버로 데이터 전송 + 새 페이지 로드”가 기본 동작이다.
- 그래서 `<form>`을 제출하면 브라우저가 기본적으로 페이지를 새로고침하며 서버로 요청을 보낸다.
- React(SPA)에서는 새로고침이 발생하면 상태(state)가 모두 초기화되어 앱 구조가 깨진다.
- 그래서 React에서는 `onSubmit`에서 **e.preventDefault()**를 사용해 기본 제출(새로고침)을 막는다.
- `e.preventDefault()`는 “브라우저가 form 제출 시 수행하는 기본 새로고침 동작을 차단”하는 코드이다.
- 이걸 막아야 React가 의도한 SPA 흐름(상태 유지, 라우팅 유지)을 유지할 수 있다.

- 버튼에서 `onClick={() => save()}`로 직접 저장 호출하는 방식은 비추.
- 이유1: Enter 제출 / 모바일 키보드 전송 버튼이 작동하지 않음.
- 이유2: `<input required>` 같은 브라우저 기본 유효성 검사가 동작하지 않음.
- 이유3: UI(Form)와 로직(save)이 섞여 구조가 지저분해짐.

- 정석 패턴:
  - Form 내부에서 입력값 state 관리.
  - 제출은 `onSubmit`에서 처리 → `e.preventDefault()`로 새로고침 차단.
  - 이후 `onSubmit(data)`로 Page에 넘기고, Page에서 save() 호출.

- 결론: **Form onSubmit + preventDefault + Page에서 save**가 실무 표준 구조.
