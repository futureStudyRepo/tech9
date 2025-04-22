
# 리눅스 기본 명령어 정리 (Debian 계열 기준)

## 1. 시스템 정보 확인

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `uname -a` | 커널 및 시스템 정보 전체 출력 | `uname -a` |
| `hostname` | 시스템의 호스트 이름 확인 | `hostname` |
| `uptime` | 시스템 실행 시간 및 부하 확인 | `uptime` |
| `top` / `htop` | 실시간 시스템 리소스 확인 | `top` |
| `free -h` | 메모리 사용량 확인 | `free -h` |
| `df -h` | 디스크 사용량 확인 | `df -h` |
| `whoami` | 현재 사용자 확인 | `whoami` |
| `id` | 사용자 ID 및 그룹 확인 | `id` |

---

## 2. 파일 및 디렉토리 작업

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `ls` | 폴더 내용 보기 | `ls` |
| `ls -l` | 상세 정보 포함 목록 보기 | `ls -l` |
| `cd` | 디렉토리 이동 | `cd /home/user` |
| `pwd` | 현재 경로 출력 | `pwd` |
| `mkdir` | 폴더 생성 | `mkdir new_folder` |
| `touch` | 빈 파일 생성 | `touch file.txt` |
| `cp` | 파일 복사 | `cp a.txt b.txt` |
| `mv` | 파일 이동/이름 변경 | `mv old.txt new.txt` |
| `rm` | 파일 삭제 | `rm file.txt` |
| `rm -r` | 폴더 및 내용 삭제 | `rm -r folder/` |
| `cat` | 파일 내용 출력 | `cat notes.txt` |
| `nano`, `vi` | 텍스트 편집기 실행 | `nano file.txt` |

---

## 3. 사용자 및 권한

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `adduser` | 사용자 추가 | `sudo adduser john` |
| `passwd` | 비밀번호 설정 | `passwd john` |
| `su` | 사용자 전환 | `su john` |
| `sudo` | 관리자 권한 실행 | `sudo apt update` |
| `chmod` | 권한 설정 | `chmod 755 script.sh` |
| `chown` | 소유자 변경 | `chown user:group file.txt` |

---

## 4. 패키지 관리 (APT)

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `sudo apt update` | 패키지 목록 갱신 | `sudo apt update` |
| `sudo apt upgrade` | 설치된 패키지 업그레이드 | `sudo apt upgrade` |
| `sudo apt install` | 패키지 설치 | `sudo apt install nginx` |
| `sudo apt remove` | 패키지 제거 | `sudo apt remove nginx` |
| `sudo apt autoremove` | 불필요한 패키지 자동 제거 | `sudo apt autoremove` |

---

## 5. 네트워크 관련

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `ip a` | IP 주소 확인 | `ip a` |
| `ping` | 연결 테스트 | `ping google.com` |
| `curl` | HTTP 요청 | `curl http://example.com` |
| `wget` | 파일 다운로드 | `wget http://example.com/file.zip` |
| `netstat -tuln` | 포트 상태 확인 | `sudo netstat -tuln` |

---

## 6. 프로세스 및 작업 제어

### 프로세스 확인

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `ps -ef` | 전체 프로세스 상세 보기 | `ps -ef` |
| `ps aux` | BSD 스타일 프로세스 보기 | `ps aux` |
| `ps -ef` | grep [이름]` | 특정 프로세스 필터 | `ps -ef` | grep nginx` |
| `pgrep` | 프로세스 이름으로 PID 찾기 | `pgrep python` |
| `pgrep -af` | 전체 명령어 포함 검색 | `pgrep -af python` |

### 프로세스 종료

