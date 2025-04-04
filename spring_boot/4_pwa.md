# 조합: Spring Boot + Thymeleaf + JAR 배포 + Lombok + Web Push API
## PWA(Progressive Web App) 기능 구현 
## ✅ 1. **`pwa` 프로젝트**  
> **조합:** Spring Boot + Thymeleaf + JAR 배포 + Lombok + Web Push API

---

### 📦 `pwa` 프로젝트 스택 요약

| 분류 | 내용 |
|------|------|
| **Spring Boot 버전** | 3.3.5 |
| **JDK 버전** | 17 |
| **빌드 툴** | Maven |
| **웹 프레임워크** | Spring Web (`spring-boot-starter-web`) |
| **템플릿 엔진** | Thymeleaf (`spring-boot-starter-thymeleaf`) |
| **Lombok** | 사용 (`optional`) – 개발 편의 |
| **개발 편의 도구** | Spring Boot Devtools (자동 재시작) |
| **테스트 도구** | `spring-boot-starter-test` |
| **웹 푸시 구현** | `web-push` 라이브러리 (Martijn Dwars) |
| **암호화 관련** | Bouncy Castle (`bcprov-jdk15on`, 버전 1.70) 2회 중복 선언됨 ❗ |
| **PWA 특징** | Web Push API, Service Worker를 Spring에서 구현하는 구조 추정 |

---

### 🔍 특징 분석

- **PWA 구현 목적**:  
  `nl.martijndwars:web-push` 라이브러리를 사용하여 **Web Push Notification** 기능 구현 → 브라우저와 푸시 서버 간 VAPID 기반 암호화 지원  

- **암호화 라이브러리**:  
  Bouncy Castle (`bcprov-jdk15on`)은 푸시 메시지의 서명과 암호화에 필수적인 의존성입니다. 다만 **중복 선언**되어 있으므로 하나는 제거해도 무방합니다.

- **데이터베이스 미사용**:  
  DB 관련 의존성 (`JPA`, `JDBC`, `MyBatis`)이 없기 때문에 DB 연동은 없거나 외부 API 기반의 상태 관리 가능성도 있음

- **Lombok & Devtools**:  
  개발 생산성 및 코드 간결화에 집중된 구성

---

### ⚠️ 개선 포인트

| 항목 | 설명 |
|------|------|
| ✅ Bouncy Castle 중복 | `bcprov-jdk15on`이 2번 선언됨 → 하나는 삭제 가능 |
| ⚠️ WebPush 보안키 설정 | `application.properties`에 VAPID public/private key 설정이 필요할 수 있음 |
| 📦 DB 미구현 여부 확인 | DB가 필요한 경우 추후 `JPA` 또는 `MyBatis` 추가 필요 |

---


## PWA(Progressive Web App)란?
PWA는 웹앱에 모바일 앱처럼 설치 및 오프라인 지원 기능을 제공하여 사용자 경험을 향상시키는 기술입니다. 웹앱을 PWA로 구성하면 사용자들은 홈 화면에 앱을 추가하거나 네이티브 앱처럼 사용할 수 있습니다.
---

### ✅ PWA 핵심 특징


1. **오프라인 지원 (Offline Support)**  
   - 캐시된 자원을 통해 오프라인에서도 웹앱이 실행될 수 있게 합니다.
   - 네트워크가 불안정하거나 연결되지 않았을 때 사용자는 최소한의 콘텐츠를 확인할 수 있습니다.

2. **홈 화면 추가 (Add to Home Screen)**  
   - PWA는 모바일 장치의 홈 화면에 추가될 수 있어, 네이티브 앱처럼 쉽게 접근할 수 있습니다.
   - Manifest 파일과 서비스 워커를 설정하면 브라우저가 자동으로 설치를 안내합니다.

