# 📘 3. 유스케이스 문서

## 📌 목차
- 액터 정의 (Actor Definitions)
- 유스케이스 시나리오 (Use Case Scenarios)
- 주요 흐름 (Main Steps and Flow of Events)
- 예외 처리 및 대안 흐름 (Alternative Flows and Edge Cases)
- 사전 조건 및 사후 조건 (Preconditions and Postconditions)
- 비즈니스 규칙 및 제약 조건 (Business Rules and Constraints)
- 예외 처리 절차 (Exception Handling Procedures)
- 사용자 인터페이스 고려사항 (User Interface Considerations)
- 데이터 요구사항 및 흐름 (Data Requirements and Data Flow)
- 보안 및 개인정보 보호 고려사항 (Security and Privacy Considerations)

---

## 1. 액터 정의 (Actor Definitions)

| 액터 | 역할 |
| --- | --- |
| 웹사이트 방문자 | 비회원 사용자로, 진단 설문과 상담 신청이 가능 |
| 일반 회원 | 로그인 후 마이페이지 접근 가능, 진단 결과 및 상담 내역 조회 |
| 관리자 | CRM 대시보드 접근, 고객 관리 및 상담 기록 관리 가능 |

---

## 2. 유스케이스 시나리오 (Use Case Scenarios)

### ✅ 유스케이스 1: AS-IS 설문 진단 및 자동 리포트 발송
- **목적:** 8개 서비스 항목에 대한 설문을 통해 사용자의 현재 상황을 진단하고, 결과 보고서를 자동으로 이메일로 발송  

### ✅ 유스케이스 2: 관리자 CRM 고객 정보 확인 및 상담내역 관리
- **목적:** 관리자 계정으로 로그인하여, 설문 및 상담 신청 정보를 조회 및 상담이력 관리  

---

## 3. 주요 흐름 (Main Steps and Flow of Events)

### 🔹 유스케이스 1: 설문 진단 흐름
1. 사용자가 서비스 상세 페이지에 진입  
2. **[무료 진단 시작하기]** 버튼 클릭  
3. Multi-step 설문 UI 노출  
4. 각 질문에 응답 → 상태 관리로 실시간 저장  
5. 마지막 단계에서 이름, 이메일, 전화번호 입력  
6. **[진단 리포트 받기]** 클릭  
7. DB 저장 + Vercel 서버리스 함수 통해 리포트 자동 발송  
8. **[감사 메시지]** UI 표시  

---

## 4. 예외 처리 및 대안 흐름 (Alternative Flows and Edge Cases)

| 단계 | 예외/대안 흐름 | 처리 방법 |
| --- | --- | --- |
| 5번 | 연락처 누락 | 필수 입력 필드로 설정, Toast 메시지 출력 |
| 6번 | 서버 오류 발생 | 사용자에게 오류 안내 메시지 표시 및 재시도 옵션 제공 |
| 7번 | 이메일 발송 실패 | 관리자에게 Slack 알림, 재발송 시도 로직 포함 |

---

## 5. 사전 조건 및 사후 조건 (Preconditions and Postconditions)

| 항목 | 설명 |
| --- | --- |
| 사전 조건 | 사용자가 설문 UI에 접근 가능해야 함 (서비스 상세 페이지 접속) |
| 사후 조건 | 설문 응답 및 개인정보는 Supabase DB에 저장되며, 결과 리포트는 이메일로 전송됨 |

---

## 6. 비즈니스 규칙 및 제약 조건 (Business Rules and Constraints)

- 한 사용자는 동일 이메일로 **1일 1회**까지만 설문 가능 (중복 제출 방지)  
- 필수 입력 항목 미입력 시 제출 불가  
- 모든 설문 응답은 **Supabase 테이블**에 저장되어야 함  

---

## 7. 예외 처리 절차 (Exception Handling Procedures)

| 상황 | 처리 방식 |
| --- | --- |
| DB 연결 실패 | 에러 메시지 로그 기록 및 재시도 버튼 제공 |
| 설문 데이터 누락 | 클라이언트 상태 검증 → 에러 메시지 표시 |
| 인증 토큰 만료 (관리자) | 자동 로그아웃 처리 및 로그인 페이지로 리다이렉트 |

---

## 8. 사용자 인터페이스 고려사항 (User Interface Considerations)

- 설문은 **progress bar** 포함된 다단계 UI로 구성  
- 오류 발생 시 **Toast 메시지** 또는 **Modal** 알림  
- 모바일에서도 손쉽게 응답 가능하도록 **반응형 UI** 설계  
- 설문 완료 후, `"보고서가 이메일로 발송되었습니다"` 메시지 표시  

---

## 9. 데이터 요구사항 및 흐름 (Data Requirements and Data Flow)

### 📥 입력 데이터
- 이름, 이메일, 연락처  
- 각 서비스 영역별 설문 응답 값 (선택형/서술형 혼합)  

### 📤 출력 데이터
- 이메일로 발송된 진단 리포트 (PDF 또는 HTML 이메일)  

### 🔄 데이터 흐름
`[Client] 설문 응답 → [Supabase] DB 저장 → [Vercel Function] 결과 생성 → [SMTP] 이메일 전송`

---

## 10. 보안 및 개인정보 보호 고려사항 (Security and Privacy Considerations)

- 모든 개인정보는 **Supabase의 RLS 정책**을 통해 접근 제한  
- 이메일, 전화번호는 **암호화 저장** 필요  
- 관리자 기능은 **NextAuth**를 통해 인증된 사용자만 접근 가능  
- 이메일 발송 시 외부 노출 방지를 위해 **백엔드 처리**에서만 메일링 실행  
