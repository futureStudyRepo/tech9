# 🌱 Spring Boot 정리

## 1. Spring Boot란?

Spring Boot는 **Spring Framework의 복잡한 설정을 최소화**하고, **독립 실행 가능한 애플리케이션을 빠르게 개발**할 수 있도록 도와주는 프레임워크입니다.

> ✅ "Convention over Configuration" 철학에 따라 개발자가 설정해야 할 부분을 최소화
---

## 2. 핵심 특징

| 기능 | 설명 |
|------|------|
| 🚀 **자동 설정 (Auto Configuration)** | 스프링 환경 구성 자동화 (DB, Web 등) |
| 🧱 **내장 웹서버 지원** | Tomcat, Jetty, Undertow 내장 가능 |
| 📦 **의존성 관리** | Starter 의존성으로 필요한 라이브러리를 한 번에 관리 |
| 🧪 **독립 실행 가능 JAR** | `java -jar` 명령으로 바로 실행 가능 |
| 📊 **Actuator 제공** | 헬스 체크, 메트릭 등 운영 도구 내장 |
| ⚙️ **설정 파일 통일** | `application.yml` 또는 `application.properties` 기반 설정 |

---

## 3. 장점

- ✅ 설정이 간편하고 빠른 개발 가능
- ✅ 테스트 및 운영환경 구성 용이
- ✅ 마이크로서비스(MSA) 개발에 적합
- ✅ 클라우드 및 Docker/Kubernetes 환경에 최적화
- ✅ 방대한 커뮤니티와 레퍼런스

---

## 4. Spring Legacy MVC와의 비교

| 항목 | Spring Legacy MVC | Spring Boot |
|------|--------------------|--------------|
| 설정 방식 | XML 중심 또는 Java Config | Auto Configuration 기반 |
| 웹 서버 | 외부 Tomcat, WAS 필요 | 내장 Tomcat 기본 제공 |
| 빌드 | WAR 생성 후 WAS에 배포 | JAR 생성 후 바로 실행 |
| 개발 속도 | 초기 설정 복잡 | 빠르고 간단한 시작 가능 |
| 운영 기능 | 직접 구성 필요 | Actuator 등 내장 운영 도구 |

---

## 5. 왜 많이 사용되는가?

- 📌 개발 생산성 향상 → **빠른 프로토타입 제작**
- 📌 DevOps 및 클라우드 친화적 구조
- 📌 학습 및 진입 장벽 낮음 → 교육용/스타트업에서도 활용 쉬움
- 📌 Spring 생태계와 완벽 호환 (JPA, Security, Data, etc.)
- 📌 실무에서 바로 적용 가능한 표준 구조 제공

---

## 6. 가능한 기술 스택 조합

### 📄 템플릿 엔진 (View)
- Thymeleaf
- JSP (비권장)
- Freemarker
- Mustache

### 💻 프론트엔드 연동
- React
- Vue.js
- Angular
> → REST API 기반 분리 구조

### 🗄 데이터베이스 연동
- ORM: Spring Data JPA, Hibernate
- SQL 매퍼: MyBatis
- 기타: Spring Data JDBC

### 💾 지원 DB
- H2 (개발용)
- MySQL, MariaDB
- PostgreSQL
- Oracle
- MongoDB

### 🔐 인증/보안
- Spring Security
- JWT 기반 인증
- OAuth2 (Google, Naver, etc.)

### 📤 메시징
- Kafka
- RabbitMQ

### 🧪 테스트
- JUnit5
- Mockito

### 📦 배포/운영
- Docker
- Kubernetes
- AWS (EC2, Elastic Beanstalk)
---

## 7. Maven vs Gradle (빌드 도구)

| 항목 | Maven | Gradle |
|------|-------|--------|
| 스크립트 형식 | XML (`pom.xml`) | Groovy/Kotlin DSL (`build.gradle`) |
| 설정 방식 | 명시적, 표준화 | 유연하고 간결 |
| 빌드 속도 | 다소 느림 | 빠름 (캐시, 병렬 빌드) |
| 사용성 | 진입장벽 낮음 | 고급 기능 활용에 적합 |
| Spring Initializr 지원 | ✅ | ✅ |
| 기업 실무 활용도 | 매우 높음 | 점점 증가 추세 |

