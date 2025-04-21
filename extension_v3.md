
---

# 🌍 크롬 확장 프로그램 만들기: 드래그 영역 번역기 (OCR + Translation)

> 드래그로 영역을 선택해 해당 부분을 이미지로 캡처하고,  
> **Flask 서버로 전송**하여 번역된 텍스트를 알림으로 보여줍니다.

---

## 📁 폴더 구조

```plaintext
📂 drag-ocr-translate-extension/
├── manifest.json          # 확장 설정
├── background.js          # 메시지 수신 및 캡처
├── content.js             # 선택 영역 감지 및 캡처
├── popup.html             # 실행 버튼 UI
├── popup.js               # 버튼 클릭 시 드래그 시작
└── icon-96x96.png         # 확장 아이콘
```

---

## 1. 📘 manifest.json – 확장 프로그램 설정

```json
{
  "manifest_version": 3,
  "name": "번역!!",
  "version": "1.0",
  "description": "Translation !!",
  "permissions": ["tabs", "activeTab", "scripting"],
  "action": {
    "default_icon": "icon-96x96.png",
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

---

## 2. ⚙ background.js – 화면 전체 캡처 응답 처리

```javascript
chrome.runtime.onMessage.addListener((req, sender, res) => {
  if (req.action === "captureVisibleTab") {
    chrome.tabs.captureVisibleTab((imageUri) => {
      res({ imageUri });
    });
    return true;
  }
});
```

- content.js에서 메시지를 보내면 해당 탭을 캡처해 이미지 URI로 반환

---

## 3. 🧩 content.js – 드래그 영역 선택 및 OCR 처리

```javascript
var startX, startY, endX, endY;
var selectionBox;
var isDragging = false;

// 메시지 수신 대기
chrome.runtime.onMessage.addListener((req)=>{
     if(req.action === 'startDrag'){
        document.body.style.cursor = 'crosshair';
        document.addEventListener('mousedown', startSelection);
        document.addEventListener('mousemove', drawSelection);
        document.addEventListener('mouseup', endSelection);
     }
});

function startSelection(event){
    isDragging = true;
    startX = event.clientX + window.scrollX;
    startY = event.clientY + window.scrollY;

    // 선택 상자 초기화
    selectionBox = document.createElement('div');
    selectionBox.style.position = 'absolute';
    selectionBox.style.border = '2px dashed blue';
    selectionBox.style.backgroundColor = 'rgba(0, 0, 255, 0.3)';
    selectionBox.style.left = `${startX}px`;
    selectionBox.style.top = `${startY}px`;
    selectionBox.style.pointerEvents ='none';
    document.body.appendChild(selectionBox);
}
function drawSelection(event){
    if(!isDragging)return;
    endX = event.clientX + window.scrollX;
    endY = event.clientY + window.scrollY;

    selectionBox.style.width = `${Math.abs(endX - startX)}px`;
    selectionBox.style.height = `${Math.abs(endY - startY)}px`;
    selectionBox.style.left = `${Math.min(startX, endX)}px`;
    selectionBox.style.top = `${Math.min(startY, endY)}px`;
}
function endSelection(){
    if(!isDragging)return;
    isDragging = false;
    try{
        // 선택 영역 캔버스에 그려서 자른 이미지를 전송!
        chrome.runtime.sendMessage({action:"captureVisibleTab"},(res)=>{
               console.log(res);
              if(chrome.runtime.lastError || !res.imageUri){
                  console.log("캡처 요청 실패:", chrome.runtime.lastError);
                  return;
              }
              const imageUri = res.imageUri;
              // 이미지를 로드하여 선택 영역으로 자름
              let image = new Image();
              image.src = imageUri;
              image.onload = ()=>{
                   const canvas = document.createElement('canvas');
                   const ctx = canvas.getContext('2d');
                   // 선택 영역 크기로 캔버스에 그림
                   const w = Math.abs(endX - startX);
                   const h = Math.abs(endY - startY);
                   canvas.width = w;
                   canvas.height =h;
                   const x = Math.min(startX, endX);
                   const y = Math.min(startY, endY);
                   ctx.drawImage(image, x , y , w, h, 0, 0, w, h);
                   //서버로 전송
                   canvas.toBlob((blob)=>{
                         const formData = new FormData();
                         formData.append('image', blob, 'capture.png');
                         fetch("http://localhost:5000/ocr",{
                            method:"POST",
                            body:formData
                         })
                         .then(res=>res.json())
                         .then(data =>{
                            alert("번역 내용:" + data.translation);
                         })
                         .catch(e => console.error("error:",e));
                   }, 'image/png');
              };
        });
    }finally{
        document.body.style.cursor = 'default';
        if(selectionBox){
            document.body.removeChild(selectionBox);
            selectionBox = null;
        }
        document.removeEventListener('mousedown', startSelection);
        document.removeEventListener('mousemove', drawSelection);
        document.removeEventListener('mouseup', endSelection);
    }
}
```

- 마우스 드래그로 시작점과 끝점을 기록하고 사각형 표시
- 드래그 종료 시, 전체 화면을 캡처하고 선택 영역만 잘라냄
- 잘라낸 이미지를 서버로 전송 (`/ocr` endpoint)  
- 서버 응답(`data.translation`)을 `alert()`으로 표시

---

## 4. 🖼 popup.html – 버튼 UI 구성

```html
<body>
  <button id="start-drag">영역 선택 및 캡처</button>
  <script src="popup.js"></script>
</body>
```

- 버튼 하나로 구성된 간단한 팝업

---

## 5. 🎮 popup.js – content script 실행 및 메시지 전달

```javascript
document.getElementById("start-drag").addEventListener('click', () => {
  chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    chrome.scripting.executeScript({
      target: { tabId: tabs[0].id },
      files: ['content.js']
    }).then(() => {
      chrome.tabs.sendMessage(tabs[0].id, { action: 'startDrag' });
    }).catch((e) => console.error("failed", e));
  });
});
```

- 버튼을 클릭하면 content.js를 삽입하고, "드래그 시작" 메시지 전송

---

## 6. 🔧 Flask 서버 예시

```python
from flask import Flask, request, jsonify
from PIL import Image
import easyocr
from googletrans import Translator
from io import BytesIO

app = Flask(__name__)
reader = easyocr.Reader(['en', 'ko'])
translator = Translator()

@app.route("/ocr", methods=["POST"])
def ocr_translate():
    return jsonify({"translation": "결과 전달"})

app.run(port=5000)
```
---

## 🧪 테스트 방법

1. **Flask 서버**(`localhost:5000`) 실행
2. `chrome://extensions/` → 개발자 모드 ON → 확장 프로그램 로드
3. 웹페이지에서 확장 아이콘 클릭 → **[영역 선택 및 캡처]**
4. 드래그 후 마우스 놓기 → 번역 결과 `alert` 창 표시

---

## ⚠ 주의사항

| 항목 | 설명 |
|------|------|
| 🔐 `scripting` 권한 필요 | manifest.json에 반드시 포함 |
| 🌐 Flask 서버 필요 | `http://localhost:5000/ocr` 경로에서 응답해야 함 |
| 📌 드래그 좌표는 화면 스크롤 반영 | `window.scrollX`, `scrollY` 포함됨 |
| 🚫 CORS 문제 해결 필요 | Flask에서 `flask-cors` 등 설정 추천 |

---

## ✅ 체크리스트

- [x] 드래그 기능이 작동하는지?
- [x] 선택 영역이 잘려서 서버로 전송되는지?
- [x] 번역 결과가 잘 뜨는지?

---

