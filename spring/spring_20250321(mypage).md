# 📘 [tech9-스프링] 수업 내용 정리


## 수업 개요
**날짜:** 2025년 03월 21일  
**주제:** [spring framework 시큐리티-비번]  

 -  spring security, 

---

## 주요 개념

# 암호화 개념 정리

### 1. 암호화란?
암호화는 데이터를 보호하기 위해 평문 데이터를 특정 알고리즘을 통해 변형하여, 인가된 사용자만이 해당 데이터를 복호화할 수 있도록 만드는 기술입니다. 암호화 방식은 크게 **단방향 암호화**와 **양방향 암호화**로 나눌 수 있습니다.

- **단방향 암호화**: 암호화된 데이터를 복호화할 수 없는 방식으로, 주로 비밀번호 저장 시 사용됩니다. 복호화할 필요가 없는 데이터를 안전하게 관리하기 위한 용도로 사용됩니다.
- **양방향 암호화**: 암호화된 데이터를 다시 복호화할 수 있는 방식으로, 중요한 데이터 전송 시 사용됩니다. 데이터 복원 가능성이 필요한 경우에 적합합니다.

### 2. 스프링 프레임워크에서의 암호화

스프링 프레임워크에서 암호화는 주로 **Spring Security**를 통해 구현됩니다. 주로 비밀번호 암호화와 인증 관련 기능에 사용됩니다.

#### pom.xml 설정
Spring Security를 사용하기 위해 필요한 의존성을 `pom.xml`에 추가합니다:

```xml
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-web</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-config</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-taglibs</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>
```

#### 암호화 설정
암호화를 위한 빈(bean)을 설정합니다. 여기서는 **BCryptPasswordEncoder**를 사용하여 비밀번호를 암호화합니다:

```xml
<bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

```

```web.xml
	<param-value>/WEB-INF/spring/appServlet/servlet-context.xml
                    /WEB-INF/spring/appServlet/security-context.xml
    </param-value>

```

이 빈을 통해 암호화를 수행하며, 주로 비밀번호를 암호화한 후 데이터베이스에 저장할 때 사용됩니다.

### 3. 회원가입 시 암호화 적용
회원가입 시 사용자의 비밀번호를 암호화한 후 저장합니다. 이를 위해, 미리 설정된 **BCryptPasswordEncoder**를 사용하여 비밀번호를 안전하게 암호화합니다.

```java
String rawPassword = "userPassword";
String encodedPassword = passwordEncoder.encode(rawPassword);
```

### 4. 로그인 처리 시 암호화된 비밀번호 검증
로그인 시에는 사용자가 입력한 비밀번호와 데이터베이스에 저장된 암호화된 비밀번호를 비교합니다. 이때, **matches** 메서드를 사용하여 암호화된 비밀번호와 일치하는지 확인합니다.

```java
boolean isPasswordMatch = passwordEncoder.matches(rawPassword, encodedPassword);
```

이 메서드는 암호화된 비밀번호에서 Salt 값을 추출하여 동일한 방식으로 암호화한 후 비교합니다. 따라서, 비밀번호를 재암호화해도 값이 달라지지만, 비교는 정확하게 이루어집니다.

### 5. 암호화의 특징
- **Salt 사용**: BCrypt 암호화 방식에서는 Salt를 사용하여 동일한 비밀번호라도 서로 다른 암호화된 값을 생성합니다. 이를 통해 무작위 대입 공격(Brute Force Attack) 등을 방지합니다.
- **단방향 암호화**: BCrypt는 단방향 암호화로, 복호화가 불가능한 암호화 방식입니다. 따라서, 비밀번호는 복호화하지 않고 입력된 값과 비교하여 검증합니다.

### 6. 스프링 시큐리티 암호화 활용
스프링 시큐리티는 암호화 외에도 다양한 보안 기능을 제공합니다. 로그인, 권한 관리, CSRF 방어 등 다양한 기능과 결합하여 애플리케이션 보안을 강화할 수 있습니다.



다음은 Spring에서 `app.properties` 파일의 값을 불러오는 방법을 설명하는 내용입니다.


# Spring에서 `app.properties` 파일 값을 불러오는 방법

Spring 애플리케이션에서 외부 설정 파일(`app.properties`)의 값을 불러와 사용하려면 다음 단계를 따를 수 있습니다.

## 1. `app.properties` 파일 설정
먼저, 외부 설정 파일인 `app.properties` 파일에 필요한 설정 값을 정의합니다.

