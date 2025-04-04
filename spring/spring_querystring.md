# 쿼리 스트링(Query String)과 JSON 데이터 전송 및 Spring 바인딩

## 1. 쿼리 스트링(Query String)이란?
쿼리 스트링은 **URL에 `?`를 사용하여 데이터를 전달하는 방식**입니다. 여러 개의 데이터를 보낼 때는 `&`로 구분합니다.

### 1.1 기본 구조
```
https://example.com/search?query=apple&page=2
```
- `?` : 쿼리 스트링의 시작
- `query=apple` : `query` 변수에 `apple` 값을 전달
- `&` : 여러 개의 파라미터 구분
- `page=2` : `page` 변수에 `2` 값을 전달

---

## 2. jQuery AJAX 요청 방식

### 2.1 쿼리 스트링(Query String) 전송 (GET 방식)
```javascript
$.ajax({
    url: "/api/data",
    type: "GET",
    data: { name: "John", age: 30 },
    success: function(response) {
        console.log(response);
    },
    error: function(error) {
        console.log(error);
    }
});
```
- `data` 객체가 `name=John&age=30` 형태로 변환되어 URL에 추가됨.

### 2.2 JSON 데이터 전송 (POST 방식)
```javascript
$.ajax({
    url: "/api/data",
    type: "POST",
    contentType: "application/json",
    data: JSON.stringify({ name: "John", age: 30 }),
    success: function(response) {
        console.log(response);
    },
    error: function(error) {
        console.log(error);
    }
});
```
- `contentType: "application/json"`을 설정하여 JSON 데이터로 전송.
- `data`를 JSON 문자열로 변환하여 전달.

---

## 3. Spring Controller에서 데이터 받기

### 3.1 쿼리 스트링(Query String) 바인딩
```java
@RestController
@RequestMapping("/api")
public class DataController {

    @GetMapping("/data")
    public String getData(@RequestParam String name, @RequestParam int age) {
        return "Received: " + name + ", Age: " + age;
    }
}
```
- `@RequestParam`을 사용하여 URL의 쿼리 스트링 데이터를 개별 변수에 바인딩.

### 3.2 JSON 데이터 바인딩
```java
@RestController
@RequestMapping("/api")
public class DataController {

    @PostMapping("/data")
    public String postData(@RequestBody User user) {
        return "Received: " + user.getName() + ", Age: " + user.getAge();
    }
}

class User {
    private String name;
    private int age;

    // Getter & Setter
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```
- `@RequestBody`를 사용하여 JSON 데이터를 객체로 변환하여 받음.

---

## 4. 쿼리 스트링과 JSON의 차이점

| 구분 | 쿼리 스트링 (Query String) | JSON |
|------|----------------|------|
| **사용 방식** | `GET` 요청 시 URL에 포함 | `POST` 요청에서 `body`에 포함 |
| **데이터 구조** | `key=value` 형식 (`&`로 구분) | `{ "key": "value" }` 형식 |
| **보안** | URL에 노출됨 | 요청 본문(body)에 포함되어 숨겨짐 |
| **용량 제한** | 일반적으로 2048자 제한 | 제한 없음 |
| **예제** | `/api/data?name=John&age=30` | `{ "name": "John", "age": 30 }` |

---

## 5. 정리
| 방식 | jQuery AJAX 전송 | Spring 바인딩 방식 |
|------|----------------|------------------|
| **쿼리 스트링** | `data: { name: "John", age: 30 }` | `@RequestParam` 사용 |
| **JSON 데이터** | `data: JSON.stringify({ name: "John", age: 30 })` | `@RequestBody` 사용 |

📌 **GET 요청에서는 쿼리 스트링을, POST 요청에서는 JSON을 주로 사용!**