3. **푸시 알림 (Push Notification)**  
   - 사용자에게 푸시 알림을 전송하여 중요한 정보를 실시간으로 전달할 수 있습니다.
   - 푸시 알림을 위해 푸시 서비스와 백엔드 통합이 필요하며, 서비스 워커에서 푸시 알림 이벤트를 수신합니다.

4. **백그라운드 동기화 (Background Sync)**  
   - 네트워크가 연결된 상태에서 자동으로 백그라운드에서 데이터 동기화가 가능합니다.
   - 예를 들어 사용자가 오프라인 상태에서 작성한 콘텐츠는 네트워크가 연결될 때 자동으로 서버에 전송될 수 있습니다.

5. **성능 최적화 (Performance Optimization)**  
   - PWA는 네이티브 앱과 유사한 사용자 경험을 제공하기 위해 속도 및 성능을 최적화해야 합니다.
   - 자원을 효율적으로 캐싱하고, 필요한 데이터만 불러오는 방식으로 성능을 개선합니다.

---

### ⚙ Spring Boot로 PWA 구현 시 주의사항

#### 1. **Service Worker 등록**
- `/static/service-worker.js` 파일을 만들어야 함
- Service Worker는 정적 리소스로 제공되어야 하므로 `resources/static/` 또는 `resources/public/`에 위치

```js
// /static/service-worker.js
self.addEventListener('install', function(event) {
  console.log('Service Worker installed');
});

self.addEventListener('fetch', function(event) {
  event.respondWith(fetch(event.request));
});
```

- index.html에서 등록:

```html
<script>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(() => console.log("Service Worker Registered"));
}
</script>
```

---

#### 2. **manifest.json 제공**
- 앱 이름, 아이콘, 시작 경로 등을 정의
- 경로: `/static/manifest.json`

```json
{
  "name": "My PWA App",
  "short_name": "PWAApp",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#317EFB",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ]
}
```

- HTML에 링크:

```html
<link rel="manifest" href="/manifest.json">
```

---

#### 3. **정적 리소스 캐싱 전략**
- 캐싱 전략을 잘못 설정하면 업데이트가 반영되지 않는 문제가 생김
- Service Worker의 캐시 이름을 버전 관리해야 함

---

#### 4. **HTTPS 환경 필요**
- 로컬에서는 `localhost`는 예외지만, 실제 배포는 HTTPS 필수

---

#### 5. **Spring Boot 설정 팁**
- 정적 파일 제공 위치는 기본적으로 `resources/static/` 이다.
- `application.properties`에서 캐싱 설정도 고려:

```properties
spring.web.resources.cache.cachecontrol.max-age=3600
```

---