예시:
```
file.upload.path=/uploads/images
file.download.path=/downloads/images
```

## 2. `servlet-context.xml`에서 `util:properties` 사용
Spring XML 설정 파일(`servlet-context.xml`)에서 `util:properties` 태그를 사용하여 해당 설정 파일을 Spring 컨텍스트에 로드합니다.

```xml
        <beans:beans xmlns="http://www.springframework.org/schema/mvc"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:beans="http://www.springframework.org/schema/beans"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:util="http://www.springframework.org/schema/util"
            xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
                http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util-4.3.xsd">
```

```xml
<util:properties id="util" location="classpath:/config/app.properties" />
```

이 설정은 `app.properties` 파일을 읽고, 그 안의 값을 Spring 애플리케이션에서 사용할 수 있도록 합니다.

## 3. Java 클래스에서 `@Value`로 설정 값 불러오기
이제 Java 클래스에서 `@Value` 어노테이션을 사용하여 `app.properties` 파일의 값을 불러올 수 있습니다.

```java
import org.springframework.beans.factory.annotation.Value;

public class FileService {

    @Value("#{util['file.upload.path']}")
    private String CURR_IMAGE_PATH;

    @Value("#{util['file.download.path']}")
    private String WEB_PATH;

    // CURR_IMAGE_PATH와 WEB_PATH를 사용하는 메서드들
}
```

위 코드에서는 `app.properties` 파일에 정의된 `file.upload.path`와 `file.download.path` 값을 각각 `CURR_IMAGE_PATH`와 `WEB_PATH` 변수에 저장합니다.

## 요약
1. `app.properties` 파일에 설정 값을 정의합니다.
2. `servlet-context.xml` 파일에서 `util:properties` 태그로 설정 파일을 로드합니다.
3. Java 클래스에서 `@Value` 어노테이션을 사용하여 설정 값을 불러옵니다.

이렇게 하면 외부 설정 파일을 통해 애플리케이션의 경로나 기타 설정 값을 손쉽게 관리할 수 있습니다.

---
# 이미지 파일 업로드
**이미지 파일 업로드**를 처리하는 Spring 서버와 JavaScript 클라이언트 간의 상호작용을 설명합니다. 코드는 클라이언트 측에서 이미지를 선택하고, 서버로 전송하여 업로드된 이미지를 처리한 후, 웹 페이지에 바로 반영하는 과정을 포함하고 있습니다.

## 1. 필수 라이브러리 등록

이 두 가지 `dependency`는 Apache Commons 라이브러리에서 파일 업로드 및 입출력(IO) 작업을 처리하기 위한 의존성입니다. 각 의존성의 역할을 설명하겠습니다.

