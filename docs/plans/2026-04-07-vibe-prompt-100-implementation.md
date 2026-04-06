# 바이브 코딩 시작 프롬프트 100선 — 구현 계획

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 14개 분야 100개 바이브 코딩 프롬프트를 키네틱 타이포그래피 테마로 보여주는 단일 HTML 웹페이지 구현

**Architecture:** 단일 `index.html` 파일에 HTML 구조 + CSS 스타일/애니메이션 + JavaScript 인터랙션을 모두 포함. 100개 프롬프트 데이터는 JS 배열로 내장. Google Fonts CDN만 외부 의존.

**Tech Stack:** HTML5, CSS3 (@keyframes, CSS Grid, Flexbox), Vanilla JavaScript, Google Fonts (Black Han Sans, Noto Sans KR, Bebas Neue)

---

### Task 1: HTML 기본 구조 + CSS 변수 + 폰트 설정

**Files:**
- Create: `index.html`

**Step 1:** `index.html` 파일 생성. 다음 포함:
- DOCTYPE, html lang="ko", meta charset/viewport
- Google Fonts CDN link (Black Han Sans, Noto Sans KR, Bebas Neue)
- `<style>` 태그 내 CSS 변수 정의:
  ```css
  :root {
    --black: #000000;
    --white: #FFFFFF;
    --red: #FF0000;
    --royal-blue: #0048BA;
    --card-bg: #111111;
    --text-gray: #AAAAAA;
    --font-title: 'Black Han Sans', sans-serif;
    --font-body: 'Noto Sans KR', sans-serif;
    --font-accent: 'Bebas Neue', sans-serif;
  }
  ```
- 기본 body 스타일 (배경 블랙, 텍스트 화이트, margin 0)
- prefers-reduced-motion 미디어 쿼리 기본 설정

**Step 2:** 브라우저에서 열어 블랙 배경 + 폰트 로딩 확인

---

### Task 2: 히어로 섹션 HTML + 키네틱 CSS 애니메이션

**Files:**
- Modify: `index.html`

**Step 1:** 히어로 섹션 HTML 구조 작성:
- 상단 바: AICLab 텍스트 로고 (좌) + "사용법" 버튼 (우)
- 메인 타이틀 영역: 각 글자를 `<span>`으로 감싸서 개별 애니메이션 가능하게
  - 1행: "코딩 몰라도 된다."
  - 2행: "붙여넣기만 하면"
  - 3행: "웹앱이 나온다." (레드 강조)
- 서브타이틀: "바이브 코딩 시작 프롬프트 100선 — AI 시대, 누구나 개발자"
- 스크롤 유도 버튼: "▼ 프롬프트 둘러보기"

**Step 2:** CSS @keyframes 애니메이션 작성:
- `@keyframes slideUp` — Y축 40px 아래에서 올라오며 opacity 0→1
- `@keyframes bounceScale` — scale(1) → scale(1.1) → scale(1) 바운스
- `@keyframes fadeIn` — opacity 0→1
- `@keyframes float` — 스크롤 유도 버튼 위아래 반복
- 각 글자에 `animation-delay` stagger 적용 (JS로 동적 세팅)

**Step 3:** 히어로 섹션 스타일링:
- 전체 높이 100vh, flexbox 중앙 정렬
- 타이틀 72px (데스크톱), 서브타이틀 로열블루
- 상단 바 고정

**Step 4:** 브라우저에서 키네틱 애니메이션 동작 확인

---

### Task 3: 사용법 모달

**Files:**
- Modify: `index.html`

**Step 1:** 모달 HTML 추가:
- 오버레이 배경 (반투명 블랙)
- 모달 박스: 3단계 안내 (① 분야 클릭 → ② 복사 버튼 → ③ AI 채팅창에 붙여넣기)
- 닫기 버튼 (X)

**Step 2:** 모달 CSS (중앙 정렬, 페이드인 애니메이션)

**Step 3:** JS — "사용법" 버튼 클릭 시 모달 열기/닫기, 오버레이 클릭 시 닫기

**Step 4:** 동작 확인

---

### Task 4: 고정 헤더 (스크롤 반응)

**Files:**
- Modify: `index.html`

