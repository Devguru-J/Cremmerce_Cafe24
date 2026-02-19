# Cremmerce Cafe24 FTP - 작업 기록

## 프로젝트 개요
- **쇼핑몰명:** Cremmerce (크리머스)
- **플랫폼:** Cafe24 스마트디자인 Easy
- **스킨 경로:** `skin1/`
- **GitHub:** https://github.com/Devguru-J/Cremmerce_Cafe24
- **VS Code SFTP:** `uploadOnSave: true` — Claude Code의 Edit 후 반드시 VS Code에서 **Cmd+S** 저장 필요

---

## 주요 파일 경로

| 파일 | 역할 |
|------|------|
| `skin1/layout/basic/css/common.css` | 전역 리셋 CSS |
| `skin1/layout/basic/css/layout.css` | 레이아웃 구조 CSS |
| `skin1/layout/basic/css/main.css` | 메인 페이지 전용 CSS |
| `skin1/layout/basic/css/sub_style.css` | 서브 페이지 CSS |
| `skin1/layout/basic/css/slideMenu.css` | 모바일 햄버거 메뉴 CSS |
| `skin1/layout/basic/js/main.js` | 메인 페이지 JS (Swiper, 탭 등) |
| `skin1/layout/basic/layout.html` | 전체 레이아웃 템플릿 (전역 CSS 포함) |
| `skin1/index.html` | 메인 페이지 (Easy 모듈 구조) |
| `skin1/product/detail.html` | 상품 상세 페이지 템플릿 |

---

## 작업 이력

### word-break 적용
- **파일:** `common.css` line 9, `main.css`
- **내용:** `body`에 `word-break:keep-all; overflow-wrap:break-word;` 추가
- `.main_title_txt02`, `.main_banner_txt01` 에도 개별 적용

---

### #wrap overflow:clip 적용
- **파일:** `layout.css` line 4
- **내용:** `overflow:hidden` → `overflow:hidden;overflow:clip;`
- **이유:** `overflow:hidden`이 scroll container를 만들어 `position:sticky` 작동 방해 → `overflow:clip`으로 sticky 허용
- **주의:** VS Code CSS 린터가 `overflow:clip` 미인식 오류 표시 → 실제 브라우저 오류 아님

---

### Curation by Category 탭 Sticky 적용
- **파일:** `main.css`
- **PC (데스크톱):**
  ```css
  .main_product_category .main_product_inner {
      position:sticky; top:211px; align-self:flex-start;
  }
  /* top:211px = 고정 헤더 191px + 여백 20px */
  ```
- **모바일 (≤1024px):**
  ```css
  .main_product_category .main_product_inner {
      position:sticky; top:59px; align-self:flex-start;
      z-index:10; background:#F6F5F1;
      padding-top:20px; padding-bottom:12px;
  }
  /* top:59px = 모바일 헤더 높이 */
  ```
- **헤더 높이:** PC `191px` (position:fixed), 모바일 `59px` (position:fixed)
- **탭 활성화 색상:** `background:#333333; color:#EBE9E1`

---

### 이달의 크리에이터 모바일 2열 레이아웃
- **파일:** `main.css` (모바일 미디어쿼리 내)
- **타겟:** `data-ez="contents-12r96jd-1"` (해당 섹션 고유 ID)
- **내용:**
  ```css
  .section[data-ez="contents-12r96jd-1"] .main_3dan_banner ul {
      flex-direction:row; flex-wrap:wrap; margin:0 -8px !important; width:calc(100% + 16px) !important;
  }
  .section[data-ez="contents-12r96jd-1"] .main_3dan_banner ul li {
      width:50%; max-width:50%; flex:0 1 50%; padding:0 8px; margin:0 0 24px;
  }
  ```

---

### 탭 하이라이트 색상 변경
- **파일:** `main.css`
- **기존:** `#f4f4f4` → **변경:** `#EBE9E1` → **최종:** `#333333` (배경), `#EBE9E1` (텍스트)
  ```css
  .main_product_category .main_product_inner .main_product_tab li.active {background-color:#333333;}
  .main_product_category .main_product_inner .main_product_tab li.active .button {color:#EBE9E1;}
  ```

---

### 모바일 햄버거 메뉴 배경색 변경
- **파일:** `slideMenu.css` line 7
- **변경:** `#aside { background-color:#fcfcfc; }` → `#F6F5F1`

---

### 헤더 스크롤 transition 추가
- **파일:** `layout.css` line 38
- **내용:** `#header { transition:transform 0.35s ease; }` 추가
- **이유:** `.fixed` 클래스 적용 시 `translateY(-50px)` 가 즉각 적용되어 어색하게 스냅되는 문제 해결

---

### 제품 상세 모바일 이미지 페이지네이션 배경 제거
- **파일:** `sub_style.css` (모바일 미디어쿼리)
- **내용:** `.ec-base-paginate.typeSwipe { background:transparent !important; }`
- **이유:** 이미지 슬라이더 하단 페이지네이션 컨테이너에 배경색이 있어 흰줄처럼 보이는 문제 해결

---

### 푸터 정보 모바일 상시 표시
- **파일:** `layout.css` (모바일 미디어쿼리)
- **내용:**
  ```css
  #footer .tablet_fold {display:block !important; width:100%; max-width:none;}
  .bt_top ul > li .bt_dummy {display:none !important;}
  .bt_top ul > li .icon[class*="icoPlus"] {display:none !important;}
  ```
- **이유:** 모바일에서 쇼핑몰 기본정보/결제정보/SNS가 접혀있어 토글 버튼 클릭 필요 → 상시 표시로 변경

---

## 컬러 팔레트

| 이름 | HEX | 용도 |
|------|-----|------|
| crem-bg | `#F6F5F1` | 사이트 전체 배경 |
| crem-text | `#2E2E2E` | 기본 텍스트 |
| crem-champagne | `#D6C9A3` | 보조색, 구분선 |
| crem-footer | `#EBE9E1` | 푸터 배경, 탭 활성 텍스트 |
| tab-active-bg | `#333333` | 탭 활성화 배경 |

---

## 주의사항

1. **VS Code SFTP `uploadOnSave`**: Claude Code의 Edit 도구로 파일 수정 후 반드시 VS Code에서 파일 열고 **Cmd+S** 저장해야 서버에 업로드됨
2. **`overflow:clip`**: VS Code CSS 린터에서 오류 표시되나 실제 브라우저에서 정상 작동
3. **헤더 높이**: PC `191px`, 모바일 `59px` — sticky top 값 계산 시 기준
4. **`data-ez` 특정 섹션 타겟팅**: 섹션 고유 ID가 변경될 경우 CSS도 업데이트 필요
   - 이달의 크리에이터: `contents-12r96jd-1`