### 1. `commons-fileupload`

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```

- **역할**: `commons-fileupload`는 **파일 업로드**를 지원하는 라이브러리입니다. 이 라이브러리를 통해 애플리케이션에서 클라이언트로부터 전송된 파일을 처리할 수 있습니다.
- **주요 기능**:
  - HTTP 요청으로 전송된 파일을 해석하고 파일 객체로 변환하는 기능.
  - 다중 파일 업로드를 지원하며, 메모리나 디스크로 파일을 저장하는 방법을 제공합니다.
  - Spring에서 `MultipartFile`과 연동되어 파일 업로드 처리를 쉽게 할 수 있도록 돕습니다.

### 2. `commons-io`

```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.8.0</version>
</dependency>
```

- **역할**: `commons-io`는 **파일 및 입출력(IO) 작업**을 보다 편리하게 처리하기 위한 유틸리티 라이브러리입니다.
- **주요 기능**:
  - 파일 및 디렉토리 관련 유틸리티 메서드 제공 (파일 복사, 이동, 삭제, 디렉토리 생성 등).
  - 스트림(Stream) 처리를 위한 편리한 메서드 (데이터 읽기/쓰기, 버퍼 처리 등).
  - 파일의 확장자, MIME 타입, 파일 경로 등을 다루는 기능.
  - 파일 시스템과 관련된 다양한 편의 메서드를 제공하여 입출력 작업을 효율적으로 처리할 수 있도록 도와줍니다.

### 요약
- **`commons-fileupload`**: 클라이언트가 업로드한 파일을 서버에서 처리하는 기능을 제공.
- **`commons-io`**: 파일 입출력 관련 작업을 쉽게 할 수 있는 유틸리티 기능을 제공.

이 두 라이브러리는 파일 업로드 및 처리 작업을 할 때 유용하게 사용됩니다.

## 2. 서버 코드 (Spring Controller)

### 2.1 파일 업로드 처리 메서드 (`uploadFiles`)

```java
@ResponseBody
@PostMapping("/files/upload")
public Map<String, Object> uploadFiles(
    @RequestParam("uploadImage") MultipartFile uploadImage,
    HttpSession session) throws Exception {
    
    MemberVO login = (MemberVO) session.getAttribute("login");  // 현재 로그인된 사용자 정보 가져오기
    String imgPath = memberService.uploadProfile(login, CURR_IMATE_PATH, WEB_PATH, uploadImage);  // 이미지 업로드 처리
    Map<String, Object> map = new HashMap<>();  // 결과를 담을 맵 생성
    map.put("message", "success");  // 성공 메시지 추가
    map.put("imagePath", imgPath);  // 이미지 경로 추가
    return map;  // 결과 반환
}
```

- `@PostMapping("/files/upload")`: 클라이언트로부터 `/files/upload` 경로로 POST 요청을 받습니다.
- `@RequestParam("uploadImage")`: 요청으로부터 전송된 이미지 파일을 받아옵니다.
- `HttpSession session`: 현재 로그인한 사용자 세션을 사용하여 `MemberVO` 객체에서 로그인 정보를 가져옵니다.
- `memberService.uploadProfile`: 서비스 메서드를 호출하여 이미지를 업로드합니다.
- 업로드된 이미지 경로(`imgPath`)를 클라이언트에게 전달합니다.

### 2.2 파일 업로드 처리 서비스 (`uploadProfile`)

```java
public String uploadProfile(MemberVO member, String uploadDir, String webPath, MultipartFile file) throws Exception {
    String originFilename = file.getOriginalFilename();  // 원본 파일 이름 가져오기
    String storedFilename = UUID.randomUUID().toString() + "_" + originFilename;  // 유니크한 파일명 생성
    String dbFilePath = webPath + storedFilename;  // 웹에서 사용할 파일 경로
    Path filePath = Paths.get(uploadDir, storedFilename);  // 서버에 저장할 파일 경로 설정

    try {
        Files.copy(file.getInputStream(), filePath);  // 서버에 파일 저장
    } catch (IOException e) {
        throw new Exception("file to save the file", e);  // 저장 실패 시 예외 처리
    }

    member.setProfileImg(dbFilePath);  // 멤버 객체에 이미지 경로 설정
    int result = dao.profileUpload(member);  // DB에 저장

    if (result == 0) {
        throw new Exception();  // DB에 저장 실패 시 예외 처리
    }

    return dbFilePath;  // 이미지 경로 반환
}
```

- `MultipartFile file`: 업로드된 파일 객체를 받아 처리합니다.
- `UUID.randomUUID()`: 중복된 파일명을 방지하기 위해 고유한 파일명을 생성합니다.
- `Files.copy()`: 파일을 서버의 실제 경로(`uploadDir`)에 저장합니다.
- `dbFilePath`: 저장된 파일의 경로를 반환하고, 사용자의 프로필 이미지 경로를 DB에 업데이트합니다.

## 3. 클라이언트 코드 (JavaScript + jQuery)

### 3.1 이미지 선택 버튼 처리

```javascript
$(document).ready(function(){
    $("#myImage").click(function(){
        $("#uploadImage").click();  // 이미지를 클릭하면 파일 선택 창이 열립니다.
    });
});
```

- `#myImage`를 클릭하면 `#uploadImage` 파일 입력 필드가 클릭되어 파일 선택 창이 열립니다.

### 3.2 이미지 파일 선택 및 유효성 검사

```javascript
$("#uploadImage").on("change", function(){
    let file = $(this)[0].files[0];  // 선택된 파일을 가져옴
    if(file){
        let fileType = file['type'];  // 파일 타입 확인
        let valTypes = ['image/gif','image/jpeg','image/png','image/jpg'];  // 허용된 이미지 파일 타입
        if(!valTypes.includes(fileType)){
            alert("유효한 이미지 타입이 아닙니다.!!");  // 유효하지 않은 이미지 타입이면 알림
            $(this).val('');  // 선택된 파일 초기화
        } else {
            let formData = new FormData($('#profileForm')[0]);  // FormData 객체에 폼 데이터를 담음
            $.ajax({
                url: '<c:url value="/files/upload" />',  // 서버에 파일을 업로드할 경로
                type: 'POST',
                data: formData,  // FormData 객체를 전송
                dataType: 'json',
                processData: false,  // 데이터를 URL 인코딩하지 않음
                contentType: false,  // contentType 설정하지 않음 (기본값 사용)
                success: function(res){
                    console.log(res);  // 성공 시 응답 출력
                    $("#myImage").attr("src", "${pageContext.request.contextPath}" + res.imagePath);  // 업로드된 이미지 경로로 프로필 이미지 업데이트
                },
                error: function(e){
                    console.log(e);  // 오류 발생 시 콘솔에 출력
                }
            });
        }
    }
});
```

