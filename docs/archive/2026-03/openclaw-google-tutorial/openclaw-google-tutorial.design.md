# Design: OpenClaw + Google Cloud 튜토리얼 웹교안

## 1. 아키텍처

### 단일 HTML 파일 구조
```
index.html
├── <head> - 메타태그, CSS 변수, 전체 스타일
├── <body>
│   ├── Sidebar Navigation (좌측 고정)
│   ├── Main Content Area
│   │   ├── Hero Section (인트로)
│   │   ├── Step 1~8 Sections
│   │   └── Outro Section
│   ├── Progress Bar (상단 고정)
│   ├── Theme Toggle (우상단)
│   └── Back to Top Button
└── <script> - 바닐라 JS
```

## 2. UI/UX 설계

### 2.1 레이아웃
- **사이드바**: 280px 고정, 현재 단계 하이라이트, 스크롤 추적
- **메인 콘텐츠**: max-width 800px, 중앙 정렬
- **상단 프로그레스바**: 스크롤 위치 기반 진행률 표시
- **모바일**: 사이드바 → 햄버거 메뉴로 전환 (768px 미만)

### 2.2 색상 시스템 (CSS 변수)

```css
/* Light Mode */
--bg-primary: #ffffff;
--bg-secondary: #f8f9fa;
--bg-code: #1e1e2e;
--text-primary: #1a1a2e;
--text-secondary: #4a4a6a;
--accent: #4f46e5;        /* 인디고 */
--accent-light: #818cf8;
--success: #10b981;
--warning: #f59e0b;
--danger: #ef4444;
--info: #3b82f6;
--border: #e2e8f0;

/* Dark Mode */
--bg-primary: #0f0f1a;
--bg-secondary: #1a1a2e;
--bg-code: #0d0d1a;
--text-primary: #e2e8f0;
--text-secondary: #94a3b8;
--accent: #818cf8;
--border: #2d2d44;
```

### 2.3 타이포그래피
- **제목**: system-ui, -apple-system, sans-serif
- **본문**: 16px/1.8 line-height
- **코드**: 'SF Mono', 'Fira Code', monospace, 14px
- **Step 번호**: 72px bold (장식적 숫자)

### 2.4 컴포넌트 설계

#### Step Card
```
┌─────────────────────────────────────────┐
│  STEP 03                                │
│  ─────────────────────────              │
│  구글 클라우드 설정                        │
│                                          │
│  설명 텍스트...                            │
│                                          │
│  ┌─────────────────────────────────┐    │
│  │ 📋 이미지 플레이스홀더             │    │
│  │ [구글 클라우드 콘솔 화면]          │    │
│  └─────────────────────────────────┘    │
│                                          │
│  ┌─ 💡 TIP ────────────────────────┐    │
│  │ 팁 내용                          │    │
│  └─────────────────────────────────┘    │
│                                          │
│  ┌─ 📋 복사하기 ───────────────────┐    │
│  │ $ npx [스킬 설치 명령어]          │ 📋 │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

#### Callout 박스 종류
| 타입 | 아이콘 | 색상 | 용도 |
|------|--------|------|------|
| tip | 💡 | accent | 유용한 정보 |
| warning | ⚠️ | warning | 주의사항 |
| danger | 🚨 | danger | 보안 경고, 절대 하지 말 것 |
| info | ℹ️ | info | 참고 정보 |
| success | ✅ | success | 완료 확인 |

#### 코드 블록
- 다크 배경 (항상)
- 우측 상단 복사 버튼
- 복사 시 "복사됨!" 토스트 표시
- 언어 라벨 (bash, json 등)

#### 이미지 플레이스홀더
- 점선 테두리, 회색 배경
- 중앙에 카메라 아이콘 + 설명 텍스트
- 실제 이미지 교체 가이드 주석 포함

## 3. 콘텐츠 상세 설계

### Step 0: 인트로
- OpenClaw 4가지 핵심 요소 카드 (그리드)
- 구글 연동 장점 요약
- 무료 크레딧 안내
- 영상 완료 후 가능한 것들 리스트

### Step 1: 환경 준비
- AntiGravity(VS Code) 설치 링크
- WSL 연결 방법 (Windows 사용자)
- 워크스페이스 폴더 생성 명령어
- 터미널 열기 + 전체화면 설정

### Step 2: OpenClaw 스킬 다운로드
- 명령어: GCP OpenClaw 스킬 설치 명령어 실행
- 폴더 구조 정리 안내
- AntiGravity 패널 설정

### Step 3: 구글 클라우드 설정
- 프로젝트 생성 (이름: openclaw)
- API 활성화 5개: Gmail, Calendar, Sheets, Docs, Drive
- OAuth 동의 화면 구성
- 데이터 액세스 범위 설정
- 클라이언트 ID/Secret 생성 및 다운로드
- `.keys` 폴더 저장

### Step 4: 결제계정 등록
- 무료 크레딧 안내
- 결제 계정 생성
- 프로젝트 연결

### Step 5: GCP VM 인스턴스 생성
- Compute Engine 활성화
- 인스턴스 사양: e2-small, Ubuntu, 25GB
- 프로젝트 ID 확인

### Step 6: gog-cli와 OpenClaw 설치
- VM SSH 접속
- OpenClaw 설치
- gog-cli 설치 + 인증
- 인증 키 JSON 복사 (SCP)

### Step 7: OpenClaw 및 텔레그램 봇 설정
- `openclaw onboard` 실행
- Google Auth 선택 + Gemini 3 Flash 선택
- 텔레그램 BotFather에서 봇 생성
- 봇 토큰 입력
- ClawHub 스킬 설치
- 퍼소나 설정

### Step 8: 구글 서비스 활용
- 메일 관리 (광고 메일 삭제)
- 캘린더 일정 등록 + 리마인더
- 뉴스 자동화 크론 등록
- 구글 시트 데이터 저장
- 구글 독스 문서 작성
- 이메일 발송

## 4. JavaScript 기능 명세

| 기능 | 구현 방식 |
|------|----------|
| 코드 복사 | `navigator.clipboard.writeText()` + 토스트 |
| 다크모드 토글 | CSS 변수 전환 + localStorage 저장 |
| 프로그레스바 | `scroll` 이벤트 + `scrollTop/scrollHeight` 계산 |
| 사이드바 하이라이트 | `IntersectionObserver` 활용 |
| 부드러운 스크롤 | `scrollIntoView({ behavior: 'smooth' })` |
| 모바일 메뉴 토글 | 햄버거 버튼 + 사이드바 슬라이드 |
| Back to Top | 스크롤 300px 이상 시 표시 |

## 5. 구현 순서

1. HTML 기본 구조 + CSS 변수/레이아웃
2. 사이드바 네비게이션
3. Step 0~8 콘텐츠 섹션
4. 코드 블록 + 복사 기능
5. Callout 박스
6. 이미지 플레이스홀더
7. 다크모드 토글
8. 프로그레스바 + 스크롤 추적
9. 모바일 반응형
10. 최종 polish
