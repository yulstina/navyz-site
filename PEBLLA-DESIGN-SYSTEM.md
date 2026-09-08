# PEBLLA 디자인 시스템 분석 → NAVYZ 적용 가이드

분석 대상: <https://www.peblla.com/> (Framer 기반, 2026-09 측정)
적용 대상: <http://soyulh.dothome.co.kr/nz/> · 소스 폴더 `작업폴더/`

이 문서는 PEBLLA 홈페이지에서 **실측한** 토큰·레이아웃·인터랙션 값을 정리하고,
그것을 NAVYZ 사이트(`index.html` + `assets/css/style.css`)에 어떻게 옮겼는지를 기록한다.
색상과 헤더는 NAVYZ 기존 값을 유지하고, **레이아웃·리듬·모션만** PEBLLA를 따른다.

---

## 1. 분석 방법

| 항목 | 방법 |
|---|---|
| 색 토큰 | 페이지 내 `--token-*` CSS 변수 전수 추출 |
| 타이포 | `h1/h2/h3` 실제 `computedStyle` 측정 |
| 섹션 리듬 | 폭 900px 이상 블록의 `top / height / backgroundColor` 스캔 |
| 모션 | Framer `data-framer-appear-id` 요소의 초기/최종 스타일 비교 |

문서 총 높이 11,608px / 뷰포트 1440×900 기준.

---

## 2. 색 (PEBLLA 원본)

PEBLLA는 **무채색 램프 + 단일 액센트** 구조다. 컬러는 거의 쓰지 않고, 색은 사진과 그라디언트가 담당한다.

```
--gray-0   #ffffff      --gray-50  #fafafa      --gray-100 #f5f5f5
--gray-200 #eeeeee      --gray-300 #e0e0e0      --gray-400 #bdbdbd
--gray-500 #9e9e9e      --gray-600 #757575      --gray-700 #616161
--gray-800 #424242      --gray-900 #212121      --black    #000000
--accent   #ff4d00      (링크 활성 / CTA 강조. 전체 면적의 1% 미만)
```

- 본문 기본색: `#616161`
- 카드 제목: `#001122`
- 섹션 배경: `#ffffff` 이 기본, 강조 밴드만 `#fafafa`
- 링크 hover → `#000`, active → `#ff4d00`

### NAVYZ 매핑 (색상은 그대로 유지)

PEBLLA의 오렌지 액센트는 **쓰지 않는다.** 역할만 그대로 옮긴다.

| PEBLLA 역할 | PEBLLA 값 | NAVYZ 값 (기존 유지) |
|---|---|---|
| 액센트 | `#ff4d00` | `#354c9c` (NAVYZ 블루) |
| 잉크 / 제목 | `#001122` | `#293241` |
| 본문 | `#616161` | `#61708a` |
| 기본 배경 | `#ffffff` | `#ffffff` |
| 강조 밴드 | `#fafafa` | `#f3f8ff` |
| 다크 밴드 | — | `#091832` (헤더·배너) |

---

## 3. 타이포그래피 (실측)

| 역할 | PEBLLA 실측 | NAVYZ 적용 |
|---|---|---|
| H1 (히어로) | Roboto 48px / lh 48px(1.0) / w500 / 흰색 | `clamp(40px,6vw,56px)` · lh 1.1 |
| H2 (섹션) | Roboto 28px / lh 34.72px(1.24) / w500 | `clamp(28px,5vw,44px)` · lh 1.1 · `max-width:800px` 중앙정렬 |
| H3 (카드) | Inter 20px / lh 28px / w600 / **ls −0.5px** | 20px / w600 / ls −0.5px |
| 본문 | 16px / **lh 1.6em** / w500 | 16px / lh 1.6 |
| 아이브로우 | 소문자 라벨 ("Features", "Hardwares", "Online") | `.title .eyebrow` |
| 문단 간격 | 20px | 20px |

핵심 규칙 3가지:

1. **히어로 line-height = 1.0.** 두 줄짜리 큰 문장을 꽉 붙여서 덩어리로 읽히게 한다.
2. **섹션 제목은 800px 폭 제한 + 중앙정렬.** 한 줄이 아니라 두 줄로 접히는 게 정상이다.
3. **아이브로우(작은 카테고리 라벨) → 큰 H2 → 한 줄 서브카피** 순서가 모든 섹션에 반복된다.

