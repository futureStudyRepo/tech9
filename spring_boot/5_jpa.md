# 조합:  Spring Boot + JPA + Thymeleaf + H2
## JPA 구현 

## ✅ 1. **`jpa` 프로젝트**  
> **조합:** Spring Boot + JPA + Thymeleaf + H2

---
# JPA와 ORM

## JPA (Java Persistence API)
JPA는 자바 애플리케이션에서 데이터베이스와 상호작용하기 위해 사용하는 ORM 표준 인터페이스입니다. JPA는 인터페이스이기 때문에, 이를 구현하는 구체적인 구현체가 필요합니다. 대표적인 JPA 구현체로는 **Hibernate**가 있습니다.

### ORM (Object-Relational Mapping)
ORM은 객체 지향 프로그래밍 언어에서 사용하는 객체와 관계형 데이터베이스의 테이블을 매핑하는 기법을 말합니다. 즉, 객체와 테이블 간의 불일치를 해결하고, 자바 코드에서 SQL을 작성하지 않고도 데이터베이스와 상호작용할 수 있게 합니다.

## JPA와 ORM을 사용하는 이유
1. **생산성 향상**: SQL을 직접 작성할 필요 없이, 객체 지향적으로 데이터베이스 작업이 가능합니다.
2. **유지 보수성 향상**: 객체 모델을 사용하기 때문에, 엔티티 구조를 통해 코드 가독성과 유지보수성이 높아집니다.
3. **데이터베이스 독립성**: SQL에 종속되지 않아 데이터베이스에 의존하지 않는 코드를 작성할 수 있습니다.
4. **지연 로딩**: 필요한 시점에 데이터를 로딩하여, 불필요한 데이터 조회를 최소화할 수 있습니다.

## JPA의 장단점

### 장점
- **생산성**: SQL 문 작성 없이 엔티티 객체를 통해 데이터베이스를 조작할 수 있습니다.
- **유지보수성**: 엔티티 객체 구조로 인해, 데이터베이스 변경이 발생해도 코드 수정이 최소화됩니다.
- **효율적인 쿼리 생성**: 필요한 경우 JPA에서 제공하는 JPQL을 통해 효율적인 쿼리 작성이 가능합니다.

### 단점
- **복잡한 쿼리 작성의 한계**: 단순 CRUD 이상의 복잡한 쿼리는 JPA만으로 처리하기 어렵고, 네이티브 SQL이 필요할 수 있습니다.
- **퍼포먼스 이슈**: 잘못된 설정이나 사용 방식으로 인해 성능 저하가 발생할 수 있습니다.
- **학습 비용**: ORM과 JPA 사용 방법을 이해하는 데 시간이 걸릴 수 있습니다.

# Hibernate

## Hibernate란?
Hibernate는 **ORM (Object-Relational Mapping)** 프레임워크로, Java 객체와 관계형 데이터베이스를 매핑하여 SQL 문 없이 데이터베이스와 상호작용할 수 있게 해줍니다. Hibernate는 JPA(Java Persistence API)의 구현체 중 하나로, **엔티티 객체**를 데이터베이스 테이블과 매핑하여 개발자가 객체 지향적으로 데이터를 처리할 수 있도록 지원합니다.

## Hibernate의 주요 기능
1. **자동 SQL 생성**: 복잡한 SQL 쿼리를 작성하지 않고도 데이터를 저장, 조회할 수 있습니다.
2. **캐싱**: Hibernate는 1차 캐시와 2차 캐시를 통해 성능을 최적화합니다.
3. **지연 로딩(Lazy Loading)**: 필요한 시점에만 데이터를 로딩하여, 불필요한 데이터 조회를 줄입니다.
4. **트랜잭션 관리**: 데이터 일관성을 위해 트랜잭션을 지원하며, 쉽게 설정할 수 있습니다.
5. **자동 매핑**: Java 객체와 데이터베이스 테이블 간의 자동 매핑을 제공합니다.

## Hibernate의 장점
- **생산성 향상**: 데이터베이스의 CRUD 작업을 쉽게 처리할 수 있으며, SQL 작성이 필요하지 않아 개발 속도가 향상됩니다.
- **데이터베이스 독립성**: 특정 데이터베이스에 종속되지 않으며, 다양한 데이터베이스를 지원합니다.
- **유지 보수성**: 객체 모델을 통해 데이터베이스와 상호작용하므로, 데이터베이스 구조가 변경되어도 코드 변경이 최소화됩니다.
- **지연 로딩**: 필요 시에만 데이터를 로드하므로 성능 최적화에 유리합니다.

