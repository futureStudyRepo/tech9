# 조합:  Spring Boot + MyBatis + Oracle + Thymeleaf
## Spring boot에서 권장하는 Thymeleaf 로 화면 구현 

## 📦 sns 프로젝트 기술 스택 요약
> **조합:**  Spring Boot + MyBatis + Oracle + Thymeleaf + Lombok
Spring Boot에서 **Thymeleaf**를 권장하는 이유 ? 
---

### ✅ 1. **Spring Boot와의 높은 통합성**
- Thymeleaf는 Spring Boot와 자연스럽게 통합되도록 설계되어 있음.
- 별도의 설정 없이도 `starter-thymeleaf` 의존성만 추가하면 바로 사용 가능.
- Spring MVC의 모델 데이터를 템플릿에서 바로 사용할 수 있어 생산성이 높음.

```xml
<!-- Spring Boot에서는 아래처럼 설정만 해주면 끝 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
---

### ✅ 2. **HTML5 기반 템플릿 – 브라우저에서 바로 열람 가능**
- Thymeleaf는 HTML 문법에 기반하여 동작하므로,
  템플릿 파일을 웹 브라우저에서 그대로 열어 미리보기 가능 (디자이너 협업 용이).
- JSP는 서버에서 컴파일되어야 렌더링되므로 정적 HTML처럼 다룰 수 없음.

---

### ✅ 3. **템플릿 인젝션 오류 방지 (보안)**
- Thymeleaf는 기본적으로 출력 데이터를 HTML 이스케이프 처리함 (`th:text`).
- JSP에서는 별도 조치를 하지 않으면 XSS 공격에 취약할 수 있음.

---

### ✅ 4. **서버 사이드 렌더링 외에도 다양한 템플릿 렌더링 지원**
- 프래그먼트 관리 (`th:replace`, `th:include`)를 통한 재사용성 확보.
- 조건문, 반복문 등을 HTML 태그 안에서 자연스럽게 표현 가능.

```html
<!-- 예시: Thymeleaf 문법 -->
<ul>
  <li th:each="item : ${itemList}" th:text="${item.name}"></li>