---

## 4. 레이아웃 · 수직 리듬 (실측 스캔 결과)

| top(px) | height | 배경 | 섹션 |
|---:|---:|---|---|
| 0 | 65 | #fff | 고정 헤더 (pill nav) |
| 65 | 1200 | #fff | 히어로 — 중앙 정렬 텍스트 + 그라디언트 배경 |
| 305 | 720 | — | 히어로 아래로 **겹쳐 내려오는** 제품 비주얼 |
| 1265 | 905 | — | Features 인트로 (아이브로우 + H2 + 문단) |
| 2300 | 720 | #fff | 01 넘버링 피처 (좌 텍스트 / 우 이미지) |
| 2900 | 720 | #fff | 02 넘버링 피처 (좌우 반전) |
| 3630 | 720 | #fff | 03 넘버링 피처 |
| 4350 | 1200 | #fff | Hardwares — 가로 카드 캐러셀 (7장) |
| 5550 | 820 | — | Online — 대형 미디어 밴드 |
| 6370 | 1870 | #fff | Advanced tools — 2단 교차 카드 |
| 8241 | 592 | — | Customer — 고객 사례 |
| 8832 | 800 | — | CTA 밴드 "Take control now" |
| 9712 | 892 | #fafafa | Stay connected — 문의 폼 + 주소 |
| 10645 | 423 | — | 푸터 5열 그리드 |

정리하면:

- **콘텐츠 폭 1200px 고정**, 바깥 컨테이너 1265px.
- **섹션 1블록 = 720px**가 기본 단위. 인트로형은 900~1200px.
- 섹션 상하 패딩 **110~120px**.
- 히어로 비주얼이 다음 섹션 위로 **겹쳐 들어간다**(top 305에서 시작해 히어로 하단을 넘김). 이게 PEBLLA 첫인상의 핵심.
- 배경색은 흰색이 압도적이고, 대비는 **색이 아니라 여백**으로 만든다.

### 그리드

- 카드 캐러셀: `flex` + `scroll-snap`, 카드폭 약 300px, gap 24px, 양쪽 원형 화살표(44px).
- 넘버링 피처: 1100px 폭 안에서 `flex-row`, 텍스트 45% / 미디어 55%, 홀짝 반전.
- 푸터: `display:grid` 5열.

### 모서리 · 그림자

실측된 border-radius 값: `8, 10, 16, 20, 30, 40, 100, 500, 1000px`

| 용도 | 값 |
|---|---|
| 작은 칩 / 태그 | 8~10px |
| 카드 · 이미지 | 16~20px |
| 대형 미디어 | 30~40px |
| 버튼 · pill | 999px (완전 알약) |
| 원형 아이콘 · 로고 버튼 | 50% |

그림자는 아주 얕게, 카드 hover에서만 깊어진다.

---

## 5. 컴포넌트

### 5-1. 헤더 (pill nav)
- 높이 65px, 화면 상단에 떠 있는 **알약형** 바.
- 좌: 로고 / 중: 텍스트 메뉴 / 우: 고대비 CTA 버튼 1개.
- 스크롤하면 배경이 불투명해지고 그림자가 생긴다 (`header.scrolled`).
- **NAVYZ는 이 헤더 스타일·색을 그대로 유지한다.**

### 5-2. 히어로
- 그라디언트 풀블리드 배경 + 중앙 정렬 흰 텍스트 + pill 버튼 1개.
- 아래로 제품 스크린샷이 겹쳐 올라온다.

### 5-2b. 넘버링 스티키 스택 (01 / 02 / 03) — 실측

PEBLLA 메인의 핵심 인터랙션. 세 블록이 `position: sticky; top: 0`으로 화면 상단에 고정되면서
**다음 카드가 이전 카드를 덮으며 올라온다.** 스크롤 하이재킹이 아니라 순수 CSS sticky다.

