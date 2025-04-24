# 📘 EC2 MySQL 설치 및 사용 가이드

> AWS EC2에 MySQL을 설치하고 Workbench로 연동

---

## 🖥️ 1. EC2 접속 및 MySQL 설치

### 📌 1-1. EC2 인스턴스 접속
```bash
ssh -i [키파일].pem ubuntu@[EC2_PUBLIC_IP]
```

### 📌 1-2. MySQL 설치
```bash
sudo apt update
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
```
---

## 🔐 2. 보안 설정

### 📌 2-1. MySQL 보안 마법사 실행

### 🔐 `mysql_secure_installation`이란?

> MySQL 설치 후 **기본 보안 설정**을 도와주는 **인터랙티브 명령어**입니다.  
> 시스템을 안전하게 만들어주는 필수 초기 설정 도구입니다.

---
### 📋 이 명령어가 하는 일 (질문으로 나옴)

| 설정 항목 | 설명 | 권장 답변 |
|-----------|------|-----------|
| ✅ root 비밀번호 설정 | 루트 계정에 비밀번호 부여 | Yes |
| 🚫 익명 사용자 삭제 | `anonymous` 사용자 제거 | Yes |
| ⛔ 원격 루트 로그인 차단 | 외부에서 root로 로그인 못 하게 | Yes |
| 🧹 테스트 DB 제거 | 누구나 접근 가능한 테스트 DB 삭제 | Yes |
| 🔁 권한 테이블 다시 로딩 | 설정이 즉시 반영되도록 | Yes |

---

### 🧾 실행 방법

```bash
sudo mysql_secure_installation
```
실행하면 아래처럼 질문을 받습니다:
```plaintext
VALIDATE PASSWORD PLUGIN can be used to test passwords
Do you want to continue with password setup? [Y/n]
...
Set root password? [Y/n]
Remove anonymous users? [Y/n]
Disallow root login remotely? [Y/n]
Remove test database and access to it? [Y/n]
Reload privilege tables now? [Y/n]
```

모두 **Y 또는 엔터(기본값 Yes)** 누르는 걸 권장합니다.

---
### ❓ 꼭 해야 하나요?
| 상황 | 설명 |
|------|------|
| ✅ 개인 서버, 교육용 프로젝트라도 | **보안 기본 습관**으로 무조건 추천 |
| ✅ EC2 같은 클라우드 서버 | 외부 노출되므로 필수 |
| ❌ Docker나 임시 테스트 환경 | 생략 가능하지만 비밀번호 설정은 별도로 해줘야 함 |
---
### ✅ 정리 요약
> `mysql_secure_installation`은 **보안 취약점을 없애고 루트 계정 보호를 도와주는 설정 마법사**입니다.  
👉 **실제 운영환경, 클라우드 환경에서는 반드시 실행해야 합니다.**

---

### 📌 2-2. 외부 접속 허용 설정
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
# bind-address = 127.0.0.1 → 0.0.0.0 으로 변경
sudo systemctl restart mysql
```

---

## 👤 3. 사용자 및 권한 생성

```sql
-- MySQL 접속
sudo mysql -u root -p

-- DB 및 사용자 생성
CREATE DATABASE mydb;
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'%';
FLUSH PRIVILEGES;
```

---

## 🧑‍💻 4. MySQL Workbench 연동

### 📌 4-1. EC2 보안 그룹에서 포트 3306 허용

### 📌 4-2. Workbench 설정
1. `+` 버튼 → 새 연결
2. 입력 정보:
    - Hostname: EC2 퍼블릭 IP
    - Port: 3306
    - Username: myuser
    - Password: (입력 후 저장)
    - Default Schema: mydb (선택)

3. **Test Connection** → 성공 → 저장

---

## 📋 5. SQL 문법 비교 (Oracle vs MySQL)

| 기능            | Oracle                                      | MySQL                                      |
|-----------------|---------------------------------------------|---------------------------------------------|
| 테이블 생성      | VARCHAR2, NUMBER, CLOB                      | VARCHAR, INT, TEXT                          |
| 자동 증가 PK     | 시퀀스 + 트리거                             | AUTO_INCREMENT                              |
| 날짜 함수        | SYSDATE                                     | NOW(), CURRENT_TIMESTAMP                    |
| 한정 행 조회     | `ROWNUM <= 10`                              | `LIMIT 10`                                  |
| 페이징 처리      | 복잡한 서브쿼리                             | `LIMIT 10 OFFSET 10` 또는 `LIMIT 10, 10`    |
| 행 번호 부여     | `ROWNUM`, `ROW_NUMBER()`                    | `ROW_NUMBER() OVER(ORDER BY ...)` (MySQL 8↑)|
| CLI 도구         | SQL*Plus                                    | mysql CLI                                   |

---

## 🆚 6. MySQL vs MariaDB 차이 요약

| 항목           | MySQL                                  | MariaDB                                  |
|----------------|------------------------------------------|-------------------------------------------|
| 소속           | Oracle                                   | 오픈소스, MySQL 창립자 주도               |
| 라이선스        | GPL + 제약 가능성                         | 100% GPL (오픈소스 강조)                  |
| JSON 지원      | 강력한 JSON 타입 및 함수                  | JSON은 문자열 처리 수준                    |
| 윈도우 함수     | 8.0부터 지원                             | 일부 버전부터 지원 (버전별 확인 필요)     |
| 기본 스토리지 엔진 | InnoDB                                  | InnoDB + Aria                              |
| 사용성          | 대규모 시스템, RDS 등에서 기본 선택       | 가벼운 시스템, 오픈소스 프로젝트 선호      |
> GPL(General Public License)
---

## ✅ 7. 예제: 테이블 생성 및 데이터 삽입

```sql
USE mydb;

CREATE TABLE cards (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nm VARCHAR(50),
    email VARCHAR(100),
    message1 TEXT,
    message2 TEXT,
    message3 TEXT
) CHARACTER SET utf8mb4;

INSERT INTO cards (nm, email, message1, message2, message3)
VALUES ('김팽수', 'hong@example.com', 'Dear 팽수 <br>, To : 김팽수' , '올 한 해 동안, 새로운 환경 속에서도 최선을 다하신 모습 정말 대단했어요! 남은 2025년 따뜻하게 보내시고. 내년에는 건강과 행복 그리고 큰 성취가 함께하길 바랄게요.<br> 2026년에는 다이어트 성공이라는 선물도 받을 수 있기를 응원합니다!🎉🎁' , 'From. nick@gmail.com' );
```

---

## 🧾 8. 쿼리 예제 (LIMIT, ROWNUM 대체)

```sql
-- 상위 10개 레코드
SELECT * FROM cards LIMIT 10;

-- 11~20번째 (페이지네이션)
SELECT * FROM cards ORDER BY id LIMIT 10 OFFSET 10;

-- MySQL 8 이상에서 행 번호 붙이기
SELECT 
    ROW_NUMBER() OVER (ORDER BY id) AS rownum,
    *
FROM cards;
```

---

## 🛑 9. 종료 명령

```bash
-- MySQL 종료
exit

