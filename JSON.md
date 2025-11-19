# 🧠 TIL - JSON 완전 정리 (JSON이란? stringify ↔ parse, 서버 통신 구조)

## 📅 날짜
2025-11-18

---

# 📘 JSON이란?

**JSON (JavaScript Object Notation)**  
→ 자바스크립트 객체 문법을 기반으로 만든 **데이터 교환용 문자열 포맷**

즉,

> **겉모양은 JS 객체처럼 생겼지만 실제로는 “문자열”이다.**

예시(JSON):
```json
{
  "title": "일기",
  "content": "내용"
}
```

---

# 🔍 JSON의 문법 규칙

JSON은 **언어 독립적인 규격**이라 문법이 매우 엄격함.

- key는 반드시 **쌍따옴표 `" "`**
- string은 반드시 `" "`  
- 마지막 콤마 금지
- undefined / 함수 / Date 직접 넣기 불가
- 배열, 숫자, Boolean 가능

예:
```json
{
  "age": 20,
  "tags": ["diary", "life"],
  "isHappy": true
}
```

---

# 🧩 JS 객체와 JSON 차이

| 항목 | JSON | JS 객체 |
|------|------|---------|
| 형태 | 문자열 | 실제 객체 |
| key | 반드시 `" "` | 옵션 |
| 전송 가능? | ✔ 가능 | ❌ 불가능 |
| 실행 가능? | ❌ | ✔ JS 코드 |
| 변환 필요? | parse 필요 | stringify 필요 |

JSON은 **텍스트**,  
JS 객체는 **실제 데이터 구조**.

---

# 📦 stringify → 서버로 보낼 때

JS 객체는 전송 불가이므로  
네트워크로 보내려면 반드시 **문자열(JSON)** 로 바꿔야 한다.

```js
const obj = { title: "일기", content: "내용" };
const json = JSON.stringify(obj);  
// 결과: "{\"title\":\"일기\",\"content\":\"내용\"}"
```

### 왜 stringify가 필수인가?
- HTTP body는 **문자열 또는 바이너리만 가능**
- 객체를 그대로 보내면 fetch가 에러 냄
- JSON 문법(Escape, 따옴표 등) 자동 처리
- 서버는 JSON 문자열을 기준으로 파싱함

---

# 📥 parse → 서버에서 받을 때

서버는 응답을 **JSON 문자열**로 보낸다.

```json
"{\"title\":\"오늘의 일기\",\"content\":\"내용\"}"
```

프론트에서는 객체로 변환해야 실제로 사용 가능:

```js
const obj = JSON.parse(jsonString);
```

→ 결과:
```js
{ title: "오늘의 일기", content: "내용" }
```

---

# 🔁 전체 흐름: stringify ↔ parse 관계

```
(프론트 JS 객체)
       |
       | stringify
       v
(JSON 문자열) ---- 네트워크 ----> (서버)
                                   |
                                   | 파싱
                                   v
                          (백엔드 객체)

(서버 응답: JSON 문자열)
       |
       | parse
       v
(프론트 JS 객체)
```

**프론트-서버 통신의 기본 공식**이 바로  
**stringify → parse** 왕복 구조.

---

# 📡 서버는 왜 JSON만 쓰는 것처럼 보일까?

정확히는:

> ❌ 서버가 JSON만 읽을 수 있는 건 아니다  
> ✔ 서버는 "문자열 + Content-Type"을 보고 해석할 뿐이다

서버가 읽을 수 있는 형식들:
- application/json  
- text/plain  
- text/html  
- multipart/form-data (파일 업로드)  
- application/xml  
- x-www-form-urlencoded  

단지 **JSON이 가장 표준적이고 다루기 편해서**  
API에서 거의 JSON을 기본으로 쓰는 것뿐.

---

# 🏷 Content-Type: application/json 의 의미

```
"Content-Type": "application/json"
```

뜻:

> **“body에 JSON 문자열이 담겨있으니 JSON으로 해석해라”**

서버는 이 헤더를 보고 JSON 파서를 실행함.

헤더가 없으면?
→ 서버에서 body가 `undefined`로 들어오는 오류 발생.

---

# 🎯 최종 요약

- **JSON은 문자열이다.**
- **JS 객체는 JSON이 아니다.**  
  (모양만 비슷)
- **전송할 때는 stringify → JSON 문자열로 변환**
- **받아올 때는 parse → 객체로 변환**
- **Content-Type 헤더로 서버가 JSON인지 판단**
- **프론트-백엔드 통신의 기본 구조는 stringify ↔ parse**

> **JSON = 데이터 교환을 위한 문자열 포맷**  
> **JS 객체 = 실행 가능한 데이터 구조**  
> 둘은 다르다!