| 속성 | 실측값 |
|---|---|
| 블록 position | `sticky`, `top: 0` (마지막만 `top: 60px`) |
| 블록 높이 | 778 / 778 / 664px (뷰포트 높이급) |
| 배경 | `#ffffff` 불투명 — 이게 있어야 아래 카드가 가려진다 |
| z-index | 순서대로 증가 (1 → 2 → 3) |
| 부모 높이 | 자식 높이 합(2220px). 별도 스크롤 길이를 만들지 않는다 |
| 큰 숫자 | 240px / weight 400 / `background-clip: text` + `-webkit-text-fill-color: transparent` |
| 숫자 그라디언트 | `linear-gradient(540deg, #eee 41%, rgba(255,255,255,0) 73%)` — 아래로 갈수록 사라짐 |
| 좌측 컬럼 | 얇은 세로 라인 + H3 + 본문, 아래에 "See also" 액센트 링크 |
| 우측 | 정사각 라이트그레이 패널 안에 흰 카드 + 제품 이미지 |

**모바일도 동일하다.** 375px 폭에서도 `sticky / top:0 / 812px(100vh)`를 그대로 유지하고
내부만 1컬럼으로 접힌다. 데스크톱에서만 스티키를 켜면 PEBLLA와 달라진다.

NAVYZ 구현: `index.html`의 `.steps > .steps-stack > .step`,
스타일은 `style.css`의 `steps : sticky numbered stack` 블록.
진입 페이드는 IntersectionObserver(threshold .28)로 1회만 실행한다.

### 5-2c. "Your brand, your app" 폰 슬라이더 — 실측

폰 프레임 안에 **312px 폭 화면 6장이 가로로 늘어선 트랙**이 있고, 마스크(312×676, `overflow:hidden`)가
한 장씩만 보여준다. 트랙의 `transform`을 0.1초 간격으로 13초간 샘플링한 결과:

| 항목 | 실측값 |
|---|---|
| 이동 단위 | 정확히 312px = 화면 1장 |
| 전환 시각 | t = 327 / 2402 / 4384 / 6460 / 8537 / 10516 / 12573 ms |
| **주기** | **≈ 2000ms** (간격 2075 / 1982 / 2076 / 2077 / 1979 / 2057) |
| **슬라이드 시간** | **≈ 900ms** (나머지 ~1100ms는 정지) |

> NAVYZ에서는 이 값을 **1400ms 주기 / 480ms 슬라이드**로 조정했다.
> PEBLLA의 폰 화면은 가벼워서 즉시 뜨지만 NAVYZ 화면은 정보량이 많아
> 2000ms로는 "넘어가는 중"이라는 게 눈에 안 들어온다는 피드백이 있었다.
> 이미지 무게도 원인이었다 — 아래 참고.
| 순환 | 6번째 다음 첫 장으로 되돌아감 |
| 로고 상태 | 비활성은 그레이스케일 + 낮은 불투명도, 활성만 원색 + 확대 |

**스크롤 반응은 없다.** 5800→6600px 구간에서 폰과 로고 6개의 화면 좌표를 재보니
스크롤량과 정확히 1:1로 움직였다 (570→−230 등, 델타 −200 / 스크롤 +200).
즉 패럴랙스나 드리프트가 전혀 없고, 스크롤에 반응하는 것은 **섹션 진입 시 1회 리빌**과
**화면 밖에서 오토플레이 정지**뿐이다. 속도 기반 translateY를 얹으면 PEBLLA와 달라진다.

NAVYZ 구현 주의: 예전에 vanilla와 jQuery 두 벌이 각각 3000ms / 2800ms 타이머를 돌리며
같은 폰 엘리먼트의 transform을 동시에 쓰고 있었다. 반드시 **한 벌만** 유지할 것.

### 5-3. 카드
```
[ 16:10 이미지 ]  radius 16px, hover 시 이미지만 scale(1.04)
 H3 20px w600 ls-0.5
 본문 15px #616161 (2~3줄)
 "Read More →"  액센트 컬러 텍스트 링크
```

### 5-4. 캐러셀
- 좌우 44px 원형 버튼, 흰 배경 + 얕은 그림자.
- 끝에 도달하면 `disabled` (opacity .35).
- 모바일에서는 버튼 숨기고 스와이프 + scroll-snap.

### 5-5. CTA 밴드
- 좌측 큰 카피 + 버튼 2개(Primary / Secondary), 우측 제품 이미지.
- 이미지가 밴드 하단에 살짝 걸쳐 잘린다.

### 5-6. 로고 마퀴
- 고객사 로고를 무한 가로 스크롤. 양끝 페이드 마스크. hover 시 정지.

---

## 6. 모션 (PEBLLA 인터랙션 언어)

PEBLLA는 스크롤 하이재킹을 쓰지 않는다. 전부 **한 번만 실행되는 진입 애니메이션 + 짧은 호버 피드백**이다.

