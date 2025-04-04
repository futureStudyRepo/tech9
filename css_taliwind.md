# 🌟 Tailwind CSS 기본 규칙 요약

Tailwind는 `유틸리티 퍼스트` CSS 프레임워크로, 클래스만으로 UI를 빠르게 구성할 수 있습니다.
"클래스로 바로 스타일을 주는 CSS 프레임워크", React/Vue/Vanilla 어디서나 인기폭발
---

## 📦 1. 클래스 = 스타일 의미

```html
<button class="bg-blue-500 text-white px-4 py-2 rounded shadow">
  버튼
</button>
```

| 클래스 | 의미 |
|--------|------|
| `bg-blue-500` | 배경색 (파랑 500단계) |
| `text-white` | 글자색 흰색 |
| `px-4`, `py-2` | 패딩 (가로/세로) |
| `rounded` | 모서리 둥글게 |
| `shadow` | 그림자 효과 |

---

## 📏 2. 기본 속성 클래스

| 속성 | 예시 | 설명 |
|------|------|------|
| 배경색 | `bg-red-500` | 배경 빨간색 |
| 텍스트색 | `text-gray-700` | 회색 글자 |
| 패딩/마진 | `p-4`, `px-2`, `mt-8` | 여백 설정 |
| 글자크기 | `text-sm`, `text-xl` | 폰트 크기 |
| 정렬 | `flex`, `items-center`, `justify-between` | Flexbox 정렬 |
| 테두리 | `border`, `border-gray-300` | 테두리 선 |
| 그리드 | `grid`, `grid-cols-2` | Grid 레이아웃 |
| 모서리 | `rounded`, `rounded-lg` | 둥근 모서리 |
| 그림자 | `shadow`, `shadow-lg` | 요소 그림자 |

---

## 🎨 3. 색상 체계 (색상 단계)

Tailwind는 색상과 함께 `숫자 단계 (50 ~ 900)`로 밝기 조절:

- `gray-50` → 매우 밝음
- `gray-900` → 매우 어두움

```html
<div class="bg-green-200 text-green-800"></div>
```

---

## 📱 4. 반응형 디자인 (모바일 우선)

| 접두어 | 의미 | 적용 화면 너비 |
|--------|------|----------------|
| `sm:` | Small | ≥ 640px |
| `md:` | Medium | ≥ 768px |
| `lg:` | Large | ≥ 1024px |
| `xl:` | Extra Large | ≥ 1280px |

```html
<p class="text-base md:text-xl lg:text-2xl">
  반응형 글자 크기
</p>
```

---

## 🌙 5. 다크 모드

설정 필요: `darkMode: "class"` (tailwind.config.js)

```html
<body class="dark">
  <div class="bg-white dark:bg-gray-900 text-black dark:text-white">
    다크모드 지원 UI
  </div>
</body>
```

---

## 🛠 6. 커스터마이징 (tailwind.config.js)

```js
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {
      colors: {
        primary: '#1E40AF'
      }
    }
  }
}
```

---

## ✅ 요약 정리

| 기능 | 접두어 예시 |
|------|-------------|
| 배경색 | `bg-blue-500` |
| 글자색 | `text-gray-800` |
| 여백 | `p-4`, `m-2`, `mt-6` |
| 크기 | `w-64`, `h-10` |
| 정렬 | `flex`, `justify-center`, `items-end` |
| 모서리 | `rounded`, `rounded-xl` |
| 반응형 | `sm:`, `md:`, `lg:` |
| 다크모드 | `dark:bg-black`, `dark:text-white` |

---

## 📌 참고 사이트

- 공식 문서: https://tailwindcss.com/docs
- 색상 예제: https://tailwindcss.com/docs/customizing-colors
- Playground (CDN 테스트): https://play.tailwindcss.com/

## sample

```html

    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>사용자 관리</title>
        <script src="https://cdn.tailwindcss.com"></script>
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link
        href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap"
        rel="stylesheet"
        />
        <style>
        body {
            font-family: "Noto Sans KR", sans-serif;
        }
        </style>
    </head>
    <body class="bg-gradient-to-tr from-blue-100 via-white to-purple-100 min-h-screen">
        <div class="max-w-md mx-auto py-10 px-6">
        <!-- 카드 -->
        <div class="bg-white rounded-2xl shadow-xl p-8">
            <!-- 헤더 -->
            <h1 class="text-3xl font-bold text-center text-blue-600 mb-6">
            사용자 등록
            </h1>

            <!-- 폼 -->
            <form class="space-y-4">
            <div>
                <label class="block mb-1 font-semibold text-gray-700">이름</label>
                <input
                type="text"
                placeholder="이름 입력"
                class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
                />
            </div>
            <div>
                <label class="block mb-1 font-semibold text-gray-700">이메일</label>
                <input
                type="email"
                placeholder="이메일 입력"
                class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
                />
            </div>
            <button
                type="submit"
                class="w-full bg-gradient-to-r from-blue-500 to-purple-500 text-white py-2 rounded-lg font-semibold shadow-md hover:brightness-110 transition"
            >
                등록하기
            </button>
            </form>

            <!-- 구분선 -->
            <div class="my-6 border-t border-dashed"></div>

            <!-- 사용자 목록 -->
            <h2 class="text-xl font-bold text-gray-800 mb-3">👥 등록된 사용자</h2>
            <div class="space-y-3">
            <div
                class="flex items-center justify-between bg-gray-50 px-4 py-3 rounded-lg border shadow-sm hover:bg-gray-100 transition"
            >
                <div>
                <p class="font-semibold text-gray-800">홍길동</p>
                <p class="text-sm text-gray-500">gildong@example.com</p>
                </div>
                <span
                class="inline-block bg-blue-100 text-blue-600 text-xs px-2 py-1 rounded-full font-medium"
                >Active</span
                >
            </div>
            <div
                class="flex items-center justify-between bg-gray-50 px-4 py-3 rounded-lg border shadow-sm hover:bg-gray-100 transition"
            >
                <div>
                <p class="font-semibold text-gray-800">이몽룡</p>
                <p class="text-sm text-gray-500">mongryong@example.com</p>
                </div>
                <span
                class="inline-block bg-green-100 text-green-600 text-xs px-2 py-1 rounded-full font-medium"
                >Pending</span
                >
            </div>
            </div>
        </div>
        </div>
    </body>
    </html>

```