- 파일이 선택되면 `#uploadImage`의 `change` 이벤트가 발생합니다.
- 이미지 파일의 유효성을 검사한 후, 유효한 파일이면 서버에 AJAX 요청을 통해 이미지를 업로드합니다.
- 업로드가 완료되면 서버가 반환한 경로(`res.imagePath`)를 사용하여 페이지에서 이미지를 즉시 업데이트합니다.

## 4. 전체 흐름 요약
1. **클라이언트 측에서 파일 선택**:
   - 사용자가 이미지를 클릭하면 파일 선택 창이 열리고, 파일을 선택하면 유효성을 검사합니다.
2. **서버로 파일 업로드**:
   - 파일이 유효하면 `FormData` 객체를 사용하여 AJAX 요청으로 서버에 파일을 전송합니다.
3. **서버에서 파일 저장**:
   - 서버는 해당 파일을 특정 경로에 저장하고, 파일 경로를 DB에 저장한 후, 클라이언트로 반환합니다.
4. **이미지 경로 반영**:
   - 서버에서 반환된 파일 경로를 사용하여 웹 페이지에 업로드된 이미지를 반영합니다.

이 코드는 사용자가 프로필 이미지를 쉽게 업로드하고, 업로드된 이미지를 웹 페이지에서 즉시 확인할 수 있도록 합니다.


```html
<form id="profileForm" enctype="multipart/form-data">
    <input type="file" name="uploadImage" id="uploadImage" accept="image/*" style="display:none;" >
</form>
```

### `multipart/form-data`의 역할:
- **파일 전송**: HTML 폼이 파일을 서버로 전송할 수 있게 하는 인코딩 방식입니다. 기본 폼 데이터 인코딩 방식은 `application/x-www-form-urlencoded`로, 파일을 포함하지 않는 간단한 텍스트 데이터만 전송할 수 있습니다. 반면, `multipart/form-data`는 텍스트 데이터와 파일을 함께 전송할 수 있습니다.
- **파일 입력 필드**: `<input type="file">`를 사용하여 파일을 선택하면, 파일 데이터가 이 방식으로 폼에 포함되어 서버로 전송됩니다.

### 요약:
- `enctype="multipart/form-data"`는 폼 데이터와 함께 파일을 서버로 전송할 때 반드시 필요합니다.
- `<input type="file">`은 파일을 선택할 수 있게 하고, 선택된 파일이 전송될 때 `multipart/form-data`를 통해 서버로 전송됩니다.

이 인코딩 방식 덕분에 서버는 클라이언트가 전송한 파일을 읽고 처리할 수 있습니다.



이 코드는 **파일 다운로드**를 처리하는 Spring 컨트롤러 메서드입니다. 클라이언트가 서버에서 파일을 다운로드할 수 있도록 파일을 읽고 전송하는 역할을 합니다. 아래는 코드의 각 부분을 설명한 내용입니다.

### 1. 메서드 설명

```java
@RequestMapping("/download")
public void download(String imageFileName, HttpServletResponse response) throws IOException {
```
- **`@RequestMapping("/download")`**: 이 메서드는 `/download` 경로로 들어오는 HTTP 요청을 처리합니다.
- **`String imageFileName`**: 다운로드할 파일의 이름을 요청 파라미터로 받습니다.
- **`HttpServletResponse response`**: HTTP 응답 객체로, 클라이언트에게 파일 데이터를 전송하는 데 사용됩니다.

### 2. 파일 경로 설정 및 파일 객체 생성