| 동작 | 값 |
|---|---|
| 진입(Appear) | `opacity 0 → 1`, `translateY(24px) → 0` |
| 지속 | 600~800ms |
| 이징 | `cubic-bezier(.25,.46,.45,.94)` (ease-out quad) |
| 스태거 | 형제 요소 간 100~150ms |
| 트리거 | 뷰포트 진입 16%, 1회만 |
| 호버(카드) | `translateY(-4px)` + 그림자 강화, 200ms |
| 호버(이미지) | `scale(1.04)`, 400ms |
| 캐러셀 이동 | `scrollBy` + `behavior:smooth` |
| 폰 목업 슬라이드 | **2000ms 주기 / 900ms 슬라이드** (아래 실측 참고) |
| 감속 모드 | `prefers-reduced-motion: reduce` 시 전부 즉시 표시 |

### NAVYZ 구현 매핑

| PEBLLA 모션 | NAVYZ 구현 |
|---|---|
| Appear (fade-up) | AOS `data-aos="fade-up"` + `data-aos-delay` 100/200/300 스태거 |
| 스크롤 진행 효과 | GSAP ScrollTrigger (히어로 비주얼 겹침) |
| 캐러셀 | 자체 `carousel-nav` + `scrollBy` |
| 세로 인용 슬라이드 | Swiper (`direction: vertical`, autoplay 2000ms, loop) |
| 로고 마퀴 | CSS `@keyframes roll`, 36s linear infinite, hover 시 `animation-play-state: paused` |
| 폰 목업 | 로고 버튼 클릭 → `track` translateX, 800ms |

---

## 7. NAVYZ 콘텐츠 매핑

PEBLLA의 **틀**에 NAVYZ의 **내용**을 끼워 넣는다. 출처: <https://www.navyz.com/>

| PEBLLA 섹션 | NAVYZ 내용 |
|---|---|
| 히어로 | "Contact-free Solutions to Run Your Business" / GET STARTED |
| Features 인트로 | "You'll Love Our POS" — 결제부터 리포팅까지 |
| 넘버링 피처 01~03 | Restaurant & QSR POS / Salon POS / Retail & Market POS |
| Hardwares 카드 | NAVYZ Total Business Solution 7종 (POS · Web · Online Ordering · Marketing · SPA & Resort · Payment · Financial) |
| Customer 인용 | 고객사 사례 |
| Advanced tools | 24시간 지원 · 다국어 지원 · 온사이트 기술자 |
| Web 섹션 | Web Solution 3종 (E-Commerce / Website / Online Offering) — POS 섹션 바로 아래 |
| Our Clients | navyz.com의 실제 고객사 로고 스트립(`company-clients.png`)을 마퀴로 |
| 블로그 | 메인에 인사이트 카드 3장 → 사진 클릭 시 `blog.html` |
| CTA 밴드 | "Get started on your 30-DAY FREE trial TODAY!" |
| 푸터 | Products / Company / Related sites |

### 메뉴 축소

NAVYZ 원본은 메뉴가 너무 많고 깊다. PEBLLA의 얕은 3-메뉴 리듬에 맞춰 줄인다.

```
Products ▾  ( POS & Kiosk · Online Ordering · Web Solution )
Blog
Login
[ Free Demo → ]   ← 우측 고대비 버튼
```

### Our Clients 로고 크롭

`company-clients.png`(1920×250)은 로고 9개가 한 장에 붙어 있는 시트다.
잉크가 있는 열을 스캔해서 각 로고의 바운딩 박스를 뽑아 두었다:

| # | 브랜드 | x, y, w, h |
|---|---|---|
| 1 | PIXIU 마라홍탕 | 41, 102, 176, 47 |
| 2 | YUKDAEJANG | 249, 90, 184, 50 |
| 3 | TOUS les JOURS | 465, 122, 175, 25 |
| 4 | Oh K-DOG | 679, 49, 170, 170 |
| 5 | 인생네컷 | 889, 39, 174, 174 |
| 6 | SALADY | 1101, 41, 170, 172 |
| 7 | 인생맥주 | 1317, 74, 163, 103 |
| 8 | 흑화당 HHD | 1532, 80, 154, 107 |
| 9 | TOM N TOMS COFFEE | 1735, 40, 185, 171 |

