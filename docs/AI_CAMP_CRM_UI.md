# 🧩 5. 관리자 CRM UI 컴포넌트 설계

## 📌 1. 전체 구조 (컴포넌트 계층)
```tsx
<AdminDashboardLayout>
  ├─ <AdminHeader />
  ├─ <CustomerManagement>
  │  ├─ <CustomerSearchFilter />
  │  ├─ <CustomerTable />
  │  └─ <CustomerDetailDrawer>
  │     ├─ <CustomerInfoSection />
  │     ├─ <ConsultationHistory />
  │     └─ <ConsultationForm />
  │  </CustomerDetailDrawer>
  └─ <AdminFooter />
</AdminDashboardLayout>
```

---

## 📋 2. 데이터베이스 구조
```sql
-- 사용자 테이블
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  phone TEXT,
  company_name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 설문 응답 테이블
CREATE TABLE survey_responses (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  service_type TEXT NOT NULL,
  responses JSONB NOT NULL,
  completed_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 상담 신청 테이블
CREATE TABLE consultations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  service_types TEXT[] NOT NULL,
  message TEXT,
  status TEXT DEFAULT 'pending',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 상담 이력 테이블
CREATE TABLE consultation_notes (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  consultation_id UUID REFERENCES consultations(id),
  admin_id UUID REFERENCES users(id),
  note TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- RLS 정책 설정
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE survey_responses ENABLE ROW LEVEL SECURITY;
ALTER TABLE consultations ENABLE ROW LEVEL SECURITY;
ALTER TABLE consultation_notes ENABLE ROW LEVEL SECURITY;
```

---

## 🧩 3. 주요 컴포넌트 상세 설계

### 📦 `<CustomerTable />`
```tsx
interface CustomerTableProps {
  customers: Customer[];
  onCustomerSelect: (customer: Customer) => void;
}

// 기능:
// - 정렬 가능한 테이블 헤더
// - 페이지네이션
// - 상태별 필터링 (대기중/진행중/완료)
// - 검색 기능
```

### 📦 `<CustomerDetailDrawer />`
```tsx
interface CustomerDetailDrawerProps {
  customer: Customer | null;
  isOpen: boolean;
  onClose: () => void;
}

// 슬라이드 패널 형태로 고객 상세 정보 표시
// 설문 응답 요약, 상담 이력, 메모 작성 기능
```

### 📦 `<ConsultationForm />`
```tsx
interface ConsultationFormProps {
  customerId: string;
  onSubmit: (data: ConsultationNote) => void;
}

// 상담 내용 입력 폼
// 날짜, 상담 유형, 내용, 다음 조치사항
```

---

## 🔎 4. 인터페이스 레이아웃
```
┌─────────────────────────────────────────────────┐
│ [검색] [서비스 필터 ▼] [상태 필터 ▼] [기간 ▼]  │
├─────────────────────────────────────────────────┤
│ 고객명 │ 이메일 │ 연락처 │ 신청일 │ 상태 │ 액션 │
│ 홍길동 │ hong@.. │ 010-123 │ 6/3 │ 완료 │ [보기] │
│ 김철수 │ chul@.. │ 010-456 │ 6/1 │ 대기 │ [보기] │
└─────────────────────────────────────────────────┘
[◀ 1 2 3 4 5 ▶]
```

---

## 🔒 5. 보안 및 권한 관리
```typescript
// middleware.ts
import { withAuth } from "next-auth/middleware";

export default withAuth({
  callbacks: {
    authorized: ({ token }) => token?.isAdmin === true,
  },
});

export const config = {
  matcher: ["/admin/:path*"],
};
```
