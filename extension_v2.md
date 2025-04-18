
---

# 🧠 크롬 확장 프로그램 만들기: OCR로 화면 텍스트 추출하기

> 이 확장 프로그램은 웹 화면을 캡처한 뒤, **서버로 이미지를 전송해 텍스트(OCR)**를 추출하고 결과를 팝업에 표시합니다.

---

## 📁 폴더 구조

```plaintext
📂 capture-ocr-extension/
├── manifest.json           # 확장 프로그램 설정
├── background.js           # content script 동작 감지
├── content.js              # 웹 페이지 삽입 스크립트
├── popup.html              # UI 팝업
├── popup.js                # 캡처 및 OCR 전송 로직
└── icon-96x96.png          # 아이콘 이미지
```

---

## 1. 📘 manifest.json – 확장 설정

```json
{
  "manifest_version": 3,
  "name": "텍스트 추출!!!",
  "version": "1.0",
  "description": "Capture !!! Screen OCR",
  "permissions": ["tabs","activeTab", "scripting"],
  "action": {
    "default_icon":"icon-96x96.png",
    "default_popup": "popup.html"
  },
  "background": {
    "service_worker": "background.js"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ]
}
```

### ✅ 핵심 포인트

| 항목 | 설명 |
|------|------|
| `tabs`, `scripting` | 탭에서 화면을 캡처하고 스크립트 삽입 |
| `background.service_worker` | 탭 상태 감지용 백그라운드 서비스 워커 |
| `default_popup` | 확장 아이콘 클릭 시 보여줄 HTML |

---

## 2. ⚙ background.js – 탭 상태 감지 & content.js 삽입

```javascript
chrome.runtime.onInstalled.addListener(() => {
  chrome.tabs.onUpdated.addListener((tabId, changeInfo, tab) => {
    if (changeInfo.status === "complete" && tab.url) {
      chrome.scripting.executeScript({
        target: { tabId: tabId },
        files: ["content.js"]
      })
      .then(() => console.log("content script successfully"))
      .catch((error) => console.log("faile", error));
    }
  });
});
```

- 탭이 로딩 완료될 때마다 content.js 삽입

---

## 3. 🧩 content.js – 웹페이지에 삽입되는 코드

```javascript
console.log("content script running");
```

- 기본 로그 출력으로 삽입 확인
- 추후 DOM 텍스트 처리 가능

---

## 4. 🖼 popup.html – OCR UI 구성

```html
<body>
  <button id="capture">Capture Screen</button>
  <div id="result-container">
      <h3>OCR 결과:</h3>
      <p id="ocr-result">결과가 여기에 표시됩니다.</p>
  </div>
  <script src="popup.js"></script>
</body>
```

- 버튼을 누르면 화면을 캡처하고 OCR 서버에 요청
- 결과는 아래 박스에 텍스트 형태로 표시됨

---

## 5. 📤 popup.js – 서버로 이미지 전송 & 결과 표시

```javascript
function onCaptureBtnClick(){
  chrome.tabs.captureVisibleTab(null, {format:"png"}, function(image){
    fetch("http://localhost:5000/upload", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ image: image })
    })
    .then(res => res.json())
    .then(data => {
      document.getElementById("ocr-result").innerHTML = data.result;
    })
    .catch(err => console.log("error", err));
  });
}
document.getElementById("capture").addEventListener('click', onCaptureBtnClick);
```

- 캡처한 이미지를 Base64로 전송 (`image`)
- Flask 등 백엔드 서버에서 OCR 수행 후 `{"result": "...텍스트..."}` 형태로 응답

---

## 6. 🧪 Flask 서버 예시 (선택사항)

```python
@app.route('/upload', methods=['POST'])
def upload():
    data = request.get_json()
    image_data = data['image'].split(",")[1]
    result = ""
    # result 결과 생성 및 전달
    return jsonify({"result": " ".join(result)})

app.run(port=5000)
```

---

## 🖥 설치 방법 (개발자 모드)

1. `chrome://extensions/` 접속
2. 우측 상단 **개발자 모드** 활성화
3. **[압축해제된 확장 프로그램 로드]** → 해당 폴더 선택
4. 아이콘 클릭 후 **[Capture Screen]** → 결과 확인

---

## ⚠ 주의 사항

| 항목 | 설명 |
|------|------|
| 📡 서버 필요 | `http://localhost:5000/upload` 주소에 Flask 서버 실행 중이어야 함 |
| 🔐 보안 정책 | `manifest_version: 3`에서는 반드시 `service_worker` 사용 |
| 📦 크롬 캐시 | 수정 후에는 항상 확장 프로그램 **삭제 후 재설치** 또는 **[다시 로드]** |
| 🌐 Cross-Origin 문제 | 서버에서 CORS 허용 필요 (Flask에서는 `flask-cors` 사용 가능) |

---

## ✅ 체크리스트

- [x] 캡처 버튼을 누르면 화면을 이미지로 저장하는지?
- [x] 서버에서 이미지 전송을 받아 OCR을 수행하는지?
- [x] 팝업에 결과가 잘 출력되는지?

---
