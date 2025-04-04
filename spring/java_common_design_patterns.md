JAVA에서 DAO, DTO, Service, VO는 주로 **Spring**과 같은 웹 애플리케이션 개발에서 많이 사용되는 개념으로, 각각 명확한 역할과 목적이 있습니다. 각 용어를 간단히 정의하고 역할과 예시를 통해 설명하겠습니다.

---

### ✅ **1. DAO (Data Access Object)**

**정의:**
- 데이터베이스 접근 로직을 분리하여 캡슐화한 객체입니다.
- CRUD(생성, 읽기, 수정, 삭제)와 같은 데이터 접근 메서드를 제공합니다.

**역할:**
- DB에 직접 접근하여 SQL문 실행 또는 ORM(예: MyBatis, JPA)을 이용하여 데이터를 가져오거나 수정합니다.
- 데이터 접근을 담당하며, 서비스 계층과 데이터 저장소(DB) 사이의 연결을 담당합니다.

**예시:**
```java
public interface UserDAO {
    User findUserById(int id);
    void insertUser(User user);
}
```

---

### ✅ **2. DTO (Data Transfer Object)**

**정의:**
- 계층 간 데이터 전송을 위한 객체입니다.
- 보통 getter/setter만 가지고 있으며 로직을 포함하지 않습니다.

**역할:**
- 계층 간 데이터 전달 시 사용되며, 주로 Service와 Controller 계층 간 데이터 전달 시 사용합니다.
- 필요한 정보만 담아 네트워크 상에서 효율적인 데이터 전송이 가능합니다.

**예시:**
```java
public class UserDTO {
    private int id;
    private String name;

    // getter, setter 생략
}
```

---

### ✅ **3. Service (서비스)**

**정의:**
- 비즈니스 로직을 처리하는 계층입니다.
- DAO를 통해 얻은 데이터를 가공하여 원하는 결과로 만들어 제공합니다.

**역할:**
- DAO를 호출하여 데이터를 조회하거나 저장합니다.
- 비즈니스 규칙 적용, 트랜잭션 관리, 예외 처리와 같은 핵심 로직을 처리합니다.
- 서비스 클래스는 종종 `@Service` 어노테이션으로 표시됩니다.

**예시:**
```java
@Service
public class UserService {

    @Autowired
    private UserDAO userDAO;

    public UserDTO getUserProfile(int id) {
        User user = userDAO.findUserById(id);
        UserDTO dto = new UserDTO();
        dto.setId(user.getId());
        dto.setName(user.getName());
        return dto;
    }
}
```

---

### ✅ **4. VO (Value Object)**

**정의:**
- 불변성을 가진 값 객체입니다.
- 동일한 값을 가진 객체는 동일 객체로 간주하며, 값 자체에 의미가 있습니다.

**역할:**
- 주로 읽기 전용으로 쓰이며, 객체의 값이 같으면 동등한 객체로 취급합니다.
- 상태 변경이 일어나지 않아 안전하게 데이터를 다룰 수 있습니다.
- DTO는 계층 간 전달용이며 VO는 값 자체의 의미를 가진 객체로, 사용 목적에서 차이가 있습니다.

**예시:**
```java
public class Money {
    private final int amount;
    private final String currency;

    public Money(int amount, String currency){
        this.amount = amount;
        this.currency = currency;
    }

    public int getAmount(){ return amount; }
    public String getCurrency(){ return currency; }

    // equals, hashCode, toString 메서드 오버라이드 필수
}
```

---

## 📌 정리 및 비교표

| 구분 | 용도 및 특징 | 불변성 | 로직 포함 여부 | 주 사용 위치 |
|------|-------------|--------|-------------|------------|
| DAO | 데이터베이스 접근 처리 | X | O | Persistence Layer |
| DTO | 계층 간 데이터 전달 | X | X | 계층 간 통신 (주로 Controller ↔ Service) |
| Service | 비즈니스 로직 처리 | X | O | 비즈니스 로직 계층 |
| VO | 값 그 자체 표현, 불변 객체 | O | X | 도메인 모델, 불변 데이터 표현 |

