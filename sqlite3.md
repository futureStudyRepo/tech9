# SQLite3 in Python

SQLite3는 Python에서 가벼운 데이터베이스를 사용할 수 있도록 도와주는 내장 모듈입니다. SQLite는 서버 없이 파일 기반으로 동작하는 관계형 데이터베이스 관리 시스템(RDBMS)입니다.

## 1. SQLite3 설치 및 기본 사용법
Python에는 `sqlite3` 모듈이 기본적으로 포함되어 있어 별도의 설치 없이 사용할 수 있습니다.

```python
import sqlite3

# 데이터베이스 연결 (파일이 없으면 자동 생성됨)
conn = sqlite3.connect("example.db")

# 커서 객체 생성
cursor = conn.cursor()
```

## 2. 테이블 생성
테이블을 생성하려면 SQL `CREATE TABLE` 문을 사용합니다.

```python
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    age INTEGER
)
''')
conn.commit()  # 변경 사항 저장
```

## 3. 데이터 삽입
데이터를 삽입할 때는 `INSERT INTO` 문을 사용합니다.

```python
cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ("Alice", 25))
conn.commit()
```

## 4. 데이터 조회
데이터를 조회할 때는 `SELECT` 문을 사용하며, `fetchall()` 또는 `fetchone()` 메서드를 활용합니다.

```python
cursor.execute("SELECT * FROM users")
rows = cursor.fetchall()

for row in rows:
    print(row)
```

## 5. 데이터 업데이트
`UPDATE` 문을 사용하여 데이터를 수정할 수 있습니다.

```python
cursor.execute("UPDATE users SET age = ? WHERE name = ?", (26, "Alice"))
conn.commit()
```

## 6. 데이터 삭제
`DELETE` 문을 사용하여 데이터를 삭제할 수 있습니다.

```python
cursor.execute("DELETE FROM users WHERE name = ?", ("Alice",))
conn.commit()
```

## 7. 연결 종료
작업이 끝나면 반드시 연결을 종료해야 합니다.

```python
conn.close()
```

## 8. 예외 처리
데이터베이스 작업 중 예외가 발생할 수 있으므로 `try-except` 구문을 사용하는 것이 좋습니다.

```python
try:
    conn = sqlite3.connect("example.db")
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users")
    rows = cursor.fetchall()
    print(rows)
except sqlite3.Error as e:
    print("에러 발생:", e)
finally:
    conn.close()
```

## 9. SQLite 파일 삭제
SQLite 데이터베이스 파일을 삭제하려면 `os` 모듈을 사용할 수 있습니다.

```python
import os

if os.path.exists("example.db"):
    os.remove("example.db")
```

## 결론
SQLite3는 가볍고 빠른 관계형 데이터베이스로, Python과 쉽게 연동할 수 있습니다. 위의 기본적인 SQL 연산을 익히면 간단한 데이터 저장 및 분석에 활용할 수 있습니다.

