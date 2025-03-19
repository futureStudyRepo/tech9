## **Jupyter Notebook & Jupyter Lab & Colab 개요**

### **1. Jupyter Notebook**
#### ✅ **Jupyter Notebook 개요**
Jupyter Notebook은 웹 기반의 대화형 환경으로, 코드 실행, 문서 작성, 데이터 시각화 등을 한 곳에서 수행할 수 있도록 해주는 도구입니다.  
특히 **Python을 비롯한 여러 언어(커널 지원)를 실행할 수 있으며**, 데이터 분석과 머신러닝에서 많이 사용됩니다.

#### 🔹 **기본 사용법**
1. **Jupyter Notebook 실행**
   ```bash
   jupyter notebook
   ```
   실행 후, 브라우저에서 Jupyter 대시보드가 열립니다.

2. **새 노트북 생성**
   - `New → Python3 (또는 설치된 커널)`을 선택하여 새 노트북을 만듭니다.

3. **셀 실행**
   - `Shift + Enter`: 현재 셀 실행 후 다음 셀로 이동
   - `Ctrl + Enter`: 현재 셀만 실행
   - `Alt + Enter`: 현재 셀 실행 후 아래에 새로운 셀 추가

4. **Markdown 사용**
   - 코드 이외에도 `Markdown`을 활용하여 문서를 작성할 수 있습니다.
   - `셀 유형 변경: Cell → Cell Type → Markdown`
   - 예시:
     ```markdown
     # 제목
     ## 소제목
     **굵은 글씨**
     *기울임 글씨*
     - 리스트 항목 1
     - 리스트 항목 2
     ```

     
### Conda 가상환경 `tech9` 사용 방법

#### 1. **가상환경 `tech9` 활성화**
터미널(또는 Anaconda Prompt)에서 다음 명령어를 실행하세요.

```bash
conda activate tech9
```

활성화되면 프롬프트 앞에 `(tech9)`가 표시됩니다.
```bash
(tech9) user@computer:~$
```

---

#### 2. **Jupyter Notebook에서 `tech9` 환경 사용**
Jupyter Notebook에서 `tech9` 환경을 사용하려면, 환경에 `ipykernel`을 설치해야 합니다.

##### (1) `tech9` 환경에서 `ipykernel` 설치
```bash
conda install -n tech9 ipykernel
```

##### (2) Jupyter에 가상환경 등록
```bash
python -m ipykernel install --user --name=tech9 --display-name "Python (tech9)"
```

이제 Jupyter Notebook을 실행한 후, **Kernel → Change Kernel**에서 `"Python (tech9)"`을 선택하면 해당 환경을 사용할 수 있습니다.

---

#### 3. **가상환경 비활성화**
가상환경을 종료하려면 다음 명령어를 입력하세요.

```bash
conda deactivate
```

---

#### 4. **추가 확인 및 문제 해결**
- **가상환경 목록 확인**:
  ```bash
  conda env list
  ```
  `tech9` 환경이 목록에 있는지 확인하세요.

- **Jupyter Notebook이 `tech9` 환경에서 실행 중인지 확인**:
  ```bash
  which python
  ```
  또는
  ```bash
  python -c "import sys; print(sys.executable)"
  ```
  실행 결과가 `tech9` 환경의 Python 실행 경로를 가리켜야 합니다.

이제 `tech9` 가상환경을 Jupyter Notebook에서도 사용할 수 있습니다! 🚀



#### 🔹 **Jupyter Notebook 장점**
✅ **대화형 환경**: 코드를 실행하면서 즉시 결과를 확인 가능  
✅ **다양한 커널 지원**: Python뿐만 아니라 R, Julia 등 다양한 언어 지원  
✅ **Markdown 지원**: 문서 작성과 코드 실행을 하나의 환경에서 가능  
✅ **강력한 데이터 시각화**: `matplotlib`, `seaborn` 등을 활용하여 시각화 가능  

#### 🔹 **Jupyter Notebook 단점**
❌ **대규모 프로젝트 관리 어려움**: Python 스크립트(.py)처럼 모듈화가 어렵고, 코드 길어지면 유지보수 불편  
❌ **동시 작업 불가**: 하나의 커널에서 실행되므로 다중 사용자 지원이 어려움  
❌ **버전 충돌 문제**: 여러 환경에서 실행할 경우 패키지 버전 문제가 발생할 수 있음  

---

### **2. Jupyter Lab**
#### ✅ **Jupyter Lab 개요**
Jupyter Lab은 Jupyter Notebook의 업그레이드 버전으로, 더 유연한 인터페이스를 제공하는 개발 환경입니다.  
Jupyter Notebook의 기능을 포함하면서도, **탭 기반 인터페이스와 다양한 확장 기능을 제공**하여 보다 강력한 개발 환경을 제공합니다.

