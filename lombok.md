
---

# 📘 Lombok이란?

**Lombok**은 자바에서 반복적으로 작성해야 하는 **보일러플레이트 코드(boilerplate code)** — 예: getter/setter, toString, equals, hashCode, 생성자 등을 **자동으로 생성해주는 라이브러리**입니다.

👉 즉, 코드를 깔끔하고 유지보수하기 쉽게 해주는 도구입니다.

---

## 💡 주요 기능 요약

| 어노테이션 | 설명 |
|------------|------|
| `@Getter`, `@Setter` | getter/setter 메서드 자동 생성 |
| `@ToString` | toString() 메서드 자동 생성 |
| `@EqualsAndHashCode` | equals(), hashCode() 자동 생성 |
| `@NoArgsConstructor` | 기본 생성자 생성 |
| `@AllArgsConstructor` | 모든 필드를 받는 생성자 생성 |
| `@RequiredArgsConstructor` | final 또는 @NonNull 필드만 받는 생성자 생성 |
| `@Data` | 위 5개 기능(@Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor)을 한 번에 적용 |
| `@Builder` | 빌더 패턴 생성자 자동 생성 |
| `@Slf4j` | 로그를 쉽게 쓰도록 `log.info()` 같은 로그 객체 자동 생성 |

---

## 🛠️ Spring에서 Lombok 사용하는 방법

### 1. **의존성 추가**

#### ✅ Maven (pom.xml)
```xml
<dependencies>
    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.30</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

#### ✅ Gradle (build.gradle)
```groovy
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.30'
    annotationProcessor 'org.projectlombok:lombok:1.18.30'
}
```

> ⚠️ `IntelliJ` 사용 시 Lombok Plugin 설치 후 `Enable annotation processing` 설정도 해줘야 합니다.

---

### 2. **예제 코드 (Spring 프로젝트에서 DTO, Entity, Service에 적용)**

#### 📦 DTO 클래스
```java
import lombok.Data;

@Data
public class MemberDTO {
    private Long id;
    private String name;
    private String email;
}
```

> `@Data`를 쓰면 `getter/setter`, `toString`, `equals`, `hashCode`, `RequiredArgsConstructor`가 전부 자동 생성됩니다.

---

#### 📦 Entity 클래스
```java
import lombok.*;

import javax.persistence.*;

@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor
@Builder
public class Member {

    @Id @GeneratedValue
    private Long id;

    private String name;

    private String email;
}
```

- `@Getter`: 모든 필드에 getter 생성
- `@NoArgsConstructor`: JPA용 기본 생성자
- `@AllArgsConstructor`, `@Builder`: 생성자 및 빌더 패턴 지원

---

## ✅ Lombok 사용 시 주의사항

| 주의점 | 설명 |
|--------|------|
| 플러그인 필수 | IntelliJ나 Eclipse에서 Lombok 플러그인 설치 필요 |
| annotation processing | IDE에서 annotation processing 옵션 활성화 필요 |
| 빌드 시 반영 | 코드는 보이지 않아도 컴파일 시 getter/setter가 자동으로 생성됨 |
| @Builder는 상속 불가 | @Builder는 상속 구조에 적용할 때 제한이 있음 (BuilderInheritance는 직접 구현 필요)

---

## 📌 결론

- Lombok은 코드 작성 시간을 단축시키고 가독성을 높여줍니다.

---