#### 6. **푸시 알림 구현 시 주의**
- Spring Boot에서 푸시 구현 시 `Web Push Protocol` 구현이 필요
- Firebase 없이 자체적으로 구현하려면 `VAPID` 키 발급 및 전송 로직 구현 필요
- 관련 라이브러리 예: [`web-push`](https://github.com/web-push-libs/webpush-java)

---

# ✅ 예제

### Progressive Web App (PWA) 기본 적용방법 및 주요 기능
#### 1. PWA란?
PWA는 웹앱에 모바일 앱처럼 설치 및 오프라인 지원 기능을 제공하여 사용자 경험을 향상시키는 기술입니다. 웹앱을 PWA로 구성하면 사용자들은 홈 화면에 앱을 추가하거나 네이티브 앱처럼 사용할 수 있습니다.
---

#### 2. PWA 기본 구성 요소

1. **Manifest 파일 (manifest.json)**  
   - 앱의 이름, 아이콘, 시작 URL, 표시 모드(예: 전체 화면, 최소 UI) 등 메타 데이터를 정의하는 JSON 파일입니다.
   - 이 파일을 통해 웹앱이 홈 화면에 설치될 때의 설정을 제어할 수 있습니다.
   ```json
    {
        "name": "PWA",
        "short_name": "PushAdmin",
        "start_url": "/",
        "display": "standalone",
        "background_color": "#ffffff",
        "theme_color": "#000000",
        "icons": [
            {
            "src": "/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
            },
            {
            "src": "/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
            }
        ]
    }
   ```
   - 이 파일을 `<link rel="manifest" href="/manifest.json">`와 같이 HTML에 추가합니다.

2. **서비스 워커 (Service Worker)**
   - 브라우저와 네트워크 사이에 위치하여 캐싱, 푸시 알림, 백그라운드 동기화 등 기능을 지원하는 자바스크립트 파일입니다.
   - 캐시된 자원을 이용해 오프라인에서도 콘텐츠를 제공하고, 네트워크 상태에 따른 대응이 가능합니다.
   ```javascript
   // service-worker.js

    
   self.addEventListener('install', (event) => {
     event.waitUntil(
       caches.open('app-cache').then((cache) => {
         return cache.addAll(['/index.html', '/styles.css', '/main.js']);
       })
     );
   });
   
   self.addEventListener('fetch', (event) => {
     event.respondWith(
       caches.match(event.request).then((response) => {
         return response || fetch(event.request);
       })
     );
    });

    self.addEventListener('push', event => {
    const data = event.data.json();

        const options = {
            body: data.body,
            icon: '/icon-96x96.png',
            badge: '/icon-72x72.svg'
        };

        event.waitUntil(
            self.registration.showNotification(data.title, options)
        );
    });

    self.addEventListener('notificationclick', event => {
        event.notification.close();
        event.waitUntil(
            clients.openWindow('https://your-site-url.com')
        );
    });


   ```
   - 이 서비스 워커 파일을 `navigator.serviceWorker.register('/sw.js')`로 등록합니다.

---
#### 3. PWA 등록 예시

PWA로 웹앱을 등록하기 위해서는 다음과 같은 단계를 거칩니다:

1. **Manifest 파일 작성 및 HTML에 링크 추가**  
2. **서비스 워커 파일 작성 및 등록**  
3. **캐싱 및 푸시 알림과 같은 추가 기능 구현**  

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Web Push Notification</title>
    <link rel="manifest" href="/manifest.json">
</head>
<body>
    <h2>Web Push 알림</h2>
    <button id="subscribeButton">푸시 알림 구독하기</button>
    <input type="hidden" th:value="${vapidPublicKey}" id="key"> 
</body>
<script src="/main.js"></script>
</html>
```

```js
        // main.js
        document.addEventListener("DOMContentLoaded", function () {
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('/service-worker.js')
                .then(registration => {
                    console.log('Service Worker registered with scope:', registration.scope);

                    // 푸시 알림 구독 버튼 이벤트 설정
                    document.getElementById("subscribeButton").addEventListener("click", async () => {
                        try {
                            var key = document.getElementById("key").value;
                            const applicationServerKey = urlBase64ToUint8Array(key);
                            console.log(applicationServerKey);
                            const options = { userVisibleOnly: true, applicationServerKey };

                            const subscription = await registration.pushManager.subscribe(options);
                            await fetch('/api/subscribe', {
                                method: 'POST',
                                headers: {
                                    'Content-Type': 'application/json'
                                },
                                body: JSON.stringify(subscription)
                            });

                            alert("푸시 알림 구독 완료");
                        } catch (error) {
                            console.error("Failed to subscribe:", error);
                            alert("푸시 알림 구독에 실패했습니다.");
                            alert(error);
                        }
                    });
                })
                .catch(error => {
                    console.error('Service Worker registration failed:', error);
                });
        }
    });

    function urlBase64ToUint8Array(base64String) {
        console.log('base64', base64String);
        const padding = '='.repeat((4 - base64String.length % 4) % 4);
        const base64 = (base64String + padding).replace(/-/g, '+').replace(/_/g, '/');
        const rawData = window.atob(base64);
        return new Uint8Array([...rawData].map(char => char.charCodeAt(0)));
    }

    function unsubscribe() {
        const endpoint = document.getElementById("endpoint").value;

        fetch('/api/unsubscribe', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ endpoint: endpoint })
        })
        .then(response => response.text())
        .then(data => {
            alert(data);
        })
        .catch(error => {
            console.error('Error:', error);
            alert('구독 취소에 실패했습니다.');
        });
    }