## Hibernate의 단점
- **복잡한 쿼리 제한**: 단순 CRUD 이외의 복잡한 쿼리 작성은 어려울 수 있으며, 경우에 따라 네이티브 SQL을 사용해야 합니다.
- **퍼포먼스 이슈**: 잘못된 매핑 설정이나 지연 로딩의 남용으로 성능 저하가 발생할 수 있습니다.
- **학습 곡선**: 초기 설정과 매핑 작업이 복잡할 수 있으며, 개념을 이해하는 데 학습 시간이 필요합니다.

---

# Gradle

## Gradle이란?
Gradle은 Java, Kotlin, Groovy, C++, Python 등 다양한 언어의 프로젝트를 자동화하고 빌드하기 위한 빌드 자동화 도구입니다. Apache Maven과 Apache Ant의 장점을 결합하여 유연성과 확장성이 뛰어난 빌드 시스템을 제공합니다. Gradle은 Groovy 또는 Kotlin DSL(Domain-Specific Language)을 사용하여 빌드 스크립트를 작성합니다.

## Gradle 사용 이유
1. **유연한 빌드 설정**: Groovy와 Kotlin을 지원하는 DSL을 통해 사용자 정의가 용이합니다.
2. **고성능 빌드**: Gradle은 빌드 성능을 최적화하여 빠르게 빌드를 수행합니다. 예를 들어, 증분 빌드와 캐시 기능을 제공하여 변경된 부분만 빌드합니다.
3. **다양한 언어 지원**: 여러 언어를 지원하여 멀티플랫폼 프로젝트를 쉽게 빌드할 수 있습니다.
4. **확장성**: 플러그인 시스템을 통해 필요에 맞게 확장할 수 있습니다. Spring Boot, Android 등 다양한 생태계에서 사용됩니다.

## Gradle의 장단점

### 장점
- **빠른 빌드 속도**: 증분 빌드, 빌드 캐시 등 고급 기능으로 빌드 성능을 높입니다.
- **유연성**: Groovy와 Kotlin 기반의 DSL을 통해 복잡한 빌드 논리를 유연하게 처리할 수 있습니다.
- **확장성**: Gradle 플러그인을 통해 쉽게 기능을 확장할 수 있으며, 사용자 정의 플러그인을 작성할 수도 있습니다.
- **좋은 멀티 프로젝트 지원**: 대규모 프로젝트에서 서브 프로젝트 간의 의존성을 효율적으로 관리할 수 있습니다.

### 단점
- **복잡한 학습 곡선**: Maven보다 유연하지만, 초기 설정이 복잡하고, DSL 학습에 시간이 필요할 수 있습니다.
- **의존성 문제**: 여러 라이브러리 간 의존성 충돌 문제가 발생할 수 있으며, 이를 해결하기 위해 의존성 버전을 세밀하게 관리해야 합니다.
- **디버깅 어려움**: DSL로 작성된 빌드 스크립트의 디버깅이 어려울 수 있습니다.

- `gradle build`: 프로젝트를 빌드합니다.
- `gradle clean`: 빌드 디렉토리를 정리합니다.
- `gradle test`: 테스트를 실행합니다.
- `gradle run`: 애플리케이션을 실행합니다 (플러그인 추가 시 가능).

---


## ✅ 📄 예제 `build.gradle` 구성 요약

### 📌 주요 기술 스택
| 기술 | 설명 |
|------|------|
| **Spring Boot 3.3.5** | 최신 Spring Boot 버전 |
| **Java 17** | `toolchain`을 사용한 명시적 JDK 설정 |
| **JPA** | 데이터베이스 ORM 매핑 |
| **Thymeleaf** | 템플릿 기반 뷰 렌더링 |
| **Lombok** | 코드 간소화 (Getter/Setter 등 자동 생성) |
| **Devtools** | 개발 시 자동 리로딩 |
| **H2** | 메모리 DB (테스트/개발용) |
---

## 🧠 상세 설명