---

실제 개발 현장에서는 DB에서 가져온 데이터를 담는 객체를 표현할 때, 주로 **VO(Value Object)**라는 용어를 더 많이 사용할까?  
---

## 📌 1. 한국 개발 현장에서의 관례적 사용

- 한국의 많은 개발 현장에서는 DB에서 조회한 데이터를 저장하기 위한 단순 객체를 흔히 **VO**로 부릅니다.
- 사실상 원칙적으로 보면 **DTO**(Data Transfer Object)라는 표현이 더 적합한 경우가 많지만, 한국에서는 전통적으로 **VO라는 용어를 일반적으로 써왔기 때문**입니다.
- 특히 MyBatis와 같은 프레임워크에서 DB 데이터를 담는 객체를 VO로 표현하는 예시가 많습니다.

---

## 📌 2. DTO vs VO 개념의 혼동

원래 의미는 다음과 같습니다.

| 구분 | DTO | VO |
|------|-----|-----|
| 목적 | 계층 간 데이터 전달 객체 | 값 자체의 의미를 가지는 불변 객체 |
| 특징 | setter/getter O (가변적) | setter X, 불변적(immutable) |
| 사용위치 | Controller-Service 간 전달 | 도메인(비즈니스 로직 내), 불변 데이터 표현 |

- 실제로는 DB에서 데이터를 담을 때 **getter/setter를 모두 가지는 형태**로 구현하지만, **개발자들이 DTO와 VO의 개념 차이를 명확히 하지 않은 상태**에서 편하게 **VO라는 명칭을 사용**하는 경우가 많습니다.

---

## 📌 3. VO가 편리하게 느껴지는 이유

- **짧고 직관적인 용어**  
  - VO(Value Object)가 DTO(Data Transfer Object)에 비해 이름이 짧고 편리하게 쓰입니다.

- **명칭 구분을 명확히 하지 않아도 개발에 지장 없기 때문**  
  - 많은 개발자들이 VO라는 명칭을 써도 프로젝트 진행에 큰 문제가 없다고 생각합니다.
  - DTO와 VO가 혼용되는 경우가 많아져 결국 개발자 사이의 암묵적 합의로 VO가 더 많이 쓰이게 됩니다.

---

## 📌 4. MyBatis 프레임워크의 영향

- MyBatis(구 iBatis)에서는 **SQL 결과를 담는 객체를 VO로 자주 표현**합니다.
- 국내 프로젝트에서는 MyBatis가 널리 쓰이며, 이 프레임워크를 통해 DB 데이터를 다룰 때 VO라는 표현을 자연스럽게 따라 사용하게 됩니다.

예시:
```java
public class UserVO {
    private int id;
    private String name;

    // getter/setter 존재
}
```

실제로는 위 코드처럼 **VO가 아닌 DTO에 가깝지만**, 현장에선 보통 VO로 명칭을 붙입니다.

---

## 📌 정리 (실무 팁✨)

| 실무상 표현 | 실제 의미에 가까운 표현 | 비고 |
|------------|---------------------|------|
| DAO        | DAO                 | 명확히 쓰임 |
| VO         | DTO                 | 혼용됨 |
| Service    | Service             | 명확히 쓰임 |
| DTO        | DTO                 | 계층 간 명확히 구분 시 |

- **엄밀히** 표현하면, DB 데이터를 담아 계층 간 전달할 때는 **DTO**가 맞습니다.
- **VO**라는 용어는 값 자체가 의미를 가지며 불변적인 객체일 때 쓰는 것이 정확합니다.
- 그러나 현실적인 이유로 국내에서는 **VO라는 표현을 관례적으로 자주 사용**합니다.

이러한 이유로 DB 관련 작업을 할 때 VO라는 표현을 더 많이 사용하게 됩니다.