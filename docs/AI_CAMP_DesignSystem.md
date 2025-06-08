# 🎨 6. AI CAMP × 경영지도센터 UI/UX 디자인 시스템 & 가이드

## 📘 목차
- 디자인 시스템 개요
- 컬러 팔레트 (TailwindCSS 기준)
- 페이지별 디자인 가이드
- 레이아웃 구성 요소
- 인터랙션 패턴
- 반응형 브레이크포인트 정의

---

## 1. 디자인 시스템 개요

| 요소 | 설명 |
| --- | --- |
| 스타일 | 모던하고 미래지향적인 UI, 곡선 엣지 강조, 블록형 레이아웃 적용 |
| 컬러 스킴 | 아날로그 색조 기반 (딥그린‑민트 중심) |
| 메인 톤 | `#07140d`, `#79c9b8`, `#0e1a16`, `#092038` (deep dark green 기반) |
| 타이포그래피 | **Noto Sans KR** + 보조 **Pretendard** + **Monospace** 조합, 시원한 행간 유지 |
| 무드 키워드 | 신뢰, 전문성, 기술력, 안정감, 혁신 |

---

## 2. TailwindCSS 컬러 팔레트
```javascript
// tailwind.config.js
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50:  '#e6f5f1',
          100: '#b3e1d6',
          200: '#79c9b8',
          300: '#53b7a1',
          400: '#2aa58a',
          500: '#1a8871', // 메인 강조색
          600: '#157a65',
          700: '#0e6b59',
          800: '#092038',
          900: '#07140d'
        },
        neutral: {
          50:  '#ffffff',
          100: '#f5f5f5',
          200: '#e0e0e0',
          300: '#cccccc',
          400: '#999999',
          500: '#666666',
          600: '#4d4d4d',
          700: '#333333',
          800: '#1a1a1a',
          900: '#000000'
        },
        accent: {
          100: '#b3ffe7',
          300: '#66ffd4',
          500: '#79c9b8',
          700: '#50a696'
        },
        error:   '#e74c3c',
        success: '#2ecc71',
        warning: '#f39c12',
        info:    '#3498db'
      },
      screens: {
        mobile:  '320px',
        tablet:  '768px',
        desktop: '1024px',
        wide:    '1440px'
      },
      fontFamily: {
        sans: ['Noto Sans KR', 'Pretendard', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace']
      },
      borderRadius: {
        xl:  '1rem',
        '2xl': '1.5rem',
        '3xl': '2rem'
      }
    },
  },
  plugins: [],
}
```

---

## 3. 페이지별 디자인 가이드

### 🏠 Root Page (`/`)
**목적:** 브랜드 첫 인상, 8대 서비스 소개 및 CTA 중심  
**핵심 컴포넌트:** `HeroSection`, `ServiceCards`, `CTA Banner`, `Footer`

**레이아웃**
- 2열 그리드(Desktop 이상), 카드형 레이아웃
- Hero: 좌측 텍스트 / 우측 애니메이션 이미지
- Service Card: 곡선 UI + hover 시 glow 효과

```tsx
// Hero Section 구조
<section className="bg-gradient-to-br from-primary-900 to-primary-800">
  <div className="container mx-auto px-4 py-20 grid grid-cols-1 lg:grid-cols-2 gap-12">
    <div className="text-white">
      <h1 className="text-5xl font-bold mb-6">AI로 혁신하는<br/>비즈니스 성장 파트너</h1>
      <p className="text-xl mb-8 text-neutral-200">8가지 통합 서비스로 귀사의 성장을 가속화합니다</p>
      <button className="bg-accent-500 hover:bg-accent-600 px-8 py-4 rounded-2xl font-semibold">
        무료 진단 시작하기
      </button>
    </div>
    <div className="relative">
      <img src="https://picsum.photos/600/400" alt="AI Innovation" className="rounded-3xl shadow-2xl" />
    </div>
  </div>
</section>
```

### 🧭 서비스 상세 페이지 (`/services/[slug]`)
**목적:** 개별 서비스 심층 소개, 진단 CTA 제공  
**핵심 컴포넌트:** `IntroSection`, `KPIVisualBlock`, `ProcessTimeline`, `SurveyForm`