```

---

#### 4. 추가 참고 사항

- PWA 기능은 HTTPS 환경에서만 동작합니다.
- 개발 단계에서는 `localhost`에서도 테스트할 수 있습니다.
- 최신 브라우저와 호환성을 고려해 서비스 워커 및 푸시 알림 API를 구현하는 것이 좋습니다.

--- 


#### 5.https 테스트

현재 소스에서 SSL 인증서를 사용하고 있어 포트를 8080으로 바꿔도 작동하지 않는다면, HTTPS 설정을 해제해야 합니다. 다음은 HTTPS 설정을 해제하고 일반 HTTP로 전환하는 방법입니다:

1. **`application.properties` 파일 수정**:
   - HTTPS 설정을 제거하거나 HTTP 설정으로 변경해야 합니다. 파일 내에 HTTPS와 관련된 설정이 있을 경우, 이를 주석 처리하거나 제거해 주세요.
   ```properties
   # 기존 HTTPS 설정을 제거하고 HTTP 설정을 사용합니다
   server.port=8080
   ```

2. **Spring Boot의 HTTPS 설정 제거**:
   - 만약 `application.properties` 외에 SSL 설정이 코드 내에 포함되어 있는 경우, 이를 제거해야 합니다.
   - 보통 SSL 설정은 Java Keystore 파일(.jks)을 사용하고 `server.ssl.enabled=true` 등의 설정을 추가하여 설정합니다. 이를 비활성화합니다.

3. **ngrok 설정**:
   - 로컬 서버가 이제 HTTP 포트 (예: 8080)에서 실행되므로, ngrok를 통해 일반 HTTP를 노출시킬 수 있습니다:
     ```bash
     ngrok http 8080
     ```
   - 이렇게 하면 ngrok가 제공하는 외부에서 접근 가능한 URL을 통해 다른 기기에서도 접근할 수 있습니다.

4. **서비스 워커(Service Worker)와 PWA 설정**:
   - 서비스 워커는 HTTPS 환경에서만 정상 동작하지만, 개발 환경에서는 `localhost` 또는 ngrok가 제공하는 HTTPS URL에서도 동작할 수 있습니다. ngrok의 HTTPS URL을 사용할 수 있습니다.
   
이 과정을 통해 HTTPS 설정을 제거하고 HTTP로 변경하여 ngrok를 사용할 수 있으며, 이를 통해 외부에서 접근이 가능하도록 설정할 수 있습니다.

#### spring boot 환경에서는 VAPID 키 적용해야함.

- 웹 푸시에서는 어떤 서버에서 메시지를 발송했는지 식별하기 위해 VAPID(Voluntray Application Server Identification) 인증 방식을 사용한다.

```bash
npx web-push generate-vapid-keys
```

출력된 `publicKey`와 `privateKey`를 기록해 둡니다. Spring Boot 애플리케이션의 `application.properties`에 `publicKey`와 `privateKey`를 추가합니다.

```properties
vapid.publicKey=YOUR_PUBLIC_VAPID_KEY
vapid.privateKey=YOUR_PRIVATE_VAPID_KEY
```

 - webpush 라이브러리 추가
**Maven**

```xml
		<!-- webpush-->
		<dependency>
			<groupId>nl.martijndwars</groupId>
			<artifactId>web-push</artifactId>
			<version>5.1.1</version>
		</dependency>
		<dependency>
			<groupId>org.bouncycastle</groupId>
			<artifactId>bcprov-jdk15on</artifactId>
			<version>1.70</version>
		</dependency>
		<dependency>
			<groupId>org.bouncycastle</groupId>
			<artifactId>bcprov-jdk15on</artifactId>
			<version>1.70</version>
		</dependency>

