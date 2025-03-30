Oracle 18c에서 특정 계정의 덤프 파일을 생성하고 임포트하기 위한 과정은 크게 다음 단계로 나눌 수 있습니다:

1. Data Pump 디렉토리 생성
2. 덤프 파일 생성 (Export)
3. 덤프 파일 임포트 (Import)

## 1. Data Pump 디렉토리 생성

Oracle에서 Data Pump를 사용하여 덤프 파일을 저장하기 위해서는 미리 Oracle 디렉토리 오브젝트를 생성해야 합니다. 이 디렉토리는 Oracle이 읽고 쓸 수 있는 파일 시스템 경로와 연결되어야 합니다. 

### 디렉토리 생성 명령
```sql
CREATE OR REPLACE DIRECTORY sql_dir AS 'C:\sql_backup\';
```

- `C:\sql_backup\`: 덤프 파일을 저장할 실제 파일 시스템 경로입니다. 해당 경로에 Oracle 서버 프로세스가 읽고 쓸 수 있는 권한이 있어야 합니다.



### 권한 부여
Oracle 계정에 생성된 디렉토리를 사용할 수 있는 권한을 부여해야 합니다.

```sql
    GRANT READ, WRITE ON DIRECTORY sql_dir TO jdbc;
```

- `jdbc`: 덤프 파일을 생성할 계정 또는 임포트할 계정.



## 2. 덤프 파일 생성 (Export)

다음은 특정 스키마(계정)의 데이터를 덤프 파일로 내보내는 `expdp` 명령입니다.

### 덤프 파일 생성 명령
```bash
expdp system/oracle@xe schemas=jdbc directory=sql_dir dumpfile=jdbc_2025.dmp logfile=jdbc_2025.log
```

- `system/oracle@xe`: Oracle 데이터베이스의 관리자 계정과 서비스 이름.
- `schemas=jdbc`: 덤프 파일을 생성할 스키마 이름.d
- `directory=sql_dir`: 앞서 생성한 Oracle 디렉토리 이름.
- `dumpfile=jdbc_2025.dmp`: 생성할 덤프 파일 이름.
- `logfile=jdbc_2025.log`: 내보내기 작업 로그를 저장할 파일 이름.

### 예시:

![dump](./img/dump1.png)


## 3. 덤프 파일 임포트 (Import)

내보낸 덤프 파일을 특정 계정으로 임포트하려면 `impdp` 명령을 사용합니다.

### 덤프 파일 임포트 명령(받는 곳에서도 디렉토리가 있어야함, 아래 명령어는 덤프파일이 있는 경로에서 실행)
```bash
impdp system/oracle@xe schemas=jdbc directory=sql_dir dumpfile=jdbc_2025.dmp logfile=jdbc_2025.log
```

- `schemas=jdbc`: 데이터를 임포트할 스키마 이름.
- `dumpfile=jdbc_2025.dmp`: 내보낸 덤프 파일 이름.
- `logfile=jdbc_2025.log`: 임포트 작업 로그를 저장할 파일 이름.


## 4. 추가 참고 사항

- **Oracle 디렉토리 확인**: 이미 존재하는 Oracle 디렉토리를 확인하려면 다음 SQL을 실행할 수 있습니다.
  ```sql
  SELECT directory_name, directory_path FROM dba_directories;
  ```

- **데이터 펌프 작업 모니터링**: 내보내기 또는 임포트 작업의 진행 상황을 모니터링하려면 Oracle SQL*Plus에서 다음 명령을 사용하여 작업 상태를 확인할 수 있습니다.
  ```sql
  SELECT * FROM dba_datapump_jobs;
  ```

이 과정을 통해 Oracle 18c에서 특정 계정의 덤프 파일을 생성하고 임포트할 수 있습니다.


---

## 도커 oracle 18c 덤프 생성 및 복사

# Oracle 데이터 덤프(Dump) 파일 도커에서 로컬로 복사하는 과정

이 가이드는 Oracle 데이터베이스를 도커 컨테이너에서 Data Pump를 이용해 덤프 파일(.DMP)로 내보낸 후, 해당 파일을 로컬로 복사하는 과정을 설명합니다.


## 1. **도커 컨테이너 접속**
도커 컨테이너에 `oracle` 사용자로 접속합니다.

```bash
docker exec -it --user=oracle oracle18c bash
```

- `oracle18c`: 도커 컨테이너 이름입니다.

---

## 2. **SQL*Plus 실행**
컨테이너 내부에서 SQL*Plus를 실행하여 데이터펌프에 필요한 설정을 합니다.

```bash
sqlplus
```

- **로그인 정보:**
  ```sql
  사용자명: system
  비밀번호: oracle
  ```

---

## 3. **Data Pump에 사용할 디렉토리 객체 생성**
Oracle에서 데이터 내보내기(Data Pump)가 진행될 디렉토리 객체를 생성하고 권한을 부여합니다.

- 디렉토리 객체 생성:
  ```sql
  CREATE OR REPLACE DIRECTORY PROJ AS '/home/oracle';
  ```

- 사용자에게 읽기/쓰기 권한 부여:
  ```sql
  GRANT READ, WRITE ON DIRECTORY PROJ TO proj;
  ```

---

## 4. **Data Pump Export 실행**
`expdp` 명령어를 사용하여 데이터베이스를 덤프 파일로 내보냅니다.

```bash
expdp proj/proj DIRECTORY=PROJ DUMPFILE=PROJ.DMP LOGFILE=export.log
```

- **매개변수 설명:**
  - `proj/proj`: 사용자 이름과 비밀번호.
  - `DIRECTORY=PROJ`: 앞에서 생성한 디렉토리 객체 이름.
  - `DUMPFILE=PROJ.DMP`: 내보낼 덤프 파일 이름.
  - `LOGFILE=export.log`: 로그 파일 이름.

---

## 5. **덤프 파일 로컬로 복사**
`docker cp` 명령어를 이용하여 도커 컨테이너에서 로컬 머신으로 덤프 파일을 복사합니다.

```bash
docker cp e02efa2bf21e:/home/oracle/PROJ.DMP /home/server3
```

- **매개변수 설명:**
  - `e02efa2bf21e`: 도커 컨테이너 ID.
  - `/home/oracle/PROJ.DMP`: 도커 컨테이너 내부 덤프 파일 경로.
  - `/home/server3`: 로컬 머신에서 파일이 저장될 경로.

---

## 6. **복사된 파일 확인**
로컬 머신에서 복사된 파일을 확인합니다.

```bash
ls /home/server3
```

---

## 문제 해결
1. **권한 문제**:  
   도커 컨테이너 내부 디렉토리(`/home/oracle`)에 `oracle` 사용자가 읽기/쓰기 권한을 가지고 있는지 확인하세요.
   ```bash
   chmod 775 /home/oracle
   ```

2. **디렉토리 경로 문제**:  
   Data Pump에서 사용하는 Oracle 디렉토리 객체가 실제 컨테이너의 디렉토리 경로와 일치하는지 확인하세요.

3. **파일 경로 문제**:  
   `docker cp` 명령어에서 경로를 정확히 지정했는지 확인하세요.

---