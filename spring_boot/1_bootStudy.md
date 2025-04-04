# 조합: Spring Boot + JSP + MyBatis + Oracle + WAR 배포 + Lombok
## Spring legacy MVC 프로젝트 스택과 비슷한 구조로 구현 
## ✅ 1. **`bootStudy` 프로젝트**  
> **조합:** Spring Boot + JSP + MyBatis + Oracle + WAR 배포 + Lombok

### 📌 설명  
- JSP 기반의 전통적인 웹 개발 구조를 유지하면서 Spring Boot에 접목  
- `WAR` 형태로 빌드하여 외부 Tomcat 서버에 배포 (내장 톰캣은 `provided`)  
- Oracle과 연동하여 실무형 DB 처리 구현 가능
---

### 📦 `bootStudy` 프로젝트 기술 스택 

| 분류 | 내용 |
|------|------|
| **Spring Boot 버전** | 3.3.4 |
| **JDK 버전** | 17 |
| **패키징 방식** | WAR (→ 외부 톰캣 배포 전제) |
| **웹 프레임워크** | Spring Web (`spring-boot-starter-web`) |
| **템플릿 엔진** | **JSP + JSTL** (`tomcat-embed-jasper`, `jakarta.servlet.jsp.jstl`) |
| **ORM/DB 연동** | MyBatis (`mybatis-spring-boot-starter`, 3.0.3) |
| **데이터베이스** | Oracle (`ojdbc11`, `runtime`) |
| **서버 설정** | 내장 톰캣 제거 (`provided` scope), WAR로 외부 톰캣 배포 |
| **JDBC 사용** | `spring-boot-starter-jdbc` 포함 |
| **테스트 환경** | JUnit + MyBatis 테스트 지원 포함 |
| **개발 편의 도구** | Lombok 사용 (`optional`) |

---

### ✅ 최종 요약 카드 (`bootStudy`)

| 항목 | 내용 |
|------|------|
| 프로젝트 목적 | Spring Boot 기반 JSP + MyBatis + Oracle 웹앱 개발 |
| 주요 라이브러리 | `spring-boot-starter-web`, `jdbc`, `mybatis`, `ojdbc11`, `JSTL`, `tomcat-embed-jasper` |
| 템플릿 방식 | JSP + JSTL |
| DB | Oracle (JDBC 연동) |
| 배포 방식 | WAR → 외부 Tomcat 배포 전제 |
| 기타 | Lombok, 테스트 의존성 포함 |

---

### ✅ 활용 예
```properties

spring.application.name=bootStudy

#jsp
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

#db
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

- pom.xml
```xml

		<!-- JSTL for JSP -->
		<dependency>
			<groupId>jakarta.servlet</groupId>
			<artifactId>jakarta.servlet-api</artifactId>
		</dependency>
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>jakarta.servlet.jsp.jstl</artifactId>
		</dependency>
		<dependency>
			<groupId>jakarta.servlet.jsp.jstl</groupId>
			<artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
		<!-- JSTL for JSP -->
		

```

- mybatis mapper(member.xml)
```xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "//mybatis.org/DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.future.my.member.dao.IMemberDAO">
	
	<select id="memList" resultType="com.future.my.member.vo.MemberVO">
		SELECT mem_id
		     , mem_nm
		FROM members
	</select>
	
</mapper>

```

---
| 프로젝트 | 템플릿 | DB | ORM | 배포 방식 | 특징 |
|----------|--------|-----|------|------------|--------|
| **bootStudy** | JSP | Oracle | MyBatis | WAR | 실무형 JSP, 외부 Tomcat 필요 |
---