</ul>
```

---

### ✅ 5. **JSP의 제약사항 제거**
- Spring Boot에서는 기본적으로 내장 톰캣을 사용하는데, JSP는 이를 완벽히 지원하지 않음.
  - JSP 컴파일은 서블릿 컨테이너 종속적 (ex: Tomcat 의존).
  - 내장 서버에서 JSP 컴파일 환경 구성은 복잡하고 번거로움.
- 반면, Thymeleaf는 순수 템플릿 렌더링이므로 내장 톰캣에서도 문제 없이 동작.

---

### ✅ 6. **활발한 커뮤니티와 문서**
- Thymeleaf는 Spring 공식 문서에서도 권장하는 템플릿 엔진.
- 다양한 튜토리얼, 예제가 많고 학습 장벽도 낮음.
---

### 🔄 요약비교

| 항목 | JSP | Thymeleaf |
|------|-----|-----------|
| Spring Boot 호환성 | 낮음 | 높음 |
| HTML 파일 미리보기 | 불가 | 가능 |
| 템플릿 재사용 | 제한적 | 용이 |
| 보안 기본 설정 | 없음 | 자동 이스케이프 |
| 서버 독립성 | 낮음 (톰캣 필요) | 높음 (내장 서버 가능) |

---
### 📦 `sns` 프로젝트 기술 스택 요약

| 분류 | 내용 |
|------|------|
| **Spring Boot 버전** | 3.3.5 |
| **JDK 버전** | 17 |
| **빌드 툴** | Maven |
| **웹 프레임워크** | Spring Web (`spring-boot-starter-web`) |
| **템플릿 엔진** | Thymeleaf (`spring-boot-starter-thymeleaf`) |
| **ORM/DB 연동** | MyBatis (`mybatis-spring-boot-starter`, 3.0.3) |
| **데이터베이스** | Oracle (`ojdbc11`, runtime 스코프) |
| **JDBC 사용** | `spring-boot-starter-jdbc` 포함 |
| **테스트 환경** | JUnit + MyBatis 테스트 (`mybatis-spring-boot-starter-test`) |
| **개발 편의 도구** | Lombok, Devtools |
| **추가 유틸리티** | Apache Commons Lang (`commons-lang3`) |

---

### 🔍 특징 분석

| 항목 | 설명 |
|------|------|
| 🧬 **MyBatis + Oracle 연동** | 실무형 데이터 접근 방식으로, Oracle DB 연동을 위한 `ojdbc11` 드라이버 사용 |
| 🌐 **템플릿 엔진** | HTML 기반 뷰 렌더링을 위한 Thymeleaf 사용 (현대적인 View 방식) |
| ⚙️ **Apache Commons Lang** | 문자열, 날짜 처리 등 다양한 유틸리티 메서드를 제공하여 코드 간결화에 도움 |
| 💡 **Devtools** | 개발 중 자동 재시작 등 빠른 피드백 가능 |
| ✅ **전형적인 Spring Boot 구조** | 테스트 및 설정도 잘 구성된 표준적인 Spring Boot 웹 앱

---

### ✅ 최종 요약 카드 (`sns`)

| 항목 | 내용 |
|------|------|
| 프로젝트 목적 | Oracle DB 기반 SNS 또는 CRUD 웹앱 |
| 주요 라이브러리 | `spring-boot-starter-web`, `thymeleaf`, `mybatis`, `ojdbc11`, `commons-lang3` |
| 템플릿 방식 | Thymeleaf |
| DB | Oracle (JDBC + MyBatis) |
| 기타 | Lombok, Devtools, MyBatis 테스트 포함 |

---

### ✅ 활용 예
```properties

spring.application.name=sns

#thymeleaf auto refresh
spring.thymeleaf.cache=false
#oracle
spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.url=jdbc:oracle:thin:@localhost:1521:xe
spring.datasource.username=jdbc
spring.datasource.password=jdbc
#mybatis
mybatis.mapper-locations=/mybatis/mapper/**/*.xml
mybatis.type-aliases-package=com.future.my.mapper
mybatis.configuration.map-underscore-to-camel-case=true
mybatis.configuration.jdbc-type-for-null=varchar

```

### 사용설정 
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
</html>
```

## 🌱 Thymeleaf 기본 문법 정리

### 1. 🔄 **변수 출력 (텍스트 출력)**

```html
<p th:text="${name}">기본값</p>
```

- `${name}`은 컨트롤러에서 넘긴 모델값
- `기본값`은 서버에서 처리 전 브라우저에서 보이는 내용 (디자인 시 확인용)
---

### 2. 🧾 **HTML 속성 바인딩**

```html
<a th:href="@{/user/{id}(id=${user.id})}">유저 페이지</a>
```

- `@{}`는 URL을 바인딩하는 문법
- 위 예시에서 `/user/3` 처럼 URL에 값이 자동 삽입됨
---
### 3. 🔁 **반복문**
```html
<ul>
  <li th:each="user : ${userList}" th:text="${user.name}">이름</li>
</ul>
```
- `userList`는 List 객체
- `user`는 각 요소
---
### 4. 🔀 **조건문**

#### if 문
```html
<span th:if="${user.isAdmin}">관리자</span>
```

#### unless 문
```html
<span th:unless="${user.isAdmin}">일반 사용자</span>
```

---

### 5. ⚙️ **값 설정 (`th:value`, `th:checked`, `th:selected`)**

```html
<input type="text" th:value="${user.name}" />
<input type="checkbox" th:checked="${user.agree}" />
<option th:selected="${user.gender == 'M'}">남성</option>
```

---

### 6. 🧩 **조각(프래그먼트) 포함**

