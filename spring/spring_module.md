# Spring Framework 주요 모듈 정리

Spring Framework는 Java 기반의 엔터프라이즈 애플리케이션 개발을 위한 강력한 프레임워크입니다. 모듈화된 구조로 이루어져 있으며, 각 모듈이 특정 기능을 담당합니다. 아래에서 각 모듈의 역할과 특징을 자세히 설명하겠습니다.

---

## 1. Spring Core

- **설명:** Spring 프레임워크의 핵심 모듈로, 다른 모든 모듈이 이 모듈을 기반으로 동작합니다.
- **주요 기능:**
  - **Bean Container(빈 컨테이너) 제공** → Spring은 빈(bean)이라는 개념을 사용하여 객체를 생성, 관리합니다. 빈 컨테이너는 객체의 생명 주기를 관리하며, 의존성 주입(Dependency Injection, DI)을 수행합니다.
  - **제어 반전(Inversion of Control, IoC)** → 애플리케이션의 흐름을 개발자가 직접 제어하는 것이 아니라, Spring 컨테이너가 객체의 생성을 관리합니다.
  - **공통 유틸리티 제공** → 문자열 처리, 리소스 관리, 데이터 변환 등의 기능을 제공합니다.

---

## 2. Spring AOP (Aspect-Oriented Programming)

- **설명:** AOP(관점 지향 프로그래밍)를 지원하는 모듈로, 핵심 로직과 부가 기능(예: 로깅, 보안, 트랜잭션 관리 등)을 분리할 수 있도록 도와줍니다.
- **주요 기능:**
  - **소스 코드 수준에서 메타데이터 추가** → 특정 메서드나 클래스에 어노테이션을 사용하여 부가 기능을 적용할 수 있습니다.
  - **AOP 인프라 제공** → 개발자가 직접 핵심 로직을 수정하지 않고도 로깅, 보안 검증 등의 기능을 적용할 수 있도록 지원합니다.

---

## 3. Spring ORM (Object-Relational Mapping)

- **설명:** Spring에서 ORM 기술을 지원하는 모듈로, Java 객체와 데이터베이스 간의 매핑을 쉽게 할 수 있도록 도와줍니다.
- **주요 기능:**
  - **JPA(Java Persistence API) 지원**
  - **Hibernate 지원**
  - **MyBatis, iBatis 지원**
  - **JDBC 연동 가능**

---

## 4. Spring DAO (Data Access Object)

- **설명:** 데이터베이스와의 연결을 쉽게 만들어 주는 모듈로, JDBC와 트랜잭션을 처리할 수 있도록 도와줍니다.
- **주요 기능:**
  - **트랜잭션 인프라 제공**
  - **JDBC 지원**
  - **DAO(데이터 접근 객체) 패턴 지원**

---

## 5. Spring Context

- **설명:** Spring의 **ApplicationContext**를 제공하며, Bean을 중심으로 애플리케이션을 관리하는 모듈입니다.
- **주요 기능:**
  - **ApplicationContext 제공** → Bean을 생성하고, 의존성을 주입하며, 라이프사이클을 관리합니다.
  - **Bean Factory 지원** → XML, Java Config, Annotation 등을 사용하여 빈을 정의하고 설정할 수 있습니다.
  - **빈 라이프사이클 관리** → Bean의 생성, 초기화, 소멸 과정 등을 관리합니다.
  - **환경 설정 및 프로퍼티 관리** → `@Value` 어노테이션 또는 `PropertySources`를 이용하여 애플리케이션 설정을 관리할 수 있습니다.
  
---

## 6. Spring Web

- **설명:** Spring을 웹 애플리케이션에서 사용할 수 있도록 지원하는 모듈입니다.
- **주요 기능:**
  - **웹 애플리케이션 컨텍스트 제공**
  - **멀티파트 요청 처리 (파일 업로드 지원)**
  - **웹 유틸리티 제공**

---

## 7. Spring Web MVC

- **설명:** Spring의 웹 애플리케이션 개발을 위한 MVC(Model-View-Controller) 프레임워크입니다.
- **주요 기능:**
  - **Spring MVC 프레임워크 제공**
  - **다양한 뷰 기술 지원 (JSP, Thymeleaf, Velocity 등)**
  - **PDF 및 Excel 출력 기능 제공**

---

## 8. Maven과의 관계

- **Maven은 ORM 프레임워크가 아닌 빌드 및 의존성 관리 도구**이며, ORM과 관련된 라이브러리(Hibernate, JPA, MyBatis 등)를 쉽게 관리할 수 있습니다.

---

