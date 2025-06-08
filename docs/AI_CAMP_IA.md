# 🗂️ 2. 정보구조 설계 문서 (IA)

## 📌 목차
- 사이트맵 (Site Map)
- 사용자 흐름 (User Flow)
- 내비게이션 구조 (Navigation Structure)
- 페이지 계층 구조 (Page Hierarchy)
- 콘텐츠 구성 (Content Organization)
- 인터랙션 패턴 (Interaction Patterns)
- URL 구조 (URL Structure)
- 컴포넌트 계층 구조 (Component Hierarchy)

---

## 1. 사이트맵 (Site Map)
```
홈
├─ AI CAMP 소개
├─ 8대 서비스
│  ├─ 생산성혁신 컨설팅
│  ├─ 정책자금 자문
│  ├─ 경매 공장구매
│  ├─ 웹사이트 구축
│  ├─ 경정청구
│  ├─ ISO·ESG 인증
│  ├─ 일터혁신 컨설팅
│  └─ 기술사업화
├─ 성공사례
├─ 고객 지원
│  ├─ 무료상담 신청
│  └─ FAQ
├─ 로그인 / 회원가입
├─ 마이페이지
│  ├─ 내 정보
│  ├─ 진단 결과
│  └─ 상담 내역
└─ 관리자 대시보드
   ├─ 고객 목록
   └─ 고객 상세/관리
```

---

## 2. 사용자 흐름 (User Flow)

### 🔹 AS-IS 진단 흐름
1. 서비스 상세페이지 진입  
2. **[무료 진단 시작하기]** 클릭  
3. 설문 응답 → 이름, 이메일, 연락처 입력  
4. 제출 → 자동 이메일 발송, DB 저장  
5. **[진단 완료 메시지]** 확인  

### 🔹 상담 신청 흐름
1. **[무료 상담 신청]** 접근  
2. 신청 폼 입력 → 제출  
3. DB 저장 및 확인 메일 자동 발송  
4. 관리자 CRM에서 확인 가능  

### 🔹 회원가입 및 로그인
1. 이메일 또는 카카오 로그인 선택  
2. NextAuth를 통한 인증 처리  
3. 로그인 후 마이페이지 접근 가능  

---

## 3. 내비게이션 구조 (Navigation Structure)
**형식:** Topbar  

| 항목 | 설명 |
| --- | --- |
| 로고 | 홈으로 이동 |
| 서비스 소개 | 드롭다운: 8개 서비스 |
| 성공사례 |  |
| 고객지원 | 드롭다운: 상담신청, FAQ |
| 로그인 / 마이페이지 |  |
| 관리자 대시보드 | 로그인 상태에서만 표시 |

---

## 4. 페이지 계층 구조 (Page Hierarchy)

| 레벨 1 | 레벨 2 | 레벨 3 |
| --- | --- | --- |
| 홈 | 서비스 | 개별 서비스 상세 |
| AI CAMP 소개 | - | - |
| 성공사례 | - | 사례 상세 |
| 고객지원 | 상담신청, FAQ | - |
| 마이페이지 | 진단 결과, 상담 내역 | 상세 정보 |
| 관리자 | 고객 리스트 | 고객 상세 |

---

## 5. 콘텐츠 구성 (Content Organization)

- **홈:** Hero, 8개 서비스 소개 카드, 진단 CTA, 성공사례 하이라이트  
- **서비스 상세:** 서비스 개요 → 기대효과 → 프로세스 → 설문 → 상담 CTA  
- **마이페이지:** 진단 리포트 다운로드, 상담 내역 열람  
- **관리자:** Supabase DB 연동 고객 목록, 메모, 상담상태 관리  

---

## 6. 인터랙션 패턴 (Interaction Patterns)

| 기능 | 방식 |
| --- | --- |
| 설문 진단 | Multi-step Form, 상태관리 필요 |
| 상담신청 | Form 제출 후 토스트 및 메일 응답 |
| 로그인 | 이메일/카카오 OAuth 연동 |
| 리포트 발송 | 제출 후 자동 이메일 전송 |
| CRM | 테이블 필터, 상세 모달, 메모 저장 기능 |

---

## 7. URL 구조 (URL Structure)

| 페이지 | URL |
| --- | --- |
| 홈 | / |
| AI CAMP 소개 | /about |
| 생산성혁신 컨설팅 | /services/productivity |
| 정책자금 자문 | /services/funding |
| 경매 공장구매 | /services/auction |
| 웹사이트 구축 | /services/website |
| 경정청구 | /services/tax-refund |
| ISO·ESG 인증 | /services/certification |
| 일터혁신 컨설팅 | /services/workplace |
| 기술사업화 | /services/tech-business |
| 성공사례 | /cases |
| 무료상담 신청 | /support/consult |
| FAQ | /support/faq |
| 마이페이지 | /mypage |
| 관리자 대시보드 | /admin/dashboard |

---

## 8. 컴포넌트 계층 구조 (Component Hierarchy)
```
App
├─ Header (Topbar)
│  ├─ Logo
│  ├─ NavLinks
│  └─ AuthButtons
├─ Footer
├─ HomePage
│  ├─ HeroSection
│  ├─ ServicesCardSection
│  ├─ CaseHighlight
│  └─ CallToAction
├─ ServiceDetailPage
│  ├─ Intro
│  ├─ ProcessSteps
│  ├─ SurveyForm
│  └─ CTAForm
├─ ConsultFormPage
├─ MyPage
│  ├─ ProfileInfo
│  ├─ SurveyResults
│  └─ ConsultHistory
├─ AdminDashboard
│  ├─ CustomerListTable
│  └─ CustomerDetailModal
```