| 명령어 | 설명 | 예시 | `sudo` 필요 여부 |
|--------|------|------|------------------|
| `kill [PID]` | PID로 종료 | `kill 1234` | 자신이 실행한 프로세스만 가능 |
| `kill -9 [PID]` | 강제 종료 | `kill -9 1234` | 필요시 `sudo` |
| `killall [이름]` | 이름 기반 종료 | `killall python` | 필요시 `sudo` |
| `pkill [이름]` | 이름 기반 종료 (pgrep과 반대) | `pkill flask` | 필요시 `sudo` |
| `xkill` | GUI에서 마우스로 창 종료 | `xkill` | X (GUI 환경만) |

> `sudo` 없이 가능한 경우: 자신이 실행한 프로세스일 때  
> 시스템 프로세스, 다른 사용자 프로세스는 `sudo` 필요

### 실전 예제

```bash
ps -ef | grep '[n]ode'           # node 프로세스 필터링 (grep 제외)
kill $(pgrep -f "node app.js")   # node app.js 프로세스 종료
```

---

## 7. 압축 및 해제

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `tar -czvf` | 폴더를 gzip으로 압축 | `tar -czvf file.tar.gz folder/` |
| `tar -xzvf` | tar.gz 압축 해제 | `tar -xzvf file.tar.gz` |
| `zip -r` | zip 압축 | `zip -r file.zip folder/` |
| `unzip` | zip 압축 해제 | `unzip file.zip` |

---

## 8. 로그 및 시스템 모니터링

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `dmesg` | 커널 메시지 출력 | `dmesg | tail` |
| `journalctl` | 시스템 로그 확인 | `journalctl -xe` |
| `tail -f` | 로그 실시간 보기 | `tail -f /var/log/syslog` |

---

## 9. 기타 유용한 명령어

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `history` | 명령어 히스토리 보기 | `history` |
| `alias` | 명령어 별칭 설정 | `alias ll='ls -l'` |
| `clear` | 터미널 화면 지우기 | `clear` |
| `man` | 명령어 설명서 | `man grep` |

---

---

# 🐧 Tomcat 설치 및 정적 페이지 실행 가이드 (Ubuntu 기준)

## 🔐 기본 접속 정보
```bash
IP: 192.168.0.44
ID / PW: 각자 이니셜
```

---

## 📁 기본 리눅스 명령어

| 명령어 | 설명 |
|--------|------|
| `ll` 또는 `pwd` | 현재 폴더 내용 / 위치 확인 |
| `mkdir mytomcat` | 폴더 생성 |
| `rm -r mytomcat` | 폴더 삭제 |
| `cd ~` | 홈 디렉토리 이동 |

---

## 📥 Tomcat 다운로드 및 설치

```bash
cd ~
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.104/bin/apache-tomcat-9.0.104.tar.gz --no-check-certificate
tar -xvzf apache-tomcat-9.0.104.tar.gz
mv apache-tomcat-9.0.104 mytomcat
```

---

## ⚙️ 포트 확인 및 변경

### 현재 열려있는 포트 확인
```bash
ss -tuln
```

### 포트 변경
```bash
nano ~/mytomcat/conf/server.xml
```

#### 📝 nano 에디터 사용법
- 저장: `Ctrl + O` → `Enter`
- 종료: `Ctrl + X`

---

## 🌐 정적 웹페이지 생성 및 배포

```bash
cd ~/mytomcat/webapps/ROOT/
echo "<h1>Hello, I'm LAG</h1>" > index.html
```

---

## 🚀 Tomcat 실행

### Java 설치 확인
```bash
java -version
```

### Tomcat 실행
```bash
~/mytomcat/bin/startup.sh
```

### 웹 브라우저 접속
```
http://192.168.0.44:할당받은포트
```

---

## 🔍 프로세스 관리

### 프로세스 확인
```bash
ps -ef | grep java
ps -ef | grep 포트번호
```

### 프로세스 종료
```bash
kill PID
```

---

## 🧪 Spring Boot JAR 실행

> Java Runtime Environment(JRE) 설치되어 있어야 함

```bash
java -jar letter-0.0.1-SNAPSHOT.jar --server.port=8093
```

---