#### 조각 만들기
```html
<!-- fragment.html -->
<div th:fragment="footer">공통 푸터입니다.</div>
```

#### 조각 사용하기
```html
<div th:replace="fragment :: footer"></div>
```

---

### 7. 🧮 **간단한 연산자**

```html
<span th:text="${user.age + 1}"></span>
<span th:text="${user.name} ?: '이름없음'"></span> <!-- Elvis operator -->
```

---

### 8. 📦 **내장 객체**

| 이름 | 설명 |
|------|------|
| `${#dates}` | 날짜 유틸 |
| `${#strings}` | 문자열 유틸 |
| `${#lists}` | 리스트 유틸 |
| `${#numbers}` | 숫자 유틸 |
| `${#maps}` | 맵 유틸 |

```html
<p th:text="${#dates.format(today, 'yyyy-MM-dd')}"></p>
```

---
## ✅ Controller 예시 (Spring Boot)

```java
@GetMapping("/hello")
public String hello(Model model) {
    model.addAttribute("name", "철수");
    return "hello";
}
```

```html
<!-- hello.html -->
<p th:text="${name}">이름</p>
```
---
| 프로젝트 | 템플릿 | DB | ORM | 배포 방식 |
|----------|--------|-----|------|------------|
| **sns** | Thymeleaf | Oracle | MyBatis | JAR | 
---
# Oracle EXTRACT 작성시간 표기를 위한 
## Oracle EXTRACT 함수 사용법 정리

Oracle 데이터베이스에서 `EXTRACT` 함수는 `DATE`, `TIMESTAMP`, `INTERVAL`과 같은 값에서 특정 날짜 또는 시간의 구성 요소를 추출하는 데 사용됩니다. 이 함수는 특정 날짜나 시간 데이터를 분석할 때 매우 유용합니다.

## 문법

```sql
EXTRACT(field FROM source)
```

- **`field`**: 추출할 날짜/시간 요소 (`YEAR`, `MONTH`, `DAY`, `HOUR`, `MINUTE`, `SECOND`, `TIMEZONE_HOUR`, `TIMEZONE_MINUTE`, `TIMEZONE_REGION`, `TIMEZONE_ABBR` 등).
- **`source`**: 날짜나 시간 값을 포함하는 `DATE`, `TIMESTAMP`, `INTERVAL` 컬럼이나 값.

## `field` 옵션 설명

| Field            | 설명                               | 사용 가능 여부          |
|------------------|----------------------------------|-----------------------|
| **YEAR**         | 연도                              | `DATE`, `TIMESTAMP`    |
| **MONTH**        | 월 (1 \~ 12)                      | `DATE`, `TIMESTAMP`    |
| **DAY**          | 일 (1 \~ 31)                      | `DATE`, `TIMESTAMP`    |
| **HOUR**         | 시 (0 \~ 23)                      | `TIMESTAMP`에서만 사용 가능 |
| **MINUTE**       | 분 (0 \~ 59)                      | `TIMESTAMP`에서만 사용 가능 |
| **SECOND**       | 초 (0 \~ 59)                      | `TIMESTAMP`에서만 사용 가능 |
| **TIMEZONE_HOUR**| 시간대의 시 부분                   | `TIMESTAMP WITH TIME ZONE`에서 사용 |
| **TIMEZONE_MINUTE** | 시간대의 분 부분                | `TIMESTAMP WITH TIME ZONE`에서 사용 |

## 주요 사용 예제

### 1. 연도 추출하기 (`YEAR`)

```sql
SELECT EXTRACT(YEAR FROM SYSDATE) AS current_year
FROM dual;
```

- 현재 연도를 추출합니다.

### 2. 월 추출하기 (`MONTH`)

```sql
SELECT EXTRACT(MONTH FROM DATE '2024-11-03') AS month_value
FROM dual;
```

- 특정 날짜에서 월을 추출합니다.

### 3. 일 추출하기 (`DAY`)

