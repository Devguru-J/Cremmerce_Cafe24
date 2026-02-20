# Cremmerce Cafe24 작업 기록

## 2026-02-20

---

## 1. Sticky 헤더 — 로고 축소 애니메이션

**파일:** `skin1/layout/basic/css/layout.css`

### 작업 내용
스크롤 시 헤더가 고정(sticky)될 때 CREMMERCE 로고가 자연스럽게 축소되고, 내비게이션 메뉴가 함께 따라가는 애니메이션 구현.

### 핵심 원리
- `#header.fixed`에 `translateY(-50px)` 적용 → 상단 toparea(50px) 숨김
- toparea의 `margin-bottom: 20px`이 자연스러운 로고 상단 여백으로 작동
- **`translateY(-70px)` 사용 금지** — 폰트 클리핑 + 아이콘이 뷰포트 최상단에 붙는 문제 발생

### 추가/변경된 CSS

```css
/* layout.css — .top_logo 기본 (line ~70) */
#header .inner .top_nav_box .top_logo {
    transition: transform 0.35s ease, top 0.35s ease;
}

/* layout.css — PC 전용 @media all and (min-width:1025px) */

/* 트랜지션 (sticky 전환 시 애니메이션) */
#header .inner .top_nav_box .top_mypage {
    transition: height 0.35s ease, margin-top 0.35s ease;
}
#header .inner .top_nav_box .top_category {
    transition: margin-top 0.35s ease;
}
#header .inner .top_nav_box .top_category > ul > li {
    transition: height 0.35s ease;
}
#header .inner .top_nav_box .top_category > ul > li > a {
    transition: line-height 0.35s ease;
}

/* Sticky 상태 */
#header.fixed .inner .top_nav_box                          { padding-bottom: 0; }
#header.fixed .inner .top_nav_box .top_logo                { transform: translateX(-50%) scale(0.6); top: 0; }
#header.fixed .inner .top_nav_box .top_mypage              { height: 20px; margin-top: 0; }
#header.fixed .inner .top_nav_box .top_category            { margin-top: 14px; }
#header.fixed .inner .top_nav_box .top_category > ul > li  { height: 28px; }
#header.fixed .inner .top_nav_box .top_category > ul > li > a { line-height: 28px; font-size: 14px; }
```

### 참고 수치
| 항목 | 값 |
|------|----|
| PC 헤더 높이 (일반) | `191px` |
| PC 헤더 높이 (sticky 후) | `~104px` |
| 모바일 헤더 높이 | `59px` |
| main.css 카테고리탭 sticky top | `top: 104px` |

---

## 2. 로고 모핑 애니메이션 — CREATOR+ECOMMERCE → CREMMERCE

### 배경
- Cremmerce = **creator + ecommerce** 합성어를 로고 애니메이션으로 표현
- Cafe24 관리자에서 SVG 파일 업로드 불가 (JPG만 지원)
- 기존 `/svg/` 디렉토리의 `<!--@import()-->` 패턴 활용

### 변경된 파일

#### `skin1/layout/basic/header.html` — line 64

```html
<!-- 변경 전 -->
<div class="top_logo" module="Layout_LogoTop">
    <a href="/"><img src="{$logo}" alt="{$mall_name}" data-ez-eb="..."></a>
</div>

<!-- 변경 후 -->
<div class="top_logo">
    <a href="/"><!--@import(/svg/logo.html)--></a>
</div>
```

> `module="Layout_LogoTop"` 제거 → Cafe24 관리자 패널의 로고 변경 UI 비활성화 (의도적)
> 로고 변경 시 `skin1/svg/logo.html` 파일 직접 수정 필요

#### `skin1/svg/logo.html` — 신규 생성

```html
<span class="logo-morph" role="img" aria-label="CREMMERCE">
    <span class="lk" style="--ai:0s">C</span>
    <span class="lk" style="--ai:.05s">R</span>
    <span class="lk" style="--ai:.1s">E</span>
    <span class="lg" style="--ai:.15s;--di:1.6s">A</span>
    <span class="lg" style="--ai:.2s;--di:1.7s">T</span>
    <span class="lg" style="--ai:.25s;--di:1.8s">O</span>
    <span class="lg" style="--ai:.3s;--di:1.9s">R</span>
    <span class="lg" style="--ai:.35s;--di:2s">+</span>
    <span class="lg" style="--ai:.4s;--di:2.1s">E</span>
    <span class="lg" style="--ai:.45s;--di:2.2s">C</span>
    <span class="lg" style="--ai:.5s;--di:2.3s">O</span>
    <span class="lk" style="--ai:.55s">M</span>
    <span class="lk" style="--ai:.6s">M</span>
    <span class="lk" style="--ai:.65s">E</span>
    <span class="lk" style="--ai:.7s">R</span>
    <span class="lk" style="--ai:.75s">C</span>
    <span class="lk" style="--ai:.8s">E</span>
</span>
```

- `.lk` (keep): 남는 글자 — C, R, E, M, M, E, R, C, E
- `.lg` (gone): 사라지는 글자 — A, T, O, R, +, E, C, O
- `--ai`: 등장 딜레이 / `--di`: 사라짐 딜레이 (CSS custom property)

### 애니메이션 타임라인

```
0s ──────────── 1.2s ── 1.6s ──────────────────── 3.0s
│                │       │                          │
│ CREATOR+ECOMMERCE      ATOR+ECO 순서대로 fade+collapse
│ 17글자 순차 등장        A→T→O→R→+→E→C→O
│ (50ms 간격)
                        최종: CREMMERCE
```

### 핵심 CSS 기법

```css
/* 사라지는 글자: opacity fade 후 max-width로 공간 collapse */
@keyframes logoOut {
    0%   { opacity: 1; max-width: 1.2em; margin-right: 2px; }
    45%  { opacity: 0; max-width: 1.2em; margin-right: 2px; }
    100% { opacity: 0; max-width: 0;     margin-right: 0;   }
}

/* 같은 요소에 두 애니메이션 순차 적용 */
.logo-morph .lg {
    animation:
        logoIn  0.4s ease var(--ai) both,      /* 등장 */
        logoOut 0.7s ease var(--di) forwards;   /* 사라짐 */
}
/* CSS 규칙: 나중에 선언된 애니메이션(logoOut)이 같은 속성에서 우선권 가짐 */
```

### 반응형 처리

```css
/* 모바일에서 CREATOR+ECOMMERCE 17글자가 화면 밖으로 넘치는 문제 해결 */
@media screen and (max-width: 1024px) {
    .logo-morph { font-size: 25px; } /* PC: 40px */
}
/* em 기반 max-width가 font-size에 비례하여 자동 축소됨 */
```

### 접근성

```css
/* 애니메이션 민감 사용자: 즉시 최종 결과(CREMMERCE)만 표시 */
@media (prefers-reduced-motion: reduce) {
    .logo-morph .lk { animation: none; opacity: 1; }
    .logo-morph .lg { display: none; }
}
```

---

## 변경 파일 요약

| 파일 | 변경 유형 | 내용 |
|------|-----------|------|
| `skin1/layout/basic/css/layout.css` | 수정 | Sticky 헤더 로고 축소 애니메이션 |
| `skin1/layout/basic/css/main.css` | 수정 | 카테고리탭 sticky top 104px |
| `skin1/layout/basic/header.html` | 수정 | `<img>` → `<!--@import(/svg/logo.html)-->` |
| `skin1/svg/logo.html` | **신규 생성** | CREATOR+ECOMMERCE → CREMMERCE 모핑 애니메이션 |