```java
OutputStream out = response.getOutputStream();
String downFile = CURR_IMATE_PATH + "\\" + imageFileName;
File file = new File(downFile);
```
- **`OutputStream out = response.getOutputStream()`**: 클라이언트에게 데이터를 전송하기 위한 출력 스트림을 생성합니다.
- **`CURR_IMATE_PATH + "\\" + imageFileName`**: 파일을 저장한 경로와 파일 이름을 결합해 실제 파일의 경로(`downFile`)를 만듭니다.
- **`File file = new File(downFile)`**: 해당 경로의 파일을 나타내는 `File` 객체를 생성합니다.

### 3. 파일이 존재하는지 확인

```java
if (!file.exists()) {
    response.sendError(HttpServletResponse.SC_NOT_FOUND, "file not found");
}
```
- **파일 존재 여부 확인**: `file.exists()`로 파일이 서버에 존재하는지 확인합니다.
- **404 오류 반환**: 파일이 존재하지 않으면 `response.sendError()`를 통해 HTTP 404 응답을 클라이언트에게 전송하고, "file not found"라는 메시지를 반환합니다.

### 4. HTTP 헤더 설정

```java
response.setHeader("Cache-Control", "no-cache");
response.setHeader("Content-Disposition", "attachment; fileName=" + imageFileName);
```
- **`Cache-Control: no-cache`**: 클라이언트가 파일을 캐시하지 않도록 설정합니다.
- **`Content-Disposition: attachment`**: 이 헤더는 파일을 다운로드로 처리하게 만듭니다. `fileName=`을 통해 파일 이름을 지정하여 클라이언트가 다운로드 시 해당 이름으로 저장되게 합니다.

### 5. 파일 데이터 전송

```java
try (FileInputStream in = new FileInputStream(file)) {
    byte[] buffer = new byte[1024 * 8];  // 8KB 버퍼 생성
    while (true) {
        int count = in.read(buffer);  // 파일 데이터를 버퍼에 읽어들임
        if (count == -1) {  // 더 이상 읽을 데이터가 없으면 루프 종료
            break;
        }
        out.write(buffer, 0, count);  // 읽어들인 데이터를 클라이언트로 전송
    }
} catch (IOException ex) {
    // 예외 처리 (파일 전송 중 오류 발생 시)
} finally {
    out.close();  // OutputStream 닫기
}
```
- **`FileInputStream`**: 파일을 읽기 위한 스트림을 생성합니다.
- **`byte[] buffer = new byte[1024 * 8]`**: 8KB 크기의 버퍼를 생성하여 파일 데이터를 읽어들일 준비를 합니다.
- **파일 읽기 및 전송**: `in.read(buffer)`로 파일에서 데이터를 읽어 버퍼에 저장하고, `out.write()`로 클라이언트에게 데이터를 실시간으로 전송합니다.
- **루프 종료**: 더 이상 읽을 데이터가 없으면 (`count == -1`), 루프가 종료됩니다.
- **스트림 종료**: 작업이 끝나면 `out.close()`로 출력 스트림을 닫습니다.

### 6. 예외 처리

```java
catch (IOException ex) {
    // 예외 처리: 파일 전송 중 오류 발생 시 처리할 코드
}
```
- 파일 전송 중 `IOException`이 발생할 수 있습니다. 예외가 발생한 경우, 여기서 필요한 처리를 할 수 있습니다.

### 요약
1. 클라이언트가 `/download` 경로로 파일 이름을 요청하면, 서버는 해당 파일을 `CURR_IMATE_PATH` 경로에서 찾아 파일이 존재하는지 확인합니다.
2. 파일이 존재하면, 파일을 읽어들여 HTTP 응답으로 클라이언트에게 전송합니다.
3. 클라이언트는 파일을 다운로드하며, 서버는 파일을 읽고 실시간으로 클라이언트에 파일 데이터를 보냅니다.
4. 파일이 존재하지 않으면, 404 에러를 반환합니다.

이 메서드는 파일 다운로드를 위해 서버의 파일을 클라이언트로 스트리밍 방식으로 전송하는 전형적인 방식입니다.

## 참고 자료

- security 관련

```xml
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-web</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-config</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-taglibs</artifactId>
  <version>5.3.13.RELEASE</version>
</dependency>
```

- file업로드 관련

```xml
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.4</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.8.0</version>
        </dependency>

```

```xml 
    <!-- root-contenxt -->

    
	<bean id="multipartResolver" 
	      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	 		<property name="maxUploadSize" value="104857600" />
	 		<property name="maxUploadSizePerFile" value="104857600" />
	 		<property name="maxInMemorySize" value="104857600" />
	 </bean>
```

## 질문 