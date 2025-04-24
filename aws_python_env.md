
---

# 🚀 EC2에 Flask 프로젝트 배포 및 실행 (Miniconda + Gunicorn)
> 처음 EC2에 접속한 상태에서, Git 프로젝트 클론부터 백그라운드 실행.

---

## 📦 1. EC2 초기 설정

### 1.1 시스템 업데이트
```bash
sudo apt update && sudo apt upgrade -y
```

### 1.2 Git 설치
```bash
sudo apt install git -y
```

---

## 🐍 2. Miniconda 설치 및 설정

### 2.1 Miniconda 설치
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
source ~/.bashrc
```

Anaconda와 Miniconda의 차이는 **설치 용량, 포함된 패키지 수, 초기 설정의 무게감**에 있습니다. 아래 표와 함께 정리해드릴게요.

---

### 📦 Anaconda vs Miniconda 차이 정리

| 항목 | **Anaconda** | **Miniconda** |
|------|--------------|---------------|
| 🔧 용도 | 풀 패키지 포함된 과학 컴퓨팅 환경 | 가볍고 최소한의 Conda 환경 |
| 📦 포함된 패키지 | 수백 개의 과학/데이터 과학 패키지 포함 (`numpy`, `pandas`, `jupyter`, 등) | `conda`만 포함 (필요한 패키지는 직접 설치) |
| 💾 설치 용량 | 3~4GB 이상 | 50~100MB 수준 |
| 🚀 설치 후 준비 상태 | 설치 직후 바로 데이터 과학 작업 가능 | 필요한 것만 골라 설치해야 함 |
| 🧰 환경 구성 방식 | 일괄 설치 및 자동 구성 | 사용자가 원하는 대로 선택해서 구성 |
| 📈 대상 사용자 | 데이터 과학 초보자, 빠르게 개발환경이 필요한 사람 | 고급 사용자, 최소한의 환경에서 시작하고 싶은 사람 |

---

### 🎯 선택 기준

- **Anaconda 추천**
  - 파이썬과 데이터 분석 처음 시작할 때
  - Jupyter Notebook, numpy, pandas 등을 **한 번에 설치**하고 싶을 때
  - 오프라인 환경에서 설치해야 할 때 (기본 패키지 동봉)

- **Miniconda 추천**
  - 가볍고 빠르게 환경 구성하고 싶을 때
  - 원하는 패키지만 설치해서 **최적화된 환경**을 만들고 싶을 때
  - 디스크 공간이 제한적일 때
  - 여러 가상환경을 사용해야 할 때

---

### 2.2 가상환경 생성 및 활성화
```bash
conda create -n flaskenv python=3.9 -y
conda activate flaskenv
```

---

## 📁 3. Flask 프로젝트 Git 클론

### 3.1 GitHub 저장소에서 프로젝트 클론
```bash
git clone https://github.com/your-username/your-flask-project.git
cd your-flask-project
```

---

## 📦 4. 의존성 설치

- local 환경에서 requirements.txt 생성
# 가상환경 활성화 후
```pip freeze > requirements.txt```


### 4.1 `requirements.txt`로 패키지 설치
```bash
conda activate flaskenv
pip install -r requirements.txt
```

> `requirements.txt` 예시:
```txt
Flask==2.3.2
gunicorn==21.2.0
requests
```

---

## 🚀 5. Flask 앱 Gunicorn으로 실행

Flask 같은 Python 웹 애플리케이션을 실제 운영환경에서 서비스하려면 반드시 필요한 **WAS(Web Application Server)** 역할을 해주는 도구가 바로 **Gunicorn**입니다.

---

## 🦄 Gunicorn 이란?

> **G**reen **Unicorn**의 줄임말  
> **Python WSGI 웹 서버**입니다.

---

## ✅ Gunicorn의 정체 한 줄 설명

> Python 웹 프레임워크(예: Flask, Django)를 **운영 환경에서 돌리기 위한 고성능 HTTP 서버**

> 설치 ```pip install gunicorn```
---

## 🔍 왜 필요한가?

| 개발 환경에서 쓰는 `app.run()` | 운영 환경에서 쓰는 Gunicorn |
|------------------------------|-----------------------------|
| 단일 프로세스 / 디버깅용 | 멀티 프로세스 / 병렬 처리 |
| 느리고 비안정 | 고성능 / 프로덕션용 |
| 브라우저 테스트용 | 실제 서비스용 |

 
---

### 5.1 기본 구조 예시
```python
# app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask!"
```

### 5.2 Gunicorn 백그라운드 실행 + 로그 저장
```bash
gunicorn -w 2 -b 0.0.0.0:5000 app:app \
  --daemon \
  --access-logfile gunicorn_access.log \
  --error-logfile gunicorn_error.log
```

---

## 🔍 6. 실행 확인 및 로그 보기

### 6.1 실행 확인
```bash
ps -ef | grep gunicorn
curl http://localhost:5000
```

### 6.2 실시간 로그 보기
```bash
tail -f gunicorn_access.log
tail -f gunicorn_error.log
```

---

## 🔓 7. EC2 보안 그룹에서 포트 열기 (중요)

1. AWS EC2 콘솔 접속
2. 인스턴스 → 보안 → 보안 그룹 → **인바운드 규칙 편집**
3. 포트 범위: `5000`, 프로토콜: `TCP`, 소스: `0.0.0.0/0` (모두 허용 또는 IP 제한 가능)

---

## 📌 정리 요약

| 단계 | 설명 |
|------|------|
| 시스템 준비 | Git 설치, 시스템 업데이트 |
| 환경 구성 | Miniconda 설치 및 가상환경 생성 |
| 코드 준비 | Git 클론으로 Flask 프로젝트 가져오기 |
| 실행 준비 | `requirements.txt`로 의존성 설치 |
| 실행 | Gunicorn으로 백그라운드 실행 및 로그 기록 |
| 확인 | `curl`, `ps`, `tail` 등으로 상태 확인 |
| 보안 그룹 설정 | AWS에서 5000 포트 열기 필수 |

---

# 콘다명령어 안될때(설치했고)
## 환경변수에 콘다 설치 경로 있는지 체크 
```nano ~/.bashrc ```
## 없으면 아래 추가
```export PATH="$HOME/miniconda3/bin:$PATH"```
## 저장 후 적용
``` source ~/.bashr ```