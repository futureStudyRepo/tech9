# 데이터 모델링과 ERD 정리

## 📌 데이터 모델링이란?
데이터 모델링(Data Modeling)은 데이터베이스를 체계적으로 설계하는 과정으로, **데이터 구조, 속성, 관계** 등을 정의하여 효율적인 데이터 관리를 가능하게 합니다. 

### 🔹 데이터 모델링의 주요 목표
1. **데이터 무결성 유지** - 중복 데이터 최소화
2. **데이터 일관성 보장** - 관계 설정을 통한 신뢰성 유지
3. **확장성과 성능 최적화** - 미래 데이터 증가 대비
4. **비즈니스 요구사항 반영** - 시스템 요구사항을 데이터 구조로 변환

### 🔹 데이터 모델링의 주요 단계
1. **개념적 모델링(Conceptual Modeling)**
   - 시스템에서 다루는 개체(Entity)와 관계(Relationship)를 정의
   - 주요 엔터티와 속성만 포함하며, 기술적 세부 사항은 제외
   
2. **논리적 모델링(Logical Modeling)**
   - 엔터티(Entity), 속성(Attribute), 기본 키(Primary Key), 외래 키(Foreign Key)를 정의
   - 특정 DBMS에 종속되지 않은 데이터 모델링 수행

3. **물리적 모델링(Physical Modeling)**
   - 논리적 모델을 특정 DBMS에서 사용 가능한 테이블, 컬럼, 인덱스 등의 형태로 변환
   - 성능 최적화 및 저장 방식 결정

---

## 📌 ERD(Entity-Relationship Diagram)란?
ERD는 **엔터티(Entity), 속성(Attribute), 관계(Relationship)** 를 시각적으로 표현한 다이어그램으로, 데이터베이스 설계의 기초가 됩니다.

### 🔹 ERD 구성 요소
| 요소 | 설명 | 표기법 |
|------|------|------|
| **엔터티(Entity)** | 데이터로 저장될 개체 (예: 사용자, 문제, 주문 등) | ▭ (사각형) |
| **속성(Attribute)** | 엔터티가 가지는 데이터 요소 (예: 이름, 이메일) | ⬭ (타원) |
| **기본 키(Primary Key, PK)** | 데이터를 유일하게 식별하는 속성 | 밑줄 |
| **외래 키(Foreign Key, FK)** | 다른 엔터티와 관계를 맺기 위한 속성 | FK 표기 |
| **관계(Relationship)** | 엔터티 간의 연관성 | ◇ (마름모) |

---

## 📌 4지선다 퀴즈 문제 모델링 과정

### **1. 요구사항 분석**
사용자는 4지선다 퀴즈를 풀 수 있으며, 각 문제는 4개의 선택지를 가짐. 사용자의 응답 데이터는 기록되어야 하며, 정답 여부를 판별할 수 있어야 함.

### **2. 엔터티 및 속성 정의**
- **QUESTION (문제)**: 문제의 텍스트 및 속성 저장
- **CHOICE (보기)**: 각 문제당 4개의 보기 저장
- **USER_TABLE (사용자)**: 퀴즈를 푸는 사용자 정보 저장
- **RESPONSE (응답)**: 사용자의 선택을 저장하고 정답 여부를 기록

### **3. 관계 정의**
1. `QUESTION` (1) : (N) `CHOICE` (1:N 관계) → 한 문제는 4개의 보기를 가짐
2. `USER_TABLE` (1) : (N) `RESPONSE` (1:N 관계) → 한 사용자는 여러 응답을 저장 가능
3. `RESPONSE` (N) : (1) `QUESTION` (N:1 관계) → 한 응답은 하나의 문제와 연결됨
4. `RESPONSE` (N) : (1) `CHOICE` (N:1 관계) → 한 응답은 하나의 보기와 연결됨

---

## 📌 4지선다 퀴즈 문제 ERD 테이블 설계 (Oracle 기준)

### 🔹 1. `QUESTION` (문제 테이블)
```sql
CREATE TABLE QUESTION (
    question_id NUMBER PRIMARY KEY,
    question_text VARCHAR2(500) NOT NULL,
    level VARCHAR2(20),
    category VARCHAR2(50)
);
```

### 🔹 2. `CHOICE` (보기 테이블)
```sql
CREATE TABLE CHOICE (
    choice_id NUMBER PRIMARY KEY,
    question_id NUMBER NOT NULL,
    choice_text VARCHAR2(255) NOT NULL,
    is_correct CHAR(1) CHECK (is_correct IN ('Y', 'N')),
    CONSTRAINT fk_choice_question FOREIGN KEY (question_id) REFERENCES QUESTION(question_id) ON DELETE CASCADE
);
```

### 🔹 3. `USER_TABLE` (사용자 테이블)
```sql
CREATE TABLE USER_TABLE (
    user_id NUMBER PRIMARY KEY,
    username VARCHAR2(100) NOT NULL,
    email VARCHAR2(200) UNIQUE NOT NULL
);
```

### 🔹 4. `RESPONSE` (응답 테이블)
```sql
CREATE TABLE RESPONSE (
    response_id NUMBER PRIMARY KEY,
    user_id NUMBER NOT NULL,
    question_id NUMBER NOT NULL,
    choice_id NUMBER NOT NULL,
    timestamp DATE DEFAULT SYSDATE,
    is_correct CHAR(1) GENERATED ALWAYS AS 
        (CASE WHEN choice_id IN (SELECT choice_id FROM CHOICE WHERE is_correct = 'Y') 
              THEN 'Y' ELSE 'N' END) VIRTUAL,
    CONSTRAINT fk_response_user FOREIGN KEY (user_id) REFERENCES USER_TABLE(user_id) ON DELETE CASCADE,
    CONSTRAINT fk_response_question FOREIGN KEY (question_id) REFERENCES QUESTION(question_id) ON DELETE CASCADE,
    CONSTRAINT fk_response_choice FOREIGN KEY (choice_id) REFERENCES CHOICE(choice_id) ON DELETE CASCADE
);
```

---

## 📌 데이터 삽입 예제
```sql
-- 문제 추가
INSERT INTO QUESTION (question_id, question_text, level, category)
VALUES (1, '데이터베이스의 기본 키란?', '쉬움', '데이터베이스');

-- 보기 추가 (4지선다)
INSERT INTO CHOICE (choice_id, question_id, choice_text, is_correct) VALUES (1, 1, '유일한 식별자', 'Y');
INSERT INTO CHOICE (choice_id, question_id, choice_text, is_correct) VALUES (2, 1, '외래 키', 'N');
INSERT INTO CHOICE (choice_id, question_id, choice_text, is_correct) VALUES (3, 1, 'NULL 값 허용', 'N');
INSERT INTO CHOICE (choice_id, question_id, choice_text, is_correct) VALUES (4, 1, '테이블의 컬럼', 'N');
```

---

