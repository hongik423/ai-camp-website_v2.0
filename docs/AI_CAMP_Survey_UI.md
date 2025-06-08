# 🧩 4. 설문 진단 UI 컴포넌트 설계

## 📌 1. 전체 구조 (컴포넌트 계층)
```tsx
<SurveyContainer>
  ├─ <SurveyProgress /> // 진행 상태 표시
  ├─ <SurveyStep> // 단계별 설문 컨테이너
  │  └─ <SurveyQuestion /> // 개별 질문 컴포넌트
  ├─ <SurveyUserInfoForm /> // 사용자 정보 입력
  ├─ <SurveySubmitButton /> // 제출 버튼
  └─ <SurveyToast /> // 알림 메시지
</SurveyContainer>
```

---

## 📋 2. 설문 질문 구조 (8개 서비스별)
```typescript
interface SurveyConfig {
  [serviceType: string]: {
    title: string;
    steps: {
      id: string;
      question: string;
      type: 'radio' | 'checkbox' | 'scale' | 'text';
      options?: { value: string; label: string; }[];
      required: boolean;
    }[];
  };
}

const surveyConfig: SurveyConfig = {
  productivity: {
    title: "AI 생산성 혁신 진단",
    steps: [
      {
        id: "ai_usage",
        question: "현재 조직에서 AI 도구를 활용하고 있습니까?",
        type: "radio",
        required: true,
        options: [
          { value: "yes", label: "예, 활용 중입니다" },
          { value: "no", label: "아니오, 활용하지 않습니다" },
          { value: "planning", label: "도입을 검토 중입니다" }
        ]
      },
      {
        id: "pain_points",
        question: "현재 가장 큰 생산성 저해 요인은 무엇입니까?",
        type: "checkbox",
        required: true,
        options: [
          { value: "repetitive", label: "반복적인 업무" },
          { value: "communication", label: "비효율적인 소통" },
          { value: "tools", label: "도구 활용 미숙" },
          { value: "process", label: "복잡한 프로세스" }
        ]
      }
    ]
  },
  funding: {
    title: "정책자금 활용 진단",
    steps: [
      {
        id: "funding_experience",
        question: "최근 1년 내 정책자금을 신청한 경험이 있으십니까?",
        type: "radio",
        required: true,
        options: [
          { value: "yes_success", label: "예, 선정되었습니다" },
          { value: "yes_fail", label: "예, 탈락했습니다" },
          { value: "no", label: "아니오, 신청한 적 없습니다" }
        ]
      }
    ]
  }
  // ... 나머지 6개 서비스
};
```

---

## 🧩 3. 주요 컴포넌트 상세 설계

### 📦 `<SurveyProgress />`
```tsx
interface SurveyProgressProps {
  currentStep: number;
  totalSteps: number;
}

// 시각적 진행률 표시
// framer-motion으로 부드러운 전환 애니메이션
```

### 📦 `<SurveyQuestion />`
```tsx
interface SurveyQuestionProps {
  question: string;
  type: 'radio' | 'checkbox' | 'scale' | 'text';
  options?: Option[];
  value: any;
  onChange: (value: any) => void;
  error?: string;
}

// shadcn/ui의 RadioGroup, Checkbox, Slider, Input 활용
// 에러 상태 표시 및 validation
```

### 📦 `<SurveyUserInfoForm />`
```tsx
interface UserInfo {
  name: string;
  email: string;
  phone: string;
  company?: string;
}

// react-hook-form + zod validation
// 필수 필드 검증 및 이메일 형식 확인
```

---

## 🎨 4. 디자인 및 사용자 경험 고려사항
- 모든 컴포넌트는 **shadcn/ui**의 Card, Input, Button 기반으로 구성  
- 모바일 환경에서 터치 대응 UI, 요소 간 여백 확보  
- 다크모드 대응 및 색상 대비 고려  
- 질문 수가 많을 경우 스크롤 이동 시 고정된 제출 버튼  

---

## 🔒 5. 기술적 구현 사항
```typescript
// 설문 상태 관리
const [surveyData, setSurveyData] = useState<SurveyData>({});
const [currentStep, setCurrentStep] = useState(0);

// 제출 처리
const handleSubmit = async (userInfo: UserInfo) => {
  try {
    // Supabase에 데이터 저장
    const { data, error } = await supabase
      .from('survey_responses')
      .insert({
        user_id: userInfo.email,
        service_type: serviceType,
        responses: surveyData,
        completed_at: new Date()
      });

    // 이메일 발송 API 호출
    await fetch('/api/send-report', {
      method: 'POST',
      body: JSON.stringify({
        email: userInfo.email,
        name: userInfo.name,
        surveyData
      })
    });

    // 성공 메시지 표시
    toast.success('진단이 완료되었습니다. 이메일을 확인해주세요.');
  } catch (error) {
    toast.error('오류가 발생했습니다. 다시 시도해주세요.');
  }
};
```