```sql
SELECT EXTRACT(DAY FROM DATE '2024-11-03') AS day_value
FROM dual;
```

- 주어진 날짜에서 일을 추출합니다.

### 4. 시간 추출하기 (`HOUR`)

```sql
SELECT EXTRACT(HOUR FROM TIMESTAMP '2024-11-03 15:45:00') AS hour_value
FROM dual;
```

- `TIMESTAMP`에서 시간을 추출합니다.

### 5. 분 추출하기 (`MINUTE`)

```sql
SELECT EXTRACT(MINUTE FROM TIMESTAMP '2024-11-03 15:45:00') AS minute_value
FROM dual;
```

- `TIMESTAMP`에서 분을 추출합니다.

### 6. 초 추출하기 (`SECOND`)

```sql
SELECT EXTRACT(SECOND FROM TIMESTAMP '2024-11-03 15:45:30.123456') AS second_value
FROM dual;
```

- `TIMESTAMP`에서 초를 추출합니다. 소수부도 포함될 수 있습니다.

### 7. 시간대 시 추출하기 (`TIMEZONE_HOUR`)

```sql
SELECT EXTRACT(TIMEZONE_HOUR FROM TIMESTAMP '2024-11-03 15:45:00 +05:30') AS tz_hour
FROM dual;
```

- 시간대가 포함된 `TIMESTAMP`에서 시간대의 시 부분을 추출합니다.
## 예제 테이블 CREATE 문

```sql

CREATE TABLE posts (
    post_id NUMBER,
    author VARCHAR2(100),
    content VARCHAR2(1000),
    post_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (post_id)
);

```

## 예제 데이터 INSERT 문

아래는 다양한 시간대(1분 전부터 한 달 전까지)에 생성된 게시글 데이터를 삽입하는 예제입니다. 작성자는 만화 `짱구`의 등장인물로 설정하였으며, 이미지 경로는 각 등장인물의 이름을 영문으로 변환하여 사용합니다.

```sql
BEGIN
    FOR i IN 1..100 LOOP
        INSERT INTO posts (post_id, author, author_img, content, post_time)
        VALUES (
            i,
            CASE MOD(i, 8)
                WHEN 1 THEN '신짱구'
                WHEN 2 THEN '짱구 엄마'
                WHEN 3 THEN '짱구 아빠'
                WHEN 4 THEN '짱아'
                WHEN 5 THEN '철수'
                WHEN 6 THEN '훈이'
                WHEN 7 THEN '맹구'
                ELSE '흰둥이'
            END,
            CASE MOD(i, 8)
                WHEN 1 THEN 'jjang-gu.png'
                WHEN 2 THEN 'jjang-gu-eomma.png'
                WHEN 3 THEN 'jjang-gu-appa.png'
                WHEN 4 THEN 'jjang-a.png'
                WHEN 5 THEN 'cheolsu.png'
                WHEN 6 THEN 'hun-i.png'
                WHEN 7 THEN 'maeng-gu.png'
                ELSE 'huindung-i.png'
            END,
            '생성된 지 ' || MOD(i, 43200) || '분 전에 생성한 게시글 내용이야',
            SYSTIMESTAMP - NUMTODSINTERVAL(MOD(i, 43200), 'MINUTE')
        );
    END LOOP;
    COMMIT;
END;
/
```
- **`post_id`**: 게시물의 고유 ID.
- **`author`**: 게시물 작성자 이름 (짱구 등장인물).
- **`author_img`**: 작성자 이미지 경로 (`영문이름.png`).
- **`content`**: 게시물 내용 (생성된 시간 정보 포함).
- **`post_time`**: 게시물이 생성된 시간 (`SYSTIMESTAMP`를 기준으로 다양한 시간대 설정).

이 스크립트는 100개의 예제 게시글을 삽입합니다. 게시글의 생성 시간은 `1분 전`부터 `약 한 달 전`(43200분)까지의 범위로 설정됩니다.