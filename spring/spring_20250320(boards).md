# 📘 [tech9-스프링] 수업 내용 정리

---

## 수업 개요
**날짜:** 2025년 03월 20일 오후 
**주제:** [spring framework 회원게시판]  
postMapping, Model, c:if, c:forEach

---

## 주요 개념

```markdown
### `@PostMapping`
`@PostMapping`은 Spring MVC에서 HTTP POST 요청을 처리하는 메서드에 사용하는 어노테이션입니다. 주로 데이터를 생성하거나 서버로 전송하는 작업에 사용됩니다.

```java
@PostMapping("/example")
public String handlePostRequest(@RequestBody ExampleData data) {
    // POST 요청 처리 로직
    return "response";
}
```

#### 주요 특징:
- HTTP POST 요청과 매핑됨.
- 주로 클라이언트에서 데이터를 서버로 보낼 때 사용됨.

---

### `Model`
`Model`은 Spring MVC에서 데이터를 뷰에 전달하는 데 사용되는 객체입니다. 주로 컨트롤러에서 뷰로 데이터를 전달할 때 사용됩니다.

```java
@GetMapping("/example")
public String examplePage(Model model) {
    model.addAttribute("key", "value");
    return "viewName";
}
```

#### 주요 특징:
- 데이터를 키-값 쌍으로 뷰에 전달.
- 뷰에서 데이터를 표현할 수 있도록 지원.

---

### `<c:if>`
JSP에서 사용되는 태그 라이브러리(JSTL)로, 조건에 따라 내용을 출력할 때 사용됩니다. `test` 속성의 값에 따라 내용을 렌더링합니다.

```jsp
<c:if test="${condition}">
    <p>조건이 참일 때만 이 내용이 출력됩니다.</p>
</c:if>
```

#### 주요 특징:
- 조건부 렌더링 지원.
- `test`가 true일 때만 내부 내용이 출력됨.

---

### `<c:forEach>`
JSTL에서 반복 처리를 할 때 사용하는 태그로, 컬렉션이나 배열과 같은 요소를 순차적으로 처리할 때 사용됩니다.

```jsp
<c:forEach var="item" items="${list}">
    <p>${item}</p>
</c:forEach>
```

#### 주요 특징:
- 컬렉션, 배열을 반복 처리.
- `var` 속성에 반복 중인 항목을 지정.

---


### **`@ExceptionHandler` 어노테이션 설명**

#### **1. 개요**
`@ExceptionHandler`는 **Spring MVC에서 예외를 처리하기 위한 어노테이션**입니다.  
컨트롤러에서 발생한 특정 예외를 감지하고, 지정한 메서드에서 예외를 처리할 수 있도록 합니다.

---

#### **2. 기본 사용법**
```java
@Controller
public class SampleController {

    @GetMapping("/example")
    public String example() {
        if (true) { // 예외 발생 상황 가정
            throw new IllegalArgumentException("잘못된 요청입니다.");
        }
        return "success";
    }