### 🔧 `plugins`

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.5'
    id 'io.spring.dependency-management' version '1.1.6'
}
```
- Gradle에서 Java + Spring Boot를 사용할 수 있도록 설정
- `dependency-management`는 Spring Boot BOM 버전 관리

---

### 🧪 Java Toolchain

```groovy
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}
```
> 시스템에 여러 JDK가 있어도 **Java 17 환경을 고정**합니다. 실무에서 JDK 버전 일관성 유지할 때 매우 유용합니다.

---

### 📦 Dependencies

```groovy
dependencies {
    implementation 'spring-boot-starter-data-jpa'  // JPA
    implementation 'spring-boot-starter-thymeleaf' // 뷰 템플릿
    implementation 'spring-boot-starter-web'       // 웹 MVC

    compileOnly 'lombok'
    annotationProcessor 'lombok'

    developmentOnly 'spring-boot-devtools'
    runtimeOnly 'com.h2database:h2'

    testImplementation 'spring-boot-starter-test'
    testRuntimeOnly 'junit-platform-launcher'
}
```

- `compileOnly` + `annotationProcessor` → Lombok 전용 설정
- `developmentOnly` → Devtools는 빌드 결과물에 포함되지 않음
- `runtimeOnly` → 실행 시에만 필요한 의존성 (예: DB 드라이버)
- `testImplementation`, `testRuntimeOnly` → 테스트 전용 설정

---

```properties

spring.application.name=jpa
server.port=8080

#hibernate 는 ORM프레임워크, Java객체와 관계형 데이터베이스를 
#매핑하여 SQL없이 데이터베이스와 상호작용할 수 있게 해줌.
#hibernate는 JPA(java persistence API)의 구현체
spring.jpa.defer-datasource-initialization=true
spring.jpa.properties.hibernate.globally_quoted_identifiers=true
spring.jpa.hibernate.ddl-auto=update

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE

spring.h2.console.enabled=true
```

## ✅ 프로젝트 구성 개요

| 영역 | 사용 기술 / 파일 |
|------|------------------|
| 백엔드 | Spring Boot 3.3.5, Spring Data JPA |
| 데이터베이스 | H2 (in-memory DB) |
| 프론트엔드 | Thymeleaf (login.html, register.html, home.html) |
| ORM 매핑 | `User` Entity + `IUserRepository` |
| 로그인 처리 | `HomeController`, `UserService` |
| 설정 파일 | `application.properties`, `data.sql` |

---
## 🔍 핵심 구성 분석

### 1. 📄 `User.java` (Entity)
- `@Entity`, `@Table(name="user")`으로 DB 테이블 `user`와 매핑
- 필드: `id`, `username`, `password`
- `@GeneratedValue(strategy=IDENTITY)` → 자동 증가
- `Lombok`의 `@Getter`, `@Setter`로 코드 간결화

### 2. 📄 `IUserRepository.java`
- `JpaRepository<User, Long>` 상속
- `findByUsername(String)` 메서드로 사용자 조회

### 3. 📄 `UserService.java`
- `findByUsername()` → 로그인용 조회
- `save()` → 회원가입 저장

### 4. 📄 `HomeController.java`
- `/login`, `/register`, `/home` 라우팅 처리
- 로그인 성공 시 `redirect:/home`, 실패 시 에러 메시지 출력
- 회원가입 시 `POST /register`에서 사용자 저장 후 로그인 페이지로 이동

### 5. 📝 `application.properties`
- DB 설정: H2 Console, SQL 로그 출력, Hibernate 설정 포함
- `ddl-auto=update` → 앱 시작 시 테이블 자동 생성/수정

### 6. 🌐 HTML 템플릿
- `login.html`, `register.html`, `home.html` 모두 `Thymeleaf` 기반
- 간단한 form 입력 및 제출 기능 제공

---

## 🔐 흐름 요약 (로그인/회원가입)

1. 사용자가 `/`로 접속하면 로그인 폼 페이지 (`login.html`) 노출
2. 아이디/비밀번호 입력 후 `POST /login` 요청 → `UserService.findByUsername()` 사용
3. 로그인 성공 시 `/home` 페이지로 이동, 실패 시 에러 메시지 출력
4. [Register] 클릭 시 `/register` 폼으로 이동
5. 폼 입력 후 제출 시 `UserService.save()` 통해 DB에 저장됨

---

## ✨ 정리 요약

| 구성 요소 | 내용 |
|-----------|------|
| 웹 | Spring MVC (`spring-boot-starter-web`) |
| 데이터 | JPA + H2 (메모리 DB) |
| 템플릿 | Thymeleaf |
| 개발 보조 | Lombok, Devtools |
| 테스트 | JUnit 5 (Spring Test 포함) |
| JDK 설정 | `Java 17` Toolchain 지정 |