```

---

```sample
vapid.publicKey=BAMUrcSv1IJmMs4qD-y_iAlpMnxDpziGA6M9eqOLPfn6gPUuWHUR8_UhDTv_baxxMxSX1WDQOk_141VTGXqpERo 
vapid.privateKey=g_1o23wPTtCdasfKK6zo4D7eE0ny5mosC0hVBztC43o 
vapid.subject=mailto:your-email@example.com
```
- admin.html

```html

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>푸시 알림 관리자 화면</title>
    </head>
    <body>
        <h2>푸시 알림 전송</h2>
        <form id="pushForm">
            <label for="title">제목:</label>
            <input type="text" id="title" name="title" required><br><br>

            <label for="body">내용:</label>
            <textarea id="body" name="body" required></textarea><br><br>

            <button type="button" onclick="sendNotification()">알림 보내기</button>
        </form>
        <h2>구독 정보</h2>
        <button type="button" onclick="getSubscriptionInfo()">구독 정보 가져오기</button>
        <div id="subscriptionsContainer" style="display:none;">
            <h3>구독 정보 목록</h3>
            <ul id="subscriptionsList"></ul>
        </div>
        <script>
            function sendNotification() {
                const title = document.getElementById("title").value;
                const body = document.getElementById("body").value;

                fetch('/api/send-notification?title=' + encodeURIComponent(title) + '&body=' + encodeURIComponent(body), {
                    method: 'POST'
                })
                .then(response => response.text())
                .then(data => {
                    alert(data);
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('알림 전송에 실패했습니다.');
                });
            }
            function getSubscriptionInfo() {

                fetch('/api/get-subscription', {
                    method: 'GET'
                })
                .then(response => {
                    console.log(response);
                    if (response.status === 204) {
                        alert('구독 정보가 없습니다.!!!!');
                        return [];
                    } else {
                        return response.json();
                    }
                })
                .then(data => {
                    if (Array.isArray(data) && data.length > 0) {
                        const subscriptionsList = document.getElementById("subscriptionsList");
                        subscriptionsList.innerHTML = '';
                        data.forEach(subscription => {
                            const listItem = document.createElement("li");
                            listItem.setAttribute("data-endpoint", subscription.endpoint);
                            listItem.textContent = `Endpoint: ${subscription.endpoint}`;
                            const unsubscribeButton = document.createElement("button");
                            unsubscribeButton.textContent = "구독 취소";
                            unsubscribeButton.onclick = () => unsubscribe(subscription.endpoint);
                            listItem.appendChild(unsubscribeButton);
                            subscriptionsList.appendChild(listItem);
                        });
                        document.getElementById("subscriptionsContainer").style.display = "block";
                    } else {
                        alert('구독 정보가 없습니다.');
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('구독 정보를 가져오는데 실패했습니다.');
                });
            }

            function unsubscribe(endpoint) {
                fetch('/api/unsubscribe', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ endpoint: endpoint })
                })
                .then(response => response.text())
                .then(data => {
                    alert(data);
                    const listItem = document.querySelector(`li[data-endpoint='${endpoint}']`);
                    if (listItem) {
                        listItem.remove();
                    }
                })
                .catch(error => {
                    console.error('Error:', error);
                    alert('구독 취소에 실패했습니다.');
                });
            }
        </script>
    </body>
    </html>



```
### ✅ 최종 요약 카드 (pwa)

| 항목 | 내용 |
|------|------|
| 프로젝트 목적 | Spring 기반의 PWA 기능 구현 (Web Push 중심) |
| 주요 라이브러리 | `spring-boot-starter-web`, `thymeleaf`, `web-push`, `bouncycastle` |
| PWA 구성 요소 | Service Worker 연동 예상, 푸시 메시지 발송 구현 |
| DB 연동 | 없음 (향후 필요 시 JPA/MyBatis 추가 고려) |
| 기타 | Lombok, Devtools, 테스트 환경 구성 포함 |

---
