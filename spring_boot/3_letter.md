# 조합: Spring Boot +  MyBatis + H2 DB + Thymeleaf + Lombok
## Spring boot AWS 배포를 위한 프로토타입 형태로 H2 DB 구현
## ✅ 1. **`letter` 프로젝트**  
> **조합:** Spring Boot + Thymeleaf + MyBatis + H2 + JAR 배포 + Lombok

### 📦 프로젝트 스택 정보 요약 (`letter` 프로젝트 기준)

| 분류 | 내용 |
|------|------|
| **Spring Boot 버전** | 3.3.5 |
| **JDK 버전** | 17 |
| **빌드 툴** | Maven |
| **템플릿 엔진** | Thymeleaf (`spring-boot-starter-thymeleaf`) |
| **웹 프레임워크** | Spring Web (`spring-boot-starter-web`) |
| **데이터베이스** | H2 Database (in-memory DB, `runtime` 스코프) |
| **ORM/DB 연동** | MyBatis (`mybatis-spring-boot-starter`, 버전 3.0.3) |
| **JDBC 사용 여부** | 사용함 (`spring-boot-starter-jdbc`) |
| **테스트 관련** | `spring-boot-starter-test`, `mybatis-spring-boot-starter-test` |
| **개발 편의 기능** | Spring Boot Devtools (자동 리로드, `runtime`) |
| **Lombok** | 사용 (`optional=true`) – 코드 간소화 목적 |
| **기타 설정** | `spring-boot-maven-plugin`에서 `lombok` 제외 설정 포함 |

---

### 🔍 특징 요약

- **Spring Boot 3.x** → Spring Framework 6 기반으로, Jakarta EE 전환된 API 사용 중  
- **MyBatis 기반 DB 연동**: JPA 대신 SQL 매퍼 프레임워크인 MyBatis 선택  
- **H2 DB**는 개발/테스트 용도로 사용되며, 배포 시 다른 DB로 전환할 가능성이 있음  
- **Lombok, Devtools**는 개발 생산성을 높이기 위한 선택  
- **뷰 레이어**는 Thymeleaf 기반으로 JSP가 아닌 HTML 기반 템플릿을 사용함

---

좋은 질문이십니다 선생님!  
Spring Boot에서 자주 쓰이는 **H2 Database**에 대해 개발용 사용법부터 배포 시 주의사항까지 단계별로 정리드릴게요.

---

## ✅ 1. H2 DB란?

**H2 Database**는 Java로 만들어진 **가볍고 빠른 인메모리 관계형 데이터베이스**입니다.

- **개발 및 테스트 용도**로 주로 사용됨
- 내장형(메모리) / 파일 기반 모두 가능
- 별도 설치 없이 사용 가능 (Spring Boot 의존성만 추가하면 됨)

---

## ✅ 2. Spring Boot에서 H2 DB 설정

### 📦 의존성 추가 (Maven)

```xml
<!-- pom.xml -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

### 🛠️ application.yml 또는 application.properties

```yaml
    # application.properties
    spring.application.name=letter
    server.port=8080

    # H2 database
    #memory use
    #spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    #file use
    #spring.datasource.url=jdbc:h2:file:c:/dev_spring/workspace-boot/letter/letter_db;DB_CLOSE_ON_EXIT=FALSE
    spring.datasource.url=jdbc:h2:file:/home/ubuntu/workspace/letter/letter_db;DB_CLOSE_ON_EXIT=FALSE
    spring.datasource.driverClassName=org.h2.Driver
    spring.datasource.username=sa
    spring.datasource.password=
    spring.h2.console.enabled=true
    spring.h2.console.settings.web-allow-others=true
    spring.h2.console.path=/h2-console
    spring.sql.init.mode=always

    #mybatis
    mybatis.mapper-locations=/mybatis/mapper/**/*.xml
    mybatis.type-aliases-package=com.future.my.mapper
    mybatis.configuration.map-underscore-to-camel-case=true
    mybatis.configuration.jdbc-type-for-null=varchar
```

- 테스트를 위한 테이블 (schema.sql)
```sql
    CREATE TABLE IF NOT EXISTS letter(
        seq INT PRIMARY KEY
        ,email VARCHAR(100) 
        ,from_nm VARCHAR(100) 
        ,to_nm VARCHAR(100)
        ,message_one VARCHAR(100)
        ,message_two VARCHAR(100)
        ,message_three VARCHAR(100)
    );

    CREATE SEQUENCE IF NOT EXISTS letter_seq
    START WITH 1
    INCREMENT BY 1;
```
- 테스트를 위한 데이터 (data.sql)

```sql
    INSERT INTO letter(seq, email, from_nm, to_nm, message_one
                    , message_two, message_three)
    SELECT NEXTVAL('LETTER_SEQ')
    ,'leeapgil@gmail.com'
    ,'judy','nick'
    ,'안녕 닉 잘 지내?'
    ,'봄이야 봄'
    ,'봄이 어디 있는지 짚신이 닳도록 돌아다녔건만,. 돌아와 보니 봄은 우리 집 매화나무 가지에 걸려 있었다.' 
    WHERE NOT EXISTS ( SELECT 1 FROM LETTER WHERE SEQ = 1);
```
- 쿼리가 oracle과 다름.
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.future.my.dao.ILetterDAO">

	<select id="getLetter" resultType="com.future.my.vo.LetterVO"
	                       parameterType="com.future.my.vo.LetterVO" >
	    	SELECT *
			FROM LETTER
			WHERE EMAIL = #{email}
			AND TO_NM= #{toNm}
			ORDER BY SEQ DESC
			LIMIT 1
	</select>
	
	<insert id="sendLetter" parameterType="com.future.my.vo.LetterVO">
			INSERT INTO letter (seq, email, from_nm, to_nm, message_one, message_two, message_three)
			VALUES(NEXTVAL('LETTER_SEQ'), #{email}, #{fromNm}, #{toNm}, #{messageOne}, #{messageTwo}, #{messageThree} )
	</insert>
	
</mapper>

```

---

## ✅ 3. 데이터 확인 방법 (H2 Console)

- 브라우저에서 접속  
  🔗 `http://localhost:8080/h2-console`

- JDBC URL: `jdbc:h2:mem:testdb` (기본값)
- User Name: `sa`
- Password: 공백 (설정에 따라)

> 💡 접속 안 될 경우: `spring.h2.console.enabled=true` 확인 필요

---

## ✅ 4. 개발 및 테스트 방법

- 개발 단계에서 빠른 초기화와 테스트 가능


---

## ⚠️ 5. AWS 배포 시 주의사항
### ❌ H2는 운영 환경에 **적합하지 않음**

- 휘발성 메모리 DB (`mem:` 방식)
- WAS 재시작 시 데이터 사라짐
- 동시에 여러 유저가 접근하는 실 서비스에는 적절하지 않음

### ✅ 해결 방법

| 목적 | DB 설정 |
|------|----------|
| 개발/테스트 | H2 (in-memory) |
| 운영용 | MySQL, PostgreSQL, RDS 등 외부 DB |


---

## ✅ 요약

| 항목 | 설명 |
|------|------|
| H2 DB | Java 기반의 경량 인메모리 DB |
| 개발 테스트 | 빠르게 테이블 생성/삭제 가능, 자동화 테스트 적합 |
| 데이터 확인 | `/h2-console` 접속하여 SQL 실행 가능 |

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


