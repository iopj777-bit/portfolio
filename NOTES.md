# 포트폴리오 사이트 작업 노트 (인수인계용)

> 이 문서는 claude.ai에서 진행하던 작업을 VS Code(Claude Code)로 이어가기 위한 인수인계 노트입니다.
> Claude Code는 이 파일을 먼저 읽고 `index.html` 구조를 파악한 뒤 작업을 이어가면 됩니다.

---

## 프로젝트 개요

- **누구:** 서비스 기획자 조가인(Cho Ga In)의 취업용 포트폴리오
- **형태:** 순수 HTML + CSS (Next.js 아님, 프레임워크 없음)
- **배포:** GitHub → Vercel 자동 배포 (푸시하면 자동 반영)
- **폰트:** Pretendard(한글), JetBrains Mono(영문 라벨)
- **작업 방식 선호:** 빠른 실행 > 긴 설명. 한국어 ~습니다체. 줄바꿈은 의미 단위로만.

---

## 디자인 토큰 (CSS :root)

```
--ink:#0B0B0C;      /* 기본 텍스트 */
--blue:#1B45D7;     /* 포인트 색 (단색 강조) */
--paper:#FFFFFF;
--mist:#F3F4F8;     /* 연한 회색 배경 */
--line:#DCDEE5;     /* 테두리 */
--muted:#71757F;    /* 흐린 텍스트 */
--gut:clamp(20px,4vw,56px);  /* 좌우 여백 */
--max:1720px;       /* 본문 최대폭 */
```

**디자인 원칙:** 흰 배경 + 파란 단색 포인트. 미니멀. 여러 색 쓰지 않기. 과한 장식 X.

---

## 페이지 구조 (index.html)

1. 헤더 (sticky nav)
2. 히어로 — 타이틀 "사용자 언어로 번역하는 서비스 기획자" + 프로필 사진 + 강점 3축 띠(해석력/설계력/수행력)
3. About Me — 소개 + 연락처 / 경력·전문분야·스킬·키워드
4. Key Projects — 주요 3개 (클릭하면 상세 팝업 열림)
5. All Projects — 전체 프로젝트 리스트
6. Contact + 푸터
7. 프로젝트 상세 팝업 3개 (풀스크린 모달)
8. 플로팅 PDF 버튼

---

## 프로젝트 상세 팝업 (핵심 구조)

**작동 방식:** Key Projects의 각 카드에 `onclick="openModal('m-hf')"` 식으로 연결.
풀스크린 모달이 뜨고, X 버튼 / ESC / 하단 닫기 버튼으로 닫힘.

**팝업 3개 id:**
- `m-hf` — 한국주택금융공사 (내용 채워짐 ✅)
- `m-president` — 21대 대통령실 (내용 미완성, [대괄호] placeholder)
- `m-kofa` — 한국영상자료원 (내용 미완성, [대괄호] placeholder)

**팝업 내부 구조 (ver3):**
```
.modal
  .modal-bar (sticky, 프로젝트명 라벨 + X버튼)
  .md-inner (max-width:1560px)
    .md-head
      .md-title (제목, org명은 파랑)
      .md-head-row (좌: .md-tags 태그 / 우: .md-meta 요약정보 — 윗선 정렬)
      .md-cover (대표 이미지, 16:7)
      [HF만] .md-cover-note (AI 생성 안내 문구)
    .md-bg (프로젝트 배경)
    .md-problems (문제점 — .prob-card 넘버링 카드, 개수 유동적)
  .md-inner
    .md-design (핵심설계 — .dz 좌우 지그재그, .dz.flip으로 순서 교대)
      .dz-text (h4 제목 + p 설명)
      .dz-img (이미지 자리, 4:3)
    .md-foot (닫기 버튼)
```

**중요 CSS 값:**
- `.md-inner{max-width:1560px}` — 팝업 콘텐츠 폭
- `.dz-text h4{font-size:1.5rem}` / `.dz-text p{font-size:1.15rem}` — 핵심설계 텍스트
- `.md-bg p{font-size:1.12rem}` — 배경 텍스트
- `.prob-grid` — `auto-fit,minmax(200px,1fr)`로 카드 개수 자동 대응
- 문제점 카드는 `.prob-card`에 flex column + `align-items:stretch`로 높이 통일

---

