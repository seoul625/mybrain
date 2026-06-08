# 📐 AI Process Visualization System Specification (Pixel Perfect Handoff)
## 🎯 프로젝트 목표 및 범위 확정
**목표:** 사용자가 자신의 프로세스에 존재하는 '구조적 손실(Structural Loss)'을 명확하게 인지하고, 이를 해결하기 위한 서비스 가입/구매 행동으로 유도하는 고효율의 인터랙티브 컴포넌트 구현.
**대상 환경:** Desktop (1440px 기준), Mobile (375px 기준).
**핵심 로직:** [AI Process Debugging Checklist] $\rightarrow$ [Loss Quantification Graph Update] $\rightarrow$ [CTA 유도].

---

## 🎨 1. 디자인 시스템 가이드라인 통합
### A. 컬러 팔레트 (Color Palette)
*   **Primary Color (신뢰/전문성):** `#2C3E50` (딥 네이비, 배경 및 주요 타이포그래피)
*   **Secondary Color (경고/주의):** `#E74C3C` (Warning Red, 손실 발생 지점, 위험 경고 텍스트)
*   **Accent Color (해결책/액션):** `#2ECC71` (에메랄드 그린, 성공적 진단, CTA 버튼의 기본 상태)
*   **Neutral Colors:** `#F4F6F8` (배경), `#34495E` (보조 타이포그래피).

### B. 타이포그래피 가이드라인 (Typography)
*   **Main Font:** Pretendard (산세리프, 높은 가독성 및 현대적 느낌).
    *   제목 (H1/H2): Bold / 32px - 48px
    *   본문 (Body): Regular / 16px - 18px

---

## 🖥️ 2. Desktop Mockup & Grid System Specification (1440px)
### A. 전체 레이아웃 구조 (Grid: 12-Column Responsive)
| 섹션 | 너비 (Grid) | 요소 배치 및 기능 | 참고 사항 |
| :--- | :--- | :--- | :--- |
| **Container** | 12 Col Max Width: 1200px | 중앙 정렬, 여백(Padding): 좌우 48px | 전체 콘텐츠가 이 그리드 내에서 작동. |
| **섹션 1:** 진단 결과 요약 (히어로) | 12 Col | `[Process Flow Diagram]` (좌: 6Col), `[Loss Graph Preview & CTA]` (우: 6Col) | 가장 시각적 충격이 커야 함. |
| **섹션 2:** AI 디버깅 체크리스트 | 12 Col | `[체크리스트 제목]`, 3개 컬럼의 카드 형태 (`Card Component`) 배치. | 각 카드는 클릭 가능한 인터랙티브 요소여야 함. |
| **섹션 3:** 손실 누적 시각화 (핵심) | 12 Col | 중앙에 거대한 그래프 영역. 좌/우로 설명 패널 분리. | 이 컴포넌트의 로직이 핵심. |

### B. 상세 컴포넌트 명세: Loss Accumulation Graph
*   **요소:** 인터랙티브 라인 차트 (Line Chart) + 데이터 사일로(Data Silo) 이미지.
*   **상태 1: 초기 진입 (Initial State):** 그래프는 평평하고 완만한 곡선으로 시작하며, '현재 예상 손실 범위'만 표시됨 (옅은 회색).
*   **상태 2: 체크리스트 검증 (Interactive Trigger):** 사용자가 [AI Process Debugging Checklist]의 특정 항목(예: '데이터 전처리 과정 누락')을 클릭하거나 확인하면, 해당 데이터가 트리거를 발생시킴.
    *   **트리거 애니메이션:** 체크박스에 초록색 액센트 (`#2ECC71`)가 번쩍임. 동시에 그래프의 특정 지점(X축)에서 수직으로 'Warning Red' 색상의 파동이 솟아오름.
    *   **시각적 피드백:** 그래프 상에 해당 손실 유형을 나타내는 사일로 이미지와 함께 `+15% Loss` 텍스트가 **애니메이션(Fade-in + Scale-up)**으로 추가됨.
*   **상태 3: 최종 누적 (Final State):** 모든 항목이 검증되고 그래프가 완성된 후, 총 손실 대비율을 강조하며 가장 높은 지점에 '최대 리스크 구간' 배너를 표시함.

---

## 📱 3. Mobile Mockup & Grid System Specification (375px)
*   모든 그리드 시스템은 단일 컬럼(1 Col)로 재배치되며, 가독성을 위해 섹션 간 여백을 최대화합니다.
*   **섹션 2 (체크리스트):** 3개 카드가 2열 배치 (`2-Column Grid`)를 기본으로 하되, 화면 크기 축소 시 자동으로 1열로 떨어지도록 합니다.
*   **핵심 로직:** 그래프와 체크리스트의 연결은 스크롤 기반 애니메이션을 사용합니다. (사용자가 체크리스트 카드를 끝까지 스크롤하면, 그 결과가 바로 아래 Graph 섹션에 반영되는 효과).

---

## 💻 4. 개발자를 위한 인터랙션 및 타이밍 가이드라인 (Critical Path)
### A. 전체 플로우와 애니메이션 시퀀스 (Timeline: Total Duration ~12s)
| 시간대 | 액션 주체 | 이벤트 / 로직 | CSS/애니메이션 기법 | 비고 |
| :--- | :--- | :--- | :--- | :--- |
| **T=0.0s** | 시스템 | 페이지 진입, 초기 그래프 표시 (상태 1) | `opacity: 0` $\rightarrow$ `opacity: 1` (Fade-in) | 손실 없음, 낮은 리스크 상태로 시작. |
| **T=1.5s** | 사용자 | [Checklist Item A] 클릭/활성화 | 체크박스: Green Glow (`#2ECC71`) / 버튼: Scale-up(1.0 $\rightarrow$ 1.1) | 성공적 진단 단계 시작. |
| **T=3.0s** | 시스템 | 손실 데이터 계산 (API 호출 시뮬레이션) | 로딩 스피너 + 텍스트 업데이트 (`Calculating...`) | 사용자가 잠시 집중하는 시간 부여. |
| **T=4.5s** | 시스템 | **[Loss Graph Update]** 발생 | 그래프: `transform: translateY(0)` (Y축 이동 애니메이션). 신규 사일로: Fade-in + Scale-up. | 경고 색상 (`#E74C3C`) 사용을 필수로 강조. |
| **T=6.0s** | 시스템 | 다음 체크리스트 항목 트리거 | 이전 손실 값에 기반하여, 새로운 손실 값이 추가되는 연쇄적 애니메이션 구현. | 누적이 눈에 보이게 설계하는 것이 중요함. |
| **T=8.0s** | 시스템 | 모든 데이터 로딩 완료 (상태 3) | 그래프: 전체적인 크기 확대(Zoom-in 효과)와 함께 최종 리스크 요약 배너 고정. | 가장 강력한 시각적 피크 지점. CTA가 이 위에 오버레이 되어야 함. |

### B. 필수 개발 태그 및 변수
*   **Data Point:** `[Loss_A]` (손실 값), `[Source_A]` (누적 원인 설명).
*   **Animation Library:** GSAP 또는 Framer Motion 사용 권장.
*   **CSS Transition:** 모든 상호작용은 최소 0.3s 이상의 부드러운 트랜지션(Transition)을 적용하여 '기술적인' 느낌을 줄 것.

---