**Step 1:** 고정 헤더 HTML 추가:
- AICLab 로고 + "바이브 코딩 100선" + "전체 펼치기/접기" 버튼
- 초기 상태: hidden

**Step 2:** CSS:
- position fixed, top 0, z-index 높게
- 블랙 배경 + 레드 1px 하단 보더
- transform translateY(-100%) → translateY(0) 전환

**Step 3:** JS — IntersectionObserver로 히어로가 뷰포트에서 벗어나면 고정 헤더 표시, 돌아오면 숨김

**Step 4:** 스크롤 동작 확인

---

### Task 5: 프롬프트 데이터 구조 (JS 배열)

**Files:**
- Modify: `index.html`

**Step 1:** `<script>` 태그 내에 14개 분야 × 100개 프롬프트 데이터를 JS 배열로 정의:

```javascript
const categories = [
  {
    name: "일상생활",
    icon: "🏠",
    prompts: [
      {
        id: 1,
        level: "쉬움",
        title: "가계부 자동 계산기",
        description: "수입·지출을 입력하면 잔액과 카테고리별 합계를 자동 계산",
        prompt: "가계부 자동 계산기 웹앱을 만들어 주세요.\n수입과 지출 항목..."
      },
      // ... 나머지 프롬프트
    ]
  },
  // ... 나머지 13개 분야
];
```

**Step 2:** 사용자가 제공한 원본 텍스트에서 모든 100개 프롬프트를 정확히 추출하여 데이터화. 각 프롬프트의 `prompt` 필드에는 제목 행 + 본문 전체 포함.

---

### Task 6: 아코디언 토글 UI 렌더링

**Files:**
- Modify: `index.html`

**Step 1:** JS로 `categories` 배열을 순회하며 DOM 생성:
- 분야별 `<section>` → 클릭 가능한 분야 제목 바 + 숨겨진 카드 컨테이너
- 카드 컨테이너 내 프롬프트 카드 DOM 생성

**Step 2:** 아코디언 CSS:
- 카드 컨테이너: max-height 0, overflow hidden, transition
- 열림 상태: max-height 계산값, 카드 stagger 애니메이션
- 분야 제목 호버: 좌측 ■ 블록 레드 확장 transition
- 화살표: transform rotate 전환

**Step 3:** JS 클릭 이벤트:
- 분야 제목 클릭 → 해당 카드 컨테이너 토글
- 다른 분야 자동 닫힘 없음 (여러 개 동시 열기 가능)

**Step 4:** 토글 동작 + 애니메이션 확인

---

### Task 7: 프롬프트 카드 스타일링 + 복사 기능

**Files:**
- Modify: `index.html`

**Step 1:** 카드 CSS:
- 배경 #111111, 둥근 모서리 8px
- 난이도 배지: [쉬움] 로열블루, [보통] 레드
- 호버: 좌측 3px 보더 (쉬움→블루, 보통→레드)
- 데스크톱 2열 그리드, 태블릿/모바일 1열

**Step 2:** 복사 버튼 + JS:
- 클릭 시 `navigator.clipboard.writeText(제목 + "\n" + 본문)`
- "복사 완료!" 텍스트 위로 떠오르며 페이드아웃 애니메이션

**Step 3:** 동작 확인 — 복사 후 메모장에 붙여넣기 검증

---

### Task 8: 전체 펼치기/접기 + 푸터 + 최종 마무리

**Files:**
- Modify: `index.html`

**Step 1:** 고정 헤더의 "전체 펼치기/접기" 버튼 JS:
- 모든 아코디언을 한 번에 열기/닫기 토글

**Step 2:** 푸터 HTML + CSS:
- AICLab 이름 + 이메일 링크 + 홈페이지 링크
- © 2026 KAEA & AICLab 김진수

**Step 3:** 반응형 최종 점검:
- 데스크톱/태블릿/모바일 각각 확인
- prefers-reduced-motion 적용 확인

**Step 4:** 전체 플로우 테스트:
- 히어로 애니메이션 → 스크롤 → 고정헤더 → 토글 → 복사 → 푸터

---

## 실행 방식

단일 HTML 파일이므로 Task 1~8을 순차적으로 진행하며, 각 Task 완료 시 브라우저에서 시각적 검증.