## ⚠️ 보안 주의 (HF 프로젝트)

**한국주택금융공사(HF)는 오픈 전 서비스라 실제 화면 공개 불가.**
- 대표 이미지·핵심설계 이미지 모두 **AI 생성 또는 추상 목업**으로 대체
- 팝업에 안내 문구 있음: "보안상 실제 화면을 공개할 수 없어 AI로 생성한 이미지로 대체했습니다"
- **실제 HF UI를 그대로 재현하지 말 것.** 개념/구조만 추상적으로.
- 특히 "나이대별 배너 차이" 같은 세부 기획은 이미지에 넣지 말고 면접에서 구두 설명용으로 남김
- 대통령실·영상자료원은 실화면 사용 가능 (제약 없음)

---

## 이미지 자산 & 사이즈 가이드

**폴더 구조:** `images/` 아래에 이미지. 썸네일은 `images/썸네일/`

**사이즈:**
- 대표 이미지(`.md-cover`): 16:7 → 1600×700
- 핵심설계 이미지(`.dz-img`): 4:3 → 1200×900
- 리스트 썸네일(`.thumb`): 16:9

**형식:** 사진·목업=JPG(300KB 이하), 다이어그램·도형=투명 PNG.
**둥근 모서리는 CSS가 처리** → GPT엔 사각형으로 요청. 이미지 자체에 둥근 모서리 넣지 말 것.

**이미 만든 자산 (이 노트와 함께 제공):**
- `hf_diagram.svg` / `hf_diagram.png` — HF 채널 역할 삼각형 다이어그램 (대표홈/인터넷금융/APP 3꼭짓점 + 가운데 HF 로고). 투명 배경.
- `hf_logo_color.png` — HF 컬러 로고 (투명 배경), 삼각형 다이어그램에 임베드됨

