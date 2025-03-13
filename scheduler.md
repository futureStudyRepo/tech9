
# APScheduler 및 Logger 사용법

## 1. APScheduler

### 개요
APScheduler(Advanced Python Scheduler)는 Python에서 정기적으로 특정 작업을 실행할 수 있도록 도와주는 라이브러리입니다. 주로 백그라운드 작업을 실행하거나 예약된 작업을 수행하는 데 사용됩니다.

### 설치
```bash
pip install apscheduler
```

### 주요 구성 요소
1. **Trigger**: 작업이 실행될 시점을 정의 (예: 일정 시간마다, 특정 시간에 실행 등)
2. **Job Store**: 작업을 저장하는 방식 (메모리, 데이터베이스 등)
3. **Executor**: 작업을 실행하는 방식 (스레드풀, 프로세스풀 등)
4. **Scheduler**: 전체 작업을 관리하는 객체

### 기본 사용법
```python
from apscheduler.schedulers.background import BackgroundScheduler
from datetime import datetime
import time

def job_function():
    print(f"작업 실행 시간: {datetime.now()}")

# 스케줄러 생성
scheduler = BackgroundScheduler()
scheduler.add_job(job_function, 'interval', seconds=5)  # 5초마다 실행
scheduler.start()

try:
    while True:
        time.sleep(1)
except (KeyboardInterrupt, SystemExit):
    scheduler.shutdown()
```

### 다양한 트리거 예시
```python
# 특정 시간에 실행
scheduler.add_job(job_function, 'date', run_date='2025-03-15 12:00:00')

# 매일 특정 시간에 실행
scheduler.add_job(job_function, 'cron', hour=12, minute=30)

# 매 10초마다 실행
scheduler.add_job(job_function, 'interval', seconds=10)
```

### 함수 호출 시 파라미터 전송 방법
```python
def job_with_args(name, age):
    print(f"이름: {name}, 나이: {age}")

scheduler.add_job(job_with_args, 'interval', seconds=5, args=["홍길동", 30])
```

```python
def job_with_kwargs(name, age):
    print(f"이름: {name}, 나이: {age}")

scheduler.add_job(job_with_kwargs, 'interval', seconds=5, kwargs={"name": "홍길동", "age": 30})
```

### Job 멈추기 및 제거하기
```python
job = scheduler.add_job(job_function, 'interval', seconds=5)

# 특정 작업 멈추기
job.pause()

# 특정 작업 다시 시작하기
job.resume()

# 특정 작업 제거하기
job.remove()
```

---

## 2. Logger

### 개요
Logger는 Python의 `logging` 모듈을 활용하여 로그를 남길 수 있도록 도와줍니다. 이는 디버깅, 오류 추적, 시스템 동작 기록에 유용합니다.

### 기본 사용법
```python
import logging

# Logger 설정
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler("app.log"),  # 파일에 로그 저장
        logging.StreamHandler()  # 콘솔에 로그 출력
    ]
)

logger = logging.getLogger("my_logger")

logger.debug("디버그 메시지")
logger.info("정보 메시지")
logger.warning("경고 메시지")
logger.error("에러 메시지")
logger.critical("치명적인 오류")
```

### 로그 레벨
| 레벨 | 설명 |
|------|----------------|
| DEBUG | 상세한 정보 (디버깅용) |
| INFO | 일반적인 정보 |
| WARNING | 경고 메시지 (문제 발생 가능) |
| ERROR | 오류 발생 |
| CRITICAL | 심각한 오류 발생 |

### 로그 파일 분할 (RotatingFileHandler)
```python
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler("app.log", maxBytes=10000, backupCount=5)
logging.basicConfig(handlers=[handler], level=logging.INFO)
```

---

## 3. APScheduler와 Logger 함께 사용하기
```python
from apscheduler.schedulers.background import BackgroundScheduler
import logging
import time

# Logger 설정
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
logger = logging.getLogger("scheduler_logger")

def scheduled_task():
    logger.info("스케줄된 작업 실행됨")

scheduler = BackgroundScheduler()
scheduler.add_job(scheduled_task, 'interval', seconds=5)
scheduler.start()

try:
    while True:
        time.sleep(1)
except (KeyboardInterrupt, SystemExit):
    scheduler.shutdown()
    logger.info("스케줄러 종료됨")
```

## 백그라운드 실행

- cmd
```conda activate tech9```
- 백그라운드 실행
```pythonw coin_scheduler.py```   
- 체크
```tasklist | findstr python```
- 프로세스 종료 
```taskkill /PID 1234567 /F```