로고마다 슬롯 크기는 같게(데스크톱 170×96) 두고, 그 안에서 비율을 유지한 채 맞춘다.
크롭 위치는 `.client-logo.c1` ~ `.c9` 클래스로 고정한다 — 마퀴가 런타임에 리스트를
복제하므로 `:nth-child()`로 잡으면 **복제된 뒤쪽 로고가 전부 사라진다.**

PC와 모바일 메뉴 항목을 일치시킨다. 헤더 마크업은 각 HTML에 인라인이므로
한 곳만 고치면 페이지마다 메뉴가 달라진다 — 반드시 전 파일에 동시 반영할 것.

---

## 8. 반응형

| 브레이크포인트 | 처리 |
|---|---|
| ≥1200px | 1200px 컨테이너, 전 레이아웃 그대로 |
| 992~1199px | 컨테이너 92vw, 카드 3열 |
| 768~991px | 넘버링 피처 1열 스택, 카드 2열 |
| <768px | 전부 1열, 캐러셀 화살표 숨김 + 스와이프, 히어로 H1 40px |
| <480px | CTA 버튼 full-width, 폰 목업 축소 |

터치 타깃 최소 44px.

---

## 9. 파일 구조 (작업폴더)

```
작업폴더/
├─ PEBLLA-DESIGN-SYSTEM.md   ← 이 문서
├─ index.html                 메인 (히어로·솔루션·산업·인사이트·CTA)
├─ products.html              Kiosk / POS 제품
├─ online.html                Online ordering
├─ web.html                   Web Solution (히어로 + 01/02/03 스티키 + Why NAVYZ + CTA)
├─ blog.html                  인사이트 목록
├─ contact.html               Free Demo 문의
├─ location.html              지점
├─ login.html / forgot-password.html
├─ policy.html / terms.html
└─ assets/
   ├─ css/style.css           사이트 전체 스타일 (섹션별 구획 주석)
   ├─ css/util.css            유틸리티
   └─ images/
```

헤더·푸터는 각 HTML에 인라인으로 들어가 있다. 헤더 마크업을 고칠 때는
**모든 HTML 파일에 동일하게** 반영해야 한다.

---

## 9-1. 이미지 무게 주의

폰 목업 화면 6장이 원본 PNG로 **합계 5.8MB**였다(`screen_dis.PNG` 1.7MB,
`screen_sijang.PNG` 1.6MB, `screen_mid.PNG` 1.0MB …). 실제 표시 폭은 338px인데
원본은 778px였고, 그래서 슬라이더가 첫 몇 바퀴 동안 빈 화면으로 돌았다.

폭 760px WebP(q .86)로 다시 인코딩해 **합계 410KB**로 줄였다(93% 감소).
원본 PNG는 `assets/images/`에 그대로 남아 있으나 어느 페이지도 참조하지 않는다 —
업로드 전에 지우면 10MB가 빠진다.

교훈: **표시 크기의 2배를 넘는 이미지는 넣지 않는다.** 모션이 아무리 정확해도
이미지가 안 뜨면 사용자는 모션 자체를 보지 못한다.

## 9-2. 텍스트 색 함정

`style.css`의 `body`는 예전 다크 테마 토큰을 그대로 쓴다 — `color: #eef0f4`(거의 흰색).
`.container h2`에는 색이 지정돼 있지 않아서, **밝은 배경 섹션을 새로 만들 때
그 섹션에 색을 지정하지 않으면 제목이 흰 배경에 흰 글씨로 나온다.**
새 섹션에는 항상 `color`를 명시할 것.

## 10. 체크리스트

- [ ] 섹션 상하 패딩 110~120px 유지
- [ ] H2는 800px 폭 제한 + 중앙정렬
- [ ] 카드 radius 16px, 버튼 radius 999px
- [ ] 진입 애니메이션은 1회만, 스태거 100~150ms
- [ ] 액센트 색 사용 면적 최소화
- [ ] 상단 메뉴 3개 초과 금지
- [ ] 블로그 카드 이미지 전체가 클릭 영역
- [ ] 넘버링 블록은 모바일에서도 sticky 유지
- [ ] 카드 이미지는 전부 같은 비율(16:10)로 고정 — 원본 크기 그대로 두면 카드 높이가 제각각이 되고 페이지가 늘어진다
- [ ] `prefers-reduced-motion` 대응
