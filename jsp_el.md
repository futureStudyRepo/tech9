# JSTL과 EL 개요 및 활용

## 1. JSTL 등장 배경
JSTL (JavaServer Pages Standard Tag Library)은 JSP에서 **비즈니스 로직과 뷰 로직을 분리**하고, **반복문, 조건문 등을 간결하게 표현**하기 위해 등장한 태그 라이브러리입니다.  

### 💡 기존 JSP 코드 문제점
JSP에서는 Java 코드(`<% %>`)를 직접 사용해야 했기 때문에, 코드가 복잡하고 유지보수가 어려웠습니다.

```jsp
<%
for (int i = 1; i <= 5; i++) {
    out.println("<p>반복 횟수: " + i + "</p>");
}
%>
```
이처럼 Java 코드가 HTML과 뒤섞이면서 가독성이 낮아지는 문제가 발생했습니다.

### ✅ JSTL의 해결책
JSTL은 XML 기반 태그를 사용하여 코드의 **가독성을 높이고, 유지보수를 쉽게 하도록 개선**했습니다.

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:forEach var="i" begin="1" end="5">
    <p>반복 횟수: ${i}</p>
</c:forEach>
```
- Java 코드 없이 XML 태그 형식으로 표현 가능
- 유지보수 및 협업에 유리

---

## 2. EL (Expression Language) 사용법
EL (Expression Language)은 JSP에서 **Java 코드 없이 데이터를 조회**할 수 있도록 만들어졌습니다.  
JSP에서 **`request.getAttribute("data")` 같은 복잡한 표현을 단순화**하는 역할을 합니다.

### 2.1 EL 기본 문법 (`${}`)
EL은 `${}` 형식으로 표현하며, 다양한 데이터를 출력할 수 있습니다.

#### 📌 예제: 문자열 출력
```jsp
<%
request.setAttribute("name", "홍길동");
%>

<p>이름: ${name}</p>
```
✅ 기존 방식: `<%= request.getAttribute("name") %>`  
✅ EL 방식: `${name}` (더 간결하고 가독성 높음)

---

### 2.2 EL을 활용한 객체 속성 접근
EL을 사용하면 JavaBean의 속성도 쉽게 접근할 수 있습니다.

#### 📌 예제: DTO 활용
```jsp
<%@ page import="com.example.User" %>
<%
User user = new User();
user.setName("이순신");
user.setEmail("lee@example.com");
request.setAttribute("user", user);
%>

<p>이름: ${user.name}</p>
<p>이메일: ${user.email}</p>
```
- `${user.name}` → `user.getName()`을 호출한 것과 동일
- `${user.email}` → `user.getEmail()`을 호출한 것과 동일

---

### 2.3 EL 연산자 활용
EL은 기본 연산자와 논리 연산을 지원합니다.

| 연산자 | 설명 | 예제 |
|--------|------|------|
| `+`, `-`, `*`, `/`, `%` | 사칙연산 | `${10 + 5}` → 15 |
| `eq`, `ne` | `==`, `!=` | `${num eq 10}` (`num == 10`) |
| `gt`, `lt`, `ge`, `le` | 비교 연산자 (`>`, `<`, `>=`, `<=`) | `${age gt 18}` (`age > 18`) |
| `and`, `or`, `not` | 논리 연산자 (`&&`, `||`, `!`) | `${isMember and isAdmin}` |
| `empty` | 값이 비어있는지 확인 | `${empty list}` |

#### 📌 예제: 조건문과 함께 사용
```jsp
<%
request.setAttribute("score", 85);
%>

<p>점수: ${score}</p>
<p>합격 여부: ${score ge 60 ? '합격' : '불합격'}</p>
```
- `score ge 60` → 60 이상이면 `true`
- 삼항 연산자 사용 가능 (`조건 ? 참일 때 값 : 거짓일 때 값`)

---

## 3. JSTL에서 `forEach` 활용
JSTL에서 가장 많이 사용되는 태그 중 하나가 `<c:forEach>`입니다.

### 3.1 기본적인 `forEach` 사용법
```jsp
<c:forEach var="i" begin="1" end="5">
    <p>반복 횟수: ${i}</p>
</c:forEach>
```
- `begin="1"`: 시작값
- `end="5"`: 끝값

---

### 3.2 리스트 반복
EL과 함께 리스트 데이터를 반복할 수 있습니다.

#### 📌 예제: 리스트 출력
```jsp
<%@ page import="java.util.*" %>
<%
List<String> fruits = Arrays.asList("사과", "바나나", "포도");
request.setAttribute("fruitList", fruits);
%>

<ul>
    <c:forEach var="fruit" items="${fruitList}">
        <li>${fruit}</li>
    </c:forEach>
</ul>
```
✅ 기존 Java 코드 필요 없이 리스트를 쉽게 출력 가능

---

### 3.3 `varStatus` 속성 활용
반복문의 상태 정보를 제공하는 `varStatus`를 활용하면 인덱스 정보 등을 얻을 수 있습니다.

#### 📌 예제: 인덱스 출력
```jsp
<c:forEach var="fruit" items="${fruitList}" varStatus="status">
    <p>${status.index + 1}. ${fruit}</p>
</c:forEach>
```
- `status.index` → 0부터 시작하는 인덱스 값
- `status.count` → 1부터 시작하는 반복 횟수

---

## 4. 결론
### 🎯 JSTL과 EL을 사용하면?
✅ **JSP 코드가 더 간결해지고 유지보수가 쉬워짐**  
✅ **비즈니스 로직을 최소화하고, 뷰 로직을 명확하게 분리**  
✅ **리스트 반복, 조건문, 연산을 간편하게 처리 가능**

이제 JSP에서 Java 코드를 줄이고 **JSTL과 EL을 적극 활용**해 보세요! 🚀
