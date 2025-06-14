AI CAMP × 경영지도센터 통합 플랫폼 완전 문서
# 📝 1. 제품 요구사항 정의서 (PRD)
## 📌 목차

- 제품 개요
- 참고 서비스 및 비교 분석
- 핵심 기능 명세
- 추가 제안 기능
- 사용자 페르소나 및 시나리오
- 기술 스택 제안

## 1. 제품 개요
**목표:**  
AI CAMP의 AI 기반 컨설팅과 경영지도센터의 기술사업화 역량을 통합하여, 기업의 성장 단계에 맞는 8가지 비즈니스 전략 서비스를 온라인에서 진단·상담·관리할 수 있는 플랫폼을 구축한다.  
**적용 프레임워크:** Business Model Zen 12단계  

가치 발견 → 창출 → 제공 → 포착 → 교정  
기업 성장 단계별 전략(1-3년, 3-7년, 7-10년, 10년 이상)에 맞춤 대응  

**제공 서비스:**  

- AI 생산성 혁신
- 정책자금 자문
- 경매 활용 공장구매
- 매출증대 웹사이트 구축
- 경정청구
- ISO·ESG 인증
- 일터혁신 상생컨설팅
- 기술사업화 지원

**데이터 저장 방식:**  
xml<storage-type>  
cloud-database (Supabase PostgreSQL)  
</storage-type>  

## 2. 참고 서비스 및 비교 분석
**참고 URL:**  
https://hongik423.github.io/ai-camp-website/  

**차별성:**  
- 통합적 진단 시스템: 8개 분야에 대한 종합 설문 및 로드맵 제공  
- CRM 중심 서비스 운영: Supabase DB 기반 고객 이력 관리  
- 자동화된 AS-IS 진단과 보고서 발송  
- 6개월 사후관리 및 재계약 유도 전략 내재  

## 3. 핵심 기능 명세
| 기능 | 설명 |
| --- | --- |
| 🔐 로그인/회원가입 | 이메일, 카카오 로그인 연동 / NextAuth.js |
| 👤 회원/고객 관리 | Supabase 기반 고객 DB + CRM 기능 |
| 📝 AS-IS 설문 진단 | 8개 서비스 항목별 설문, 이메일 자동 보고서 발송 |
| 📬 무료상담 신청 | 양식 기반 신청 → DB 저장 및 자동 확인메일 발송 |
| 📞 다채널 정보 수집 | 전화, 이메일, 설문, 상담폼 통해 고객정보 확보 가능 |
| 🗃️ 관리자 CRM 대시보드 | 고객별 상담이력, 설문 결과, 메모 관리 기능 포함 |

## 4. 추가 제안 기능
- 📊 **분석 대시보드:** 상담 및 진단 통계를 시각화  
- 💬 **챗봇 기반 상담도우미:** 초기 문의 자동 응대  
- 📱 **모바일 앱 대응형 웹:** 반응형 디자인 적용  
- 🧾 **보고서 자동 PDF 생성 및 다운로드 기능**  
- 🎯 **고객 등급화 및 알림 발송 시스템**  

## 5. 사용자 페르소나 및 시나리오
### 👤 사용자 페르소나
| 구분 | 설명 |
| --- | --- |
| CEO/대표 | 매출 증대, 비용 절감 관심 / ROI 중심 결정 |
| 임원 | 실행 전략 및 성과 지표 기반 접근 |
| 실무자 | 신청 및 실행 편의성, 자동화 중요 |
| 업종별 | 제조업, 기술기업, 서비스업 등 맞춤화 필요 |

### 📌 시나리오 예시
#### 📍 설문 진단 시나리오
1. 사용자가 '생산성혁신' 상세페이지 진입  
2. '무료 진단' 버튼 클릭 → 설문 시작  
3. Multi-step 폼으로 항목 응답  
4. 마지막에 이름/연락처/이메일 입력  
5. 자동으로 리포트 이메일 전송 + DB 저장  

#### 📍 상담 신청 시나리오
1. '무료 상담 신청' 메뉴 접근  
2. 양식 입력 후 제출  
3. DB 저장 및 접수 확인 메일 자동 발송  
4. 관리자는 CRM에서 고객 정보 확인  

## 6. 기술 스택 제안
| 항목 | 기술 스택 |
| --- | --- |
| 프레임워크 | Next.js (App Router) |
| 데이터베이스 | Supabase (PostgreSQL) |
| 인증 시스템 | NextAuth.js |
| UI 컴포넌트 | shadcn/ui |
| 스타일링 | Tailwind CSS |
| 폼 관리 | React Hook Form |
| 애니메이션 | Framer Motion |
| 아이콘 | Lucide-react |
| 배포 | Vercel |
| 개발 환경 | Node.js |