> 🔹 **Maven 추천**: 학습용, 표준 구조, 팀 개발  
> 🔹 **Gradle 추천**: 속도 중요, 커스터마이징 필요, Kotlin 활용

- Maven(pom.xml)
```xml
    <dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    </dependencies>
```

- Gradle(build.gradle)
```gradle
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-web'
    }
```

## 8. 요약

> Spring Boot는 **개발 생산성을 극대화**하고, **유연하면서도 강력한 생태계**를 제공하는 프레임워크입니다.  
> 다양한 기술 스택과 조합되어 빠른 웹 서비스 개발 및 운영을 가능하게 합니다.  
> **Maven/Gradle과 함께 구성**, 내장 톰캣 + REST API 기반으로 **MSA/클라우드/실무 환경에 매우 적합**합니다.


---


# ☕ Spring Boot: JAR vs WAR 차이점

Spring Boot에서는 기본적으로 **JAR** 형식의 배포를 권장하지만, **WAR** 형식도 사용할 수 있습니다. 아래는 JAR과 WAR의 주요 차이점을 비교한 내용입니다.

---

## 📌 JAR vs WAR 비교

| 항목             | JAR (Java ARchive)                         | WAR (Web Application Archive)                       |
|------------------|--------------------------------------------|------------------------------------------------------|
| **실행 방식**     | ✅ 독립 실행 가능 (`java -jar`)              | ❌ 외부 WAS (Tomcat, Jetty 등)에 배포해야 함          |
| **내장 서버**     | 포함 (내장 Tomcat, Jetty, Undertow 중 택1)   | 없음 (서버는 외부 제공 필요)                        |
| **Spring Boot 권장** | ✅ 기본 권장 배포 형식                        | 제한된 환경에서만 사용 (예: 기존 WAS에 배포)         |
| **파일 구조**     | 단일 파일 (`*.jar`)                         | 웹 앱 구조 (`WEB-INF`, `web.xml`, `*.war`) 필요      |
| **배포 대상**     | 클라우드, AWS, 로컬, Docker 등              | 전통적 WAS (WebLogic, JBoss, Tomcat 등)             |
| **개발 생산성**   | 높음 (설정 간단, 바로 실행 가능)            | 낮음 (복잡한 설정, 외부 WAS 필요)                   |

---

## ✅ JAR 방식의 장점

- 내장 Tomcat 포함 → `java -jar`로 바로 실행 가능
- 서버 환경에 관계없이 유연한 배포 가능
- Docker, 클라우드, 로컬 실행에 최적화
- DevOps, CI/CD에 잘 어울림

---

## ❓ WAR 방식은 언제 사용하나요?

- 기존 **WAS 기반 시스템** 유지가 필요한 경우 (예: 회사의 Tomcat 서버에 배포)
- 기존 **Java EE 프로젝트와 통합**해야 하는 경우
- 회사 정책상 WAR만 배포 허용되는 경우

---

## ⚙ WAR 파일로 빌드하는 방법 (Spring Boot)

### 1. `SpringBootServletInitializer` 상속

```java
@SpringBootApplication
public class MyApplication extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(MyApplication.class);
    }
}
```

### 2. `pom.xml` 설정

```xml
<packaging>war</packaging>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <scope>provided</scope> <!-- 내장 Tomcat 제거 -->
    </dependency>
</dependencies>
```

### 3. 실행 방법

- Tomcat 등의 WAS에 `.war` 파일을 복사하여 배포

---

## 🚀 JAR 실행 예시 (CMD)

```bash
java -jar myapp-0.0.1-SNAPSHOT.jar
```

포트 지정:

```bash
java -jar myapp.jar --server.port=8081
```

---

## 📝 요약

- **Spring Boot에서는 JAR 방식이 기본이며 더 간단하고 현대적인 배포 방식**입니다.
- **WAR 방식은 전통적인 WAS에 배포가 필요할 때 사용**합니다.



---