#### 🔹 **Jupyter Lab 실행**
1. **설치 (Jupyter Notebook과 별개로 설치 가능)**
   ```bash
   conda install -c conda-forge jupyterlab
   ```
   또는
   ```bash
   pip install jupyterlab
   ```

2. **Jupyter Lab 실행**
   ```bash
   jupyter lab
   ```
   실행하면 브라우저에서 Jupyter Lab 환경이 열립니다.

#### 🔹 **Jupyter Lab 장점**
✅ **더 강력한 인터페이스**: 파일 탐색기, 터미널, 텍스트 편집기 등을 탭 형태로 활용 가능  
✅ **코드 편집 향상**: `.py` 파일 직접 편집 가능, 다중 탭 기능 제공  
✅ **확장 기능 지원**: 플러그인 시스템을 통해 기능 추가 가능  
✅ **Jupyter Notebook과 완벽 호환**: 기존 노트북 파일 그대로 실행 가능  

#### 🔹 **Jupyter Lab 단점**
❌ **리소스 사용량 증가**: Jupyter Notebook보다 메모리 사용량이 많음  
❌ **초기 적응 필요**: UI가 다소 복잡하여 처음 사용할 때 익숙해지는 데 시간이 필요함  

---

### **3. Google Colab (Colaboratory)**
#### ✅ **Google Colab 개요**
Google Colab은 Google이 제공하는 클라우드 기반의 Jupyter Notebook 환경입니다.  
설치 없이 웹 브라우저에서 실행할 수 있으며, GPU 및 TPU를 무료로 사용할 수 있다는 점이 큰 장점입니다.

#### 🔹 **Google Colab 사용법**
1. **Colab 접속**  
   👉 [Google Colab](https://colab.research.google.com/) 에 접속  
   (Google 계정 필요)

2. **새 노트북 생성**  
   - `파일 → 새 노트` 선택

3. **Python 실행**  
   - Jupyter Notebook과 동일하게 `Shift + Enter`로 셀 실행

4. **Google Drive 연동**  
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
   실행하면 Google Drive의 데이터를 Colab에서 직접 사용할 수 있음.

#### 🔹 **Google Colab 장점**
✅ **설치 필요 없음**: 웹 브라우저에서 바로 실행 가능  
✅ **무료 GPU/TPU 지원**: 딥러닝 학습에 매우 유용  
✅ **Google Drive 연동 가능**: 파일 저장 및 불러오기 용이  
✅ **공유 및 협업 용이**: Google Docs처럼 다른 사용자와 공유 가능  

#### 🔹 **Google Colab 단점**
❌ **제한된 컴퓨팅 리소스**: 무료 GPU 사용 시간이 제한됨  
❌ **인터넷 연결 필요**: 로컬에서 오프라인 실행 불가  
❌ **환경 설정 유지 어려움**: 런타임이 초기화되면 모든 설치된 패키지가 사라짐  

---

## **🔍 Jupyter Notebook vs. Jupyter Lab vs. Google Colab 비교**
| 기능 | Jupyter Notebook | Jupyter Lab | Google Colab |
|------|-----------------|-------------|-------------|
| **설치 필요 여부** | 필요 | 필요 | 필요 없음 (클라우드 기반) |
| **인터페이스** | 단순 | 고급 (탭 및 확장 지원) | 웹 기반 (단순) |
| **확장성** | 제한적 | 확장 가능 (플러그인 지원) | 제한적 |
| **GPU 지원** | 없음 | 없음 | 무료 GPU/TPU 지원 |
| **파일 관리** | 제한적 | 강력한 파일 관리 가능 | Google Drive 연동 가능 |
| **협업 기능** | 제한적 | 제한적 | 실시간 협업 가능 |
| **사용 용도** | 기본적인 데이터 분석, 머신러닝 | 고급 개발 환경 | 클라우드 기반 머신러닝 |

---

## **📌 정리**
- **Jupyter Notebook** → Python 코드를 대화형으로 실행하며 문서와 함께 정리하기 좋은 환경  
- **Jupyter Lab** → Jupyter Notebook의 업그레이드 버전으로, 다중 파일 작업과 확장 기능을 지원  
- **Google Colab** → 클라우드 기반 Jupyter 환경으로, 무료 GPU/TPU 지원 및 협업 가능  

---

### **✅ 언제 어떤 걸 사용하면 좋을까?**
- **Python 학습 및 간단한 데이터 분석** → Jupyter Notebook
- **대규모 프로젝트 관리, 다중 파일 편집** → Jupyter Lab
- **무료 GPU/TPU 활용, 클라우드 환경에서 작업** → Google Colab

각 도구의 특징을 잘 이해하고 목적에 맞게 활용하면 효율적인 개발 및 연구 환경을 만들 수 있습니다! 🚀