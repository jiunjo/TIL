# 🧠 TIL - JSON 완전 정리 (JSON이란? stringify ↔ parse, 프론트/서버 역할)

## 📅 날짜
2025-11-18

---

# 📘 JSON이란?

**JSON (JavaScript Object Notation)**  
→ 자바스크립트 객체 문법을 기반으로 한 **데이터 교환용 문자열 포맷**

즉,

> **겉보기엔 객체처럼 생겼지만 실제로는 “문자열”이다.**

예 (JSON 문자열):
```json
{
  "title": "일기",
  "content": "내용"
}
```

---

# 🔍 JSON 문법 규칙

JSON은 언어 독립적 포맷이라 규칙이 매우 엄격하다.

- key는 반드시 `"쌍따옴표"`
- 문자열도 `"쌍따옴표"`
- 마지막 콤마 금지
- `undefined`, 함수, Date 직접 넣기 불가
- 숫자 / Boolean / 배열 가능

---

# 🧩 JSON vs JS 객체

| 항목 | JSON | JS 객체 |
|------|------|---------|
| 타입 | **문자열** | **객체(Object)** |
| key | 항상 `" "` | 자유 |
| 전송 가능? | ✔ 가능 | ❌ 불가능 |
| 실행 가능? | ❌ | ✔ |
| 변환 필요? | parse 필요 | stringify 필요 |

겉모양이 비슷하지만 **완전히 다른 자료형**이다.

---

# 📦 stringify: JS 객체 → JSON 문자열

네트워크(body)는 **문자열만 전송 가능**하기 때문에  
객체를 그대로 보낼 수 없다.

그래서 객체를 문자열(JSON)로 변환해야 한다.

```js
const obj = { title: "일기", content: "내용" };
const jsonText = JSON.stringify(obj);
```

결과:

```
"{\"title\":\"일기\",\"content\":\"내용\"}"
```

### 왜 stringify가 꼭 필요할까?

- HTTP body는 문자열/바이너리만 허용됨  
- JSON 문법 자동 보정  
- 따옴표/줄바꿈/특수문자 escape 자동 처리  
- 유지보수 가능  
- 표준 데이터 형식(JSON)으로 서버가 해석 가능

---

# 📥 parse: JSON 문자열 → JS 객체

서버에서 응답을 보낼 때도 **JSON 문자열 형태**로 보낸다.

프론트에서는 이걸 JS 객체로 변환해야 실제로 다룰 수 있음.

```js
const obj = JSON.parse('{"title":"일기","content":"내용"}');
```

결과:

```js
{ title: "일기", content: "내용" }
```

---

# 🔁 전체 흐름: stringify ↔ parse

```
(프론트 JS 객체)
       |
       |  JSON.stringify()
       v
(JSON 문자열) ---- 네트워크 ----> (서버)
                                   |
                                   |  서버 언어의 JSON 파싱
                                   v
                         (서버 객체로 변환 후 처리)

<---- 서버 응답(JSON 문자열) ----

(프론트 JSON.parse() or response.json())
       |
       v
(프론트 JS 객체)
```

---

# 🔥 프론트 JSON.parse vs 서버 JSON 파싱은 다르다

이름은 “parse”지만 동작 환경과 언어가 완전히 다름.

### ✔ 프론트 JSON.parse()
- JS 내장 함수  
- JSON 문자열 → JS 객체

### ✔ 서버의 JSON 파싱
서버 언어마다 방식이 다름:

- Node.js → `express.json()` (내부적으로 JSON.parse)
- Python → `json.loads()`
- Java → `ObjectMapper.readValue()`
- Go → `json.Unmarshal()`

즉,  
> **프론트와 서버는 서로 다른 “파싱 시스템”을 갖고 있다.**

---

# 🏷 Content-Type: application/json

```
"Content-Type": "application/json"
```

의미:

> “이 body는 JSON 문자열이므로 JSON 파서로 해석해라.”

서버는 body 내용 자체를 보고 JSON 판단하는 게 아니라  
**Content-Type을 보고 JSON인지 결정한다.**

---

# 🎯 최종 요약

- **JSON = 문자열 기반 데이터 포맷**
- **JS 객체는 JSON 아님**
- **서버 전송 = 객체 → 문자열(stringify)**
- **서버 응답 = 문자열(JSON)**
- **프론트 사용 = 문자열 → 객체(parse)**
- **프론트 stringify / 서버 parse는 서로 다른 역할**
- **Content-Type 헤더로 JSON 파싱 여부가 결정됨**

> **JSON은 “언어를 넘나드는 공통 데이터 포맷”이고  
> stringify ↔ parse는 그 포맷을 주고받기 위한 필수 과정이다.**
