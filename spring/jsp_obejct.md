## 📌 1. JSP 기본 객체(Built-in Object)

JSP 기본 객체는 JSP 페이지에서 별도의 선언 없이 사용할 수 있는 객체입니다.

| 객체명     | 설명                                   | 주요 메서드                        |
|------------|----------------------------------------|------------------------------------|
| request    | 클라이언트의 요청정보 처리             | `getParameter()`, `getAttribute()` |
| response   | 클라이언트로 응답 전송                 | `sendRedirect()`, `setContentType()`|
| out        | 클라이언트로 출력                      | `print()`, `println()`             |
| session    | 세션 관리                              | `setAttribute()`, `getAttribute()` |
| application| 웹 애플리케이션 전체 공유 데이터 처리  | `setAttribute()`, `getAttribute()` |
| pageContext| 페이지 내에서의 데이터 관리            | `setAttribute()`, `getAttribute()` |
| page       | JSP 페이지 자신을 나타냄 (`this`)      | -                                  |
| config     | JSP 설정 정보                          | `getServletContext()`              |
| exception  | 예외 처리 시 발생한 예외 객체 참조     | `getMessage()`                     |

### ✅ 사용 예시
```jsp
<%
    String id = request.getParameter("userId");
    out.println("사용자 아이디: " + id);
%>
```

---

## 📌 2. JavaBeans(빈)

자바빈은 데이터 객체로서 getter와 setter로 구성된 클래스로, JSP와 연동하여 데이터 처리에 활용합니다.

빈(Bean)은 **데이터의 저장 및 처리를 위해 사용하는 자바 객체(Java 객체)**로서,  
주로 JSP 또는 서블릿에서 데이터를 전달하고 관리할 목적으로 사용됩니다.

즉, 빈은 **데이터를 저장하고 다루기 위한 목적의 자바 클래스**라고 생각하면 됩니다.

### ✅ 빈(Bean)의 특징

빈으로 사용되는 클래스는 다음의 조건을 충족해야 합니다.

- **기본 생성자(Default Constructor)** 가 반드시 존재해야 합니다.
- 모든 필드는 **private 접근 제한자**를 가져야 합니다.
- 모든 필드에 대해 **getter와 setter 메서드**를 제공합니다.

이러한 규칙을 지키면 JSP 페이지에서 자동으로 데이터를 주고받을 때 편리하게 사용할 수 있습니다.

---
### (1) 빈 생성하기 (`UserBean.java`)
```java
package com.example.beans;

public class UserBean {
    private String id;
    private String name;

    public UserBean() {} // 기본 생성자 필수!

    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

### (2) JSP에서 빈 사용하기 (`useBean`, `setProperty`, `getProperty`)
```jsp
<jsp:useBean id="user" class="com.example.beans.UserBean" scope="session"/>
<jsp:setProperty name="user" property="id" value="hong123"/>
<jsp:setProperty name="user" property="name" value="홍길동"/>

아이디 : <jsp:getProperty name="user" property="id"/><br>
이름 : <jsp:getProperty name="user" property="name"/>
```

- `scope`는 빈의 유효범위를 지정합니다.  
  (`page`, `request`, `session`, `application` 중 선택)

---

## 📌 3. 세션(Session)

세션은 사용자의 정보를 서버에 저장하여 브라우저와 서버 간 상태를 유지하기 위해 사용합니다.

### (1) 세션에 데이터 저장하기
```jsp
<%
    session.setAttribute("loginId", "hong123");
%>
```

### (2) 세션에서 데이터 가져오기
```jsp
<%
    String loginId = (String)session.getAttribute("loginId");
    out.println("세션에 저장된 ID: " + loginId);
%>
```

### (3) 세션 삭제 및 무효화하기
```jsp
<%
    session.removeAttribute("loginId");  // 특정 속성 삭제
    session.invalidate();                // 세션 전체 무효화(로그아웃 처리)
%>
```

---

## 📌 전체 예제 코드 (로그인 예제)

### 로그인 폼(`loginForm.jsp`)
```html
<form action="loginProcess.jsp" method="post">
    아이디: <input type="text" name="userId"><br>
    이름: <input type="text" name="userName"><br>
    <input type="submit" value="로그인">
</form>
```

### 로그인 처리(`loginProcess.jsp`)
```jsp
<jsp:useBean id="user" class="com.example.beans.UserBean" scope="session"/>
<jsp:setProperty name="user" property="*" />

<%
    // 세션에 저장 (별도 설정 없이도 useBean이 session에 자동 저장됨)
    response.sendRedirect("loginResult.jsp");
%>
```

### 로그인 결과(`loginResult.jsp`)
```jsp
<jsp:useBean id="user" class="com.example.beans.UserBean" scope="session"/>
환영합니다, <jsp:getProperty name="user" property="name"/>님!<br>
아이디: <jsp:getProperty name="user" property="id"/>
```

---

## 📌 핵심 정리 ✨

- **기본객체**: JSP가 기본 제공하여 언제든지 사용 가능한 객체.
- **빈(JavaBeans)**: 데이터를 관리하는 클래스로 getter, setter를 이용해 JSP에서 데이터를 처리.
- **세션(Session)**: 서버에서 클라이언트 상태를 유지하기 위한 객체로 로그인, 장바구니 등 상태 유지에 사용.