**레이아웃**
- 좌측 정렬형 텍스트 영역 + 오른쪽 이미지/지표
- 설문은 multi‑step 구조(progress bar 포함)

---

## 4. 레이아웃 구성 요소

### ✅ 공통 레이아웃 라우트 적용
`/, /services/, /support/, /mypage, /admin/*`

### ✅ 공통 컴포넌트

| 컴포넌트 | 설명 |
| --- | --- |
| `<Header>` | Topbar + 드롭다운 메뉴 (로그인 상태에 따라 마이페이지/로그인 표시) |
| `<Footer>` | 로고 + 연락처 + 이용약관 |
| `<Container>` | `max-w-[1280px]` + `padding‑x` 적용 |

### ✅ 반응형 레이아웃

| 디바이스 | 레이아웃 기준 |
| --- | --- |
| Mobile | 1열, full width |
| Tablet | 1‑2열 hybrid |
| Desktop 이상 | 2~3열, 카드 UI, Hover 상호작용 |

---

## 5. 인터랙션 패턴

| 유형 | 설명 |
| --- | --- |
| Hover | 카드 glow, 버튼 scaleUp, 링크 underline transition |
| Click | 설문 응답 시 상태 변경 + AutoScroll |
| Scroll | Service 카드 등장 시 fade‑in (framer‑motion) |
| Modal | 상담 완료 시 확인 모달 띄우기 |
| Drawer | 관리자 상세정보 슬라이드 패널 |

---

## 6. 반응형 브레이크포인트 정의
```scss
$breakpoints: (
  'mobile': 320px,
  'tablet': 768px,
  'desktop': 1024px,
  'wide': 1440px
);
```

| 구간 | 설명 |
| --- | --- |
| 모바일 | 전체 요소 수직 정렬, 큰 버튼, 스크롤 대응 강화 |
| 태블릿 | 양측 여백 확보, 버튼 사이즈 유지, 카드 2열 |
| 데스크탑 | 레이아웃 확장, 애니메이션 추가, 카드 3열 |
| 와이드 | hero 비주얼 확대, `max-w-[1440px]` 기준으로 중앙 정렬 |

---

## 📁 프로젝트 구조
```text
ai-camp-website/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── register/
│   │       └── page.tsx
│   ├── (main)/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── about/
│   │   │   └── page.tsx
│   │   ├── services/
│   │   │   └── [slug]/
│   │   │       └── page.tsx
│   │   ├── cases/
│   │   │   └── page.tsx
│   │   ├── support/
│   │   │   ├── consult/
│   │   │   │   └── page.tsx
│   │   │   └── faq/
│   │   │       └── page.tsx
│   │   └── mypage/
│   │       └── page.tsx
│   ├── admin/
│   │   ├── layout.tsx
│   │   └── dashboard/
│   │       └── page.tsx
│   └── api/
│       ├── auth/
│       │   └── [...nextauth]/
│       │       └── route.ts
│       ├── send-report/
│       │   └── route.ts
│       └── consultations/
│           └── route.ts
├── components/
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   └── Container.tsx
│   ├── features/
│   │   ├── survey/
│   │   │   ├── SurveyContainer.tsx
│   │   │   ├── SurveyProgress.tsx
│   │   │   ├── SurveyQuestion.tsx
│   │   │   └── SurveyUserInfoForm.tsx
│   │   └── admin/
│   │       ├── CustomerTable.tsx
│   │       ├── CustomerDetailDrawer.tsx
│   │       └── ConsultationForm.tsx
│   └── ui/
│       └── (shadcn components)
├── lib/
│   ├── supabase.ts
│   ├── auth.ts
│   └── pdf-generator.ts
├── types/
│   └── index.ts
├── public/
├── .env.local
├── middleware.ts
├── tailwind.config.js
└── package.json
```

---

## 🚀 구현 가이드

### 환경 변수 설정 (`.env.local`)
```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# NextAuth
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your_nextauth_secret

# OAuth Providers
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
KAKAO_CLIENT_ID=your_kakao_client_id
KAKAO_CLIENT_SECRET=your_kakao_client_secret

# Email Service
RESEND_API_KEY=your_resend_api_key

# Admin
ADMIN_EMAIL=admin@aicamp.kr
```
