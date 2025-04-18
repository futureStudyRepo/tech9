# 🌟 크롬 확장 프로그램 만들기: 화면 캡처 확장 
## 📁 프로젝트 구성도
```plaintext
📂 capture-extension/
├── manifest.json          # 확장 프로그램의 설정 파일
├── background.js          # 탭 상태를 감지하고 content script 삽입
├── content.js             # 웹 페이지에서 실행되는 코드
├── popup.html             # 버튼 UI 페이지
├── popup.js               # 버튼을 눌렀을 때의 동작 정의
└── icon.png               # 브라우저에 표시될 아이콘
```

---

## 1. 📘 manifest.json – 확장 프로그램 설명서

```json
{
  "manifest_version": 3,
  "name": "화면캡처!",
  "version": "1.0",
  "description": "Capture !!! Screen",
  "permissions": ["tabs", "activeTab", "scripting"],
  "action": {
    "default_icon": "icon.png",
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

### 📌 주요 필드 설명

| 필드명 | 설명 |
|--------|------|
| `manifest_version` | 반드시 3으로 설정 (현재 최신 버전) |
| `name`, `description`, `version` | 확장 프로그램 이름, 설명, 버전 정보 |
| `permissions` | `tabs`, `activeTab`, `scripting`은 화면 캡처 및 스크립트 실행을 위해 필요 |
| `action` | 브라우저 상단 아이콘 클릭 시 보여줄 UI 정의 |
| `background.service_worker` | 탭 상태 감지 등 백그라운드 작업 처리 |
| `content_scripts` | 웹페이지에 자동 삽입될 자바스크립트 정의 |

---

## 2. 🧠 background.js – 탭 상태 감지 & content.js 삽입

```javascript
chrome.runtime.onInstalled.addListener(() => {
  chrome.tabs.onUpdated.addListener((tabId, changeInfo, tab) => {
    if (changeInfo.status === "complete" && tab.url) {
      chrome.scripting.executeScript({
        target: { tabId: tabId },
        files: ["content.js"]
      })
      .then(() => console.log("content script successfully"))
      .catch((error) => console.log("fail", error));
    }
  });
});
```

- 탭이 새로 로딩되면 `content.js`를 삽입
- 설치된 순간부터 이벤트 리스너가 작동

---

## 3. 🧩 content.js – 웹페이지 안에 삽입되는 코드

```javascript
console.log("content script running");
```

- 간단한 로그로 content script가 잘 삽입되는지 확인 가능
- 향후 DOM 조작도 여기서 가능

---

## 4. 🖼 popup.html – UI 만들기

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Capture extension</title>
</head>
<body>
  <button id="capture">Capture Screen</button>
  <script src="popup.js"></script>
</body>
</html>
```

- 아이콘 클릭 시 뜨는 팝업 화면 정의
- "화면 캡처" 버튼 포함

---

## 5. ⚙ popup.js – 버튼 누르면 화면 저장

```javascript
function onCaptureBtnClick() {
  chrome.tabs.captureVisibleTab(null, { format: "png" }, function(image) {
    const link = document.createElement("a");
    link.href = image;
    link.download = "screenshot.png";
    link.click();
  });
}

document.getElementById("capture").addEventListener('click', onCaptureBtnClick);
```

- `chrome.tabs.captureVisibleTab`을 통해 현재 보이는 화면 캡처
- 자동으로 다운로드 실행

---

## 6. 🧷 icon.png – 아이콘 이미지

- `action.default_icon`으로 연결
- 확장 도구 모음에 표시됨

---

## 🖥 설치 방법 (개발자 모드)

1. **크롬 브라우저에서 주소창에** `chrome://extensions/` 입력
2. 우측 상단 **[개발자 모드]** ON
3. **[압축해제된 확장 프로그램 로드]** 클릭
4. 해당 프로젝트 폴더(`capture-extension`) 선택
5. 브라우저 우측 상단에 아이콘 생기면 성공

---

## ⚠ 크롬 확장 개발 시 주의사항

| 항목 | 설명 |
|------|------|
| 🔐 권한 설정 | `permissions` 항목에 필요한 권한을 명시하지 않으면 작동 안 함 |
| 🚫 보안 정책 | `manifest_version: 3`에서는 `background.js`가 아닌 `service_worker` 사용 |
| ⏱ 설치 후 반응 없음 | 확장 설치 후 탭을 **새로고침해야** background와 content script가 적용됨 |
| 📦 폴더 구조 유지 | `manifest.json`, js 파일, 아이콘 이미지 등 모두 **같은 폴더 안**에 있어야 함 |
| 🧪 콘솔 확인 | `content.js`, `popup.js` 내부 `console.log`는 각각 웹페이지 / 팝업의 개발자도구에서 확인 |

---

## 💡 확장 기능 확장 아이디어

- 캡처한 이미지를 서버에 업로드
- 선택 영역만 캡처 (마우스로 드래그)
- OCR 기능 연결 (텍스트 추출)

---

## ✅ 마무리 체크리스트

- [x] `manifest.json` 구조 이해
- [x] 각 스크립트의 역할 알기 (`background`, `content`, `popup`)
- [x] 크롬에 직접 설치해 보기
- [x] 화면 캡처 동작 확인

---
