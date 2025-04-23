물론입니다, 선생님. 아래는 말씀하신 **Spring Boot 실행 및 시간 설정 관련 리눅스 명령어 스크립트**를 마크다운 형식으로 정리한 내용입니다:

---

# ☕ Ubuntu 서버에서 Spring Boot 실행 스크립트 (OpenJDK 17 & 시간 설정)

## 📦 패키지 목록 갱신 및 업그레이드
```bash
# 패키지 목록 업데이트
sudo apt update
# 설치 가능한 패키지 업그레이드 (자동 승인)
sudo apt upgrade -y
```

## ☕ OpenJDK 17 설치
```bash
# OpenJDK 17 설치
sudo apt install openjdk-17-jdk -y
```

## 🕒 시스템 시간 확인 및 변경
```bash
# 현재 시간 확인
date
# 기존 시간 설정 링크 삭제
sudo rm /etc/localtime
# 시간대를 서울(Asia/Seoul)로 설정
sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

# 변경된 시간 확인
date
```

## 🚀 Spring Boot 애플리케이션 실행
```bash
# 일반 실행 (포트 8080)
java -jar letter-0.0.1-SNAPSHOT.jar --server.port=8080

# 백그라운드 실행 및 로그 저장
nohup java -jar letter-0.0.1-SNAPSHOT.jar --server.port=8080 > letter.log 2>&1 &
```

## 🔍 실시간 로그 확인
```bash
# 로그 파일 실시간 출력
tail -f letter.log
```

## 🧠 Java 프로세스 확인
```bash
# 실행 중인 java 프로세스 확인
ps -ef | grep java
```

## AWS 인바운드 규칙 편집
- 8080 포트를 전체 허용
---

# 🛠️ Spring boot Gradle 프로젝트 JAR 빌드 및 배포

## ✅ 1. `build.gradle` 설정 확인

`build.gradle` 파일에 아래 플러그인과 설정이 있어야 합니다:

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.4'  // 사용 중인 버전에 맞게
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'  // 또는 11 등

bootJar {
    archiveFileName = 'myapp.jar'  // 생성될 jar 이름 (선택사항)
}
```

> `bootJar`는 실행 가능한 Spring Boot JAR을 생성합니다.

---

## ✅ 2. Gradle JAR 빌드 명령 실행

### 방법 A: STS에서 직접 실행
1. **Gradle Tasks** 뷰 열기 (View → Gradle Tasks)
2. `project → build → bootJar` 더블클릭 실행

### 방법 B: 터미널에서 실행 (STS 내 터미널 or 외부)
```bash
./gradlew bootJar
```

> Windows에서는: `gradlew.bat bootJar`

---

## ✅ 3. 빌드된 JAR 확인 경로

```bash
./build/libs/myapp.jar
```

→ 이 파일을 EC2에 업로드하면 됩니다.

---

## ✅ 4. EC2 업로드 및 실행 (요약)

```bash
nohup java -jar myapp.jar > app.log 2>&1 &
```

---