    // 예외 처리 메서드
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException e) {
        return new ResponseEntity<>("예외 발생: " + e.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```
#### **3. 동작 방식**
1. `/example` 요청 시 `IllegalArgumentException` 발생.
2. `@ExceptionHandler(IllegalArgumentException.class)`가 해당 예외를 감지.
3. `handleIllegalArgumentException()` 메서드가 실행되며 예외 메시지와 함께 HTTP 400 응답 반환.

---

#### **4. 다양한 예외 처리 방법**
##### **1) 다중 예외 처리**
한 개의 `@ExceptionHandler` 메서드에서 여러 개의 예외를 처리할 수 있습니다.
```java
@ExceptionHandler({IllegalArgumentException.class, NullPointerException.class})
public ResponseEntity<String> handleMultipleExceptions(RuntimeException e) {
    return new ResponseEntity<>("예외 발생: " + e.getMessage(), HttpStatus.BAD_REQUEST);
}
```

##### **2) 전역 예외 처리 (`@ControllerAdvice`)**
- 개별 컨트롤러가 아닌, **모든 컨트롤러의 예외를 전역적으로 처리**하고 싶을 때 사용합니다.
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGlobalException(Exception e) {
        return new ResponseEntity<>("서버 오류 발생: " + e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

##### **3) 사용자 정의 예외 처리**
- `@ExceptionHandler`를 활용하여 **사용자 정의 예외**도 처리할 수 있습니다.

```java
public class CustomException extends RuntimeException {
    public CustomException(String message) {
        super(message);
    }
}

@Controller
public class CustomController {

    @GetMapping("/custom")
    public String customError() {
        throw new CustomException("사용자 정의 예외 발생!");
    }

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<String> handleCustomException(CustomException e) {
        return new ResponseEntity<>("예외 처리됨: " + e.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

---

#### **5. 주요 특징**
✅ 특정 컨트롤러에서 발생한 예외만 처리 가능 (`@ControllerAdvice` 사용 시 전역 적용)  
✅ 여러 예외를 한 번에 처리 가능  
✅ `ResponseEntity<>` 등을 활용하여 HTTP 응답 제어 가능  
✅ JSON 응답을 반환하는 `@RestController`에서도 활용 가능  

---

#### **6. `@ExceptionHandler` vs `@ControllerAdvice` 차이**
| 어노테이션 | 적용 범위 | 특징 |
|-----------|---------|------|
| `@ExceptionHandler` | 개별 컨트롤러 | 해당 컨트롤러에서 발생하는 특정 예외만 처리 |
| `@ControllerAdvice` | 전역 (모든 컨트롤러) | 모든 컨트롤러에서 발생하는 예외를 한 곳에서 관리 |

👉 **`@ControllerAdvice`는 전역 예외 처리, `@ExceptionHandler`는 특정 컨트롤러에서 예외 처리에 적합**  

---

### **결론**
- `@ExceptionHandler`는 특정 컨트롤러에서 예외를 개별적으로 처리하는 데 사용.
- `@ControllerAdvice`와 함께 사용하면 전역적인 예외 관리 가능.
- `ResponseEntity<>`를 활용하면 JSON 응답으로 반환 가능.

✅ **컨트롤러에서 예외가 발생하면, `@ExceptionHandler`가 예외를 잡아 적절한 응답을 반환할 수 있도록 도와줍니다!** 🚀




## 실습 예제 


- 관련 계정 생성 및 테이블 생성

```sql
 -- 사용자 게시판 
CREATE TABLE boards (
    board_no NUMBER GENERATED ALWAYS AS IDENTITY (START WITH 1 INCREMENT BY 1 NOCACHE), -- 1씩 증가 (NOCACHE 설정)
    board_title VARCHAR2(1000),
    mem_id VARCHAR2(100) NOT NULL,
    board_content VARCHAR2(2000),
    create_dt DATE DEFAULT SYSDATE,
    update_dt DATE DEFAULT SYSDATE,
    use_yn VARCHAR2(1) DEFAULT 'Y',
    PRIMARY KEY (board_no), -- PRIMARY KEY 지정
    CONSTRAINT fk_member FOREIGN KEY(mem_id) REFERENCES members(mem_id) -- FOREIGN KEY 직접 적용
);


-- 댓글 테이블
CREATE TABLE replys (
    reply_no NUMBER, -- 자동 증가 없음, 직접 값 입력 필요
    board_no NUMBER(10),
    mem_id VARCHAR2(100),
    reply_content VARCHAR2(1000),
    reply_date DATE DEFAULT SYSDATE,
    del_yn VARCHAR2(1) DEFAULT 'N',
    PRIMARY KEY (board_no, reply_no),
    CONSTRAINT fk_mem_id FOREIGN KEY (mem_id) REFERENCES members(mem_id) ON DELETE CASCADE, -- 회원 삭제 시 관련 댓글도 삭제
    CONSTRAINT fk_board_no FOREIGN KEY (board_no) REFERENCES boards(board_no) ON DELETE CASCADE -- 게시글 삭제 시 관련 댓글도 삭제
);
  
```


## 참고 자료

- pdf 2장, 3장 참고하세요 

## 질문 

- 