**삼각형 다이어그램 색 바꾸기:** `hf_diagram.svg` 맨 위 `<style>`에서
`.tri{stroke:#8A8D94}` (선), `.title{fill:#1A1A1A}` (제목), `.desc{fill:#6B6E76}` (설명) 세 값만 수정.
현재 회색 톤. 사이트 파랑(#1B45D7)으로 바꿔도 됨.

---

## SVG 만들기 (Claude Code에서 가능)

Claude Code도 Python + cairosvg로 SVG 생성/PNG 변환 가능. 방식:
```bash
pip install cairosvg --break-system-packages
# 한글 렌더하려면 Pretendard 폰트 설치 필요:
#   ~/.fonts/ 에 Pretendard-Bold.otf, Pretendard-Regular.otf 넣고 fc-cache -f
python3 -c "import cairosvg; cairosvg.svg2png(url='x.svg', write_to='x.png', output_width=1280, output_height=1040)"
```
- SVG 주석에 `--` 문자 넣지 말 것 (XML 파싱 에러남)
- 한글 파일을 Python으로 쓸 때 `unicode_escape` 쓰지 말 것 (한글 깨짐). bash `cat > EOF` 또는 직접 문자열로.

---

## 남은 할 일 (TODO)

### 내용 채우기
- [ ] **대통령실 팝업** (`m-president`) — 배경/문제점/핵심설계 [대괄호] 채우기
- [ ] **영상자료원 팝업** (`m-kofa`) — 배경/문제점/핵심설계 [대괄호] 채우기 (실화면 캡처 가능)
- [ ] 각 팝업 참여도(%) 입력

### 이미지
- [ ] HF 대표 배너 (16:7) — 폰+데스크톱 모니터, 블러 처리, navy. GPT 생성 예정
- [ ] HF 리스트 썸네일 (16:9) — 위와 같은 톤. 현재 실화면이라 교체 필요
- [x] HF 핵심설계 이미지들 (4:3):
  - [1] 삼각형 다이어그램 → `hf_diagram.png` 적용 완료 (유일한 실제 이미지 파일)
  - [2] 맞춤형 화면 → HTML/CSS 목업(`.mock-custom`)으로 적용 완료 (상태별 신청전/중/이용중 스켈레톤 폰 3개)
  - [3] 신청절차 개선 → HTML/CSS 목업(`.mock-apply`)으로 적용 완료 (Before: 하위 단계 은닉 / After: STEP 2·4 표시기 + KRDS 적용 컴포넌트 칩 목록)
  - [4] 모바일 최적화 → HTML/CSS 목업(`.mock-mobile`)으로 적용 완료 (바텀시트/풀팝업/캘린더 UI 패턴)
  - ⚠️ **[2][3][4]는 SVG가 아니라 순수 HTML/CSS(div)로 구현되어 있음.** 처음엔 SVG로 만들었으나, `<img>`로 불러온 SVG는 페이지의 Pretendard 웹폰트를 못 쓰고 브라우저가 대체 폰트로 폴백하면서 일부 한글 글자가 깨져 렌더링되는 문제가 있었음(예: "하위"→"하쥐" 식으로 오검색). HTML/CSS로 다시 만들어 페이지 폰트를 그대로 쓰게 해서 해결. **앞으로 이 3개 다이어그램에 텍스트를 추가/수정할 땐 SVG가 아니라 `index.html`의 해당 `.mock-*` 마크업과 CSS(“HF 핵심설계 목업” 섹션, `.dz-img` 근처)를 직접 편집할 것.**
  - `.mock-apply`는 콘텐츠가 많아 `.dz-img.tall`(aspect-ratio 해제)을 추가로 붙여둠 — 4:3 박스에 넣으면 내부 스크롤 생기니 유지할 것.
  - cairosvg로 PNG 변환하려 했으나 이 Windows 환경엔 libcairo 네이티브 라이브러리가 없어 실패했음(참고용 — 지금은 SVG 자체를 안 쓰므로 무관).
- [ ] 대통령실·영상자료원 이미지
- [ ] profile.jpg, icon-1~3.png, 스킬 로고들(ppt/excel/claude/slack/notion)

### 다이어그램 넣기
- [ ] `hf_diagram.png`를 `images/`에 넣고, HF 팝업 첫 번째 `.dz-img`를 이미지로 교체:
  ```html
  <div class="dz-img" style="background:#fff"><img src="images/hf_diagram.png" alt="채널 역할 다이어그램"></div>
  ```
  (흰 배경 style 권장 — 투명 다이어그램이 흰 배경 위에 얹힘)
- [ ] 삼각형 색 회색 vs 파랑 최종 결정

### 기타
- [ ] PDF/이력서 링크 연결 (현재 `href="#"`)
- [ ] 프로젝트 기간·명칭 사실 확인 (KRDS 등)

---

## HF 핵심설계 텍스트 (확정본 — 참고)

1. **채널 역할 재정립** (삼각형 다이어그램 자리)
   > 각 채널의 명확한 역할 재정립을 기반으로 IA를 설계해 고객 편의성을 확립했습니다.
   > 채널 별 특성과 주요 이용 목적을 고려해 IA를 설계하고, 각 채널의 역할에 적합한 신규 서비스를 구성하여 서비스 접근성과 이용 편의성을 높였습니다.

2. **맞춤형 화면**
   > 고객의 주요 이용 목적을 고려해 맞춤형 화면을 구성했습니다.
   > 고객의 주요 이용 목적과 서비스 이용 패턴을 고려해 핵심 서비스 접근성을 높이고, 사용자에게 필요한 정보를 중심으로 효율적인 화면 경험을 제공했습니다.

3. **신청절차 개선 + KRDS**
   > 타 금융사 벤치마킹으로 신청절차 개선사항을 도출하고, KRDS의 사용자 중심 설계 원칙을 반영해 UI/UX를 개선했습니다.
   > 타 금융사의 신청 과정을 비교·분석하여 신청 단계별 정보와 입력 항목을 중심으로 개선사항을 도출하고, 사용자가 신청 흐름을 쉽게 이해할 수 있도록 직관적인 화면 구조와 UI/UX를 설계했습니다.

4. **모바일 최적화**
   > 모바일 이용 특성을 고려한 UI/UX를 설계해 모바일 환경에서의 이용 편의성을 강화했습니다.
   > 모바일 환경에서의 이용 편의성을 고려해 주요 UI 패턴을 적용하고, 타 금융기관 벤치마킹을 통해 바텀시트·풀팝업·캘린더 UI 등을 활용하여 모바일 특성에 맞는 화면과 인터랙션을 설계했습니다.

**HF 문제점 4카드:** ①채널 별 역할 구분 미흡 ②주요 기능 분산 ③복잡한 신청 프로세스 ④모바일 최적화 미흡
