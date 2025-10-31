# Windsurf - 테토-에겐 성격 유형 테스트 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: 테토-에겐 성격 유형 테스트 (Teto-Egen Personality Test)  
**버전**: 0.1.0  
**설명**: 테스토스테론과 에스트로겐 성향을 바탕으로 한 성격 유형 테스트 웹 애플리케이션

---

## 🎯 프로젝트 목적

사용자의 성격을 4가지 유형(테토남, 에겐남, 테토녀, 에겐녀)으로 분류하는 심리 테스트 웹사이트입니다. 테스토스테론과 에스트로겐이라는 호르몬 특성을 기반으로 성격 유형을 구분하며, 연애 먹이사슬 개념을 포함한 재미있는 결과를 제공합니다.

---

## 🛠 기술 스택

### 프론트엔드
- **React**: 19.1.0 (최신 버전)
- **TypeScript**: 4.9.5
- **Styled Components**: 6.1.19 (CSS-in-JS)
- **React Scripts**: 5.0.1 (Create React App 기반)

### 개발 도구
- **Testing Library**: React, Jest-DOM, User-Event
- **Web Vitals**: 성능 측정

### 빌드 도구
- Create React App (CRA) 기반 설정
- TypeScript 컴파일러 설정 (ES5 타겟)

---

## 📁 프로젝트 구조

```
teto-egen_test/
├── .gitignore
├── .vercel/                    # Vercel 배포 설정
└── teto-egen-test/
    ├── README.md
    ├── package.json
    ├── package-lock.json
    ├── tsconfig.json
    ├── build/                  # 빌드 결과물
    ├── node_modules/
    ├── public/
    │   ├── ads.txt            # Google AdSense 설정
    │   ├── favicon.ico
    │   ├── index.html
    │   ├── logo192.png
    │   ├── logo512.png
    │   ├── manifest.json
    │   └── robots.txt
    └── src/
        ├── App.tsx            # 메인 애플리케이션 (789줄)
        ├── App.css
        ├── App.test.tsx
        ├── index.tsx
        ├── index.css
        ├── logo.svg
        ├── react-app-env.d.ts
        ├── reportWebVitals.ts
        ├── setupTests.ts
        └── components/
            └── AdSense.tsx    # Google AdSense 컴포넌트
```

---

## 🎨 주요 기능

### 1. 4단계 테스트 플로우

#### **Step 1: 인트로 화면 (intro)**
- 테스트 소개 및 설명
- 연애 먹이사슬 구조 시각화 (원형 다이어그램)
- 4가지 성격 유형 소개

#### **Step 2: 성별 선택 (gender)**
- 남성/여성 선택
- 성별에 따라 결과 유형이 달라짐

#### **Step 3: 테스트 진행 (test)**
- 총 10개의 질문
- 각 질문당 4개의 선택지 (랜덤 순서로 표시)
- 진행률 표시 (Progress Bar)
- 점수 기반 성격 유형 판별

#### **Step 4: 결과 화면 (result)**
- 성격 유형 결과 표시
- 유형별 특성 리스트
- 연애 먹이사슬에서의 위치
- 재테스트 기능

---

## 🧬 성격 유형 시스템

### 4가지 성격 유형

#### 1. **테토남 (Teto Male)** 💪
- **색상**: #e74c3c (빨강)
- **특징**: 테스토스테론 남성
- **성격**: 공격성과 사냥 본능이 강하고, 주도적이며 현실 지향적
- **주요 특성**:
  - 공격성과 사냥 본능이 강함
  - 자기주장이 강하며 리더십 있음
  - 감정보다 논리를 우선시
  - 친구가 많고 무리 생활에 익숙
  - 외부 세계(정치, 사회적 지위)에 관심 많음
  - 도전과 모험을 좋아함

#### 2. **에겐남 (Egen Male)** 🎨
- **색상**: #9b59b6 (보라)
- **특징**: 에스트로겐 남성
- **성격**: 감수성과 섬세함이 뛰어나며, 예민하고 부드러운
- **주요 특성**:
  - 감수성과 섬세함이 뛰어남
  - 자기 감정과 타인 감정에 민감
  - 추상적 개념(예술, 철학)에 흥미
  - 내면지향적이며 개인적 세계 중시
  - 트렌드에 민감하고 미적 감각 뛰어남

#### 3. **테토녀 (Teto Female)** 🔥
- **색상**: #f39c12 (주황)
- **특징**: 테스토스테론 여성
- **성격**: 활발하고 적극적이며, 독립적이고 도전적
- **주요 특성**:
  - 활발하고 적극적인 성격
  - 독립심이 강함
  - 단순한 사고 구조, 빠른 판단력
  - 성취나 커리어 중심적 사고
  - 운동이나 활동적 취미 선호

#### 4. **에겐녀 (Egen Female)** 🌸
- **색상**: #e91e63 (핑크)
- **특징**: 에스트로겐 여성
- **성격**: 부드럽고 감성적이며, 섬세하고 정적인 분위기
- **주요 특성**:
  - 주장이나 기가 강하지 않음
  - 복잡한 내면 구조를 가짐
  - 타인의 감정에 민감하고 공감 능력 높음
  - 부드럽고 정적인 분위기
  - 예술, 문학, 디자인 등에 깊은 몰입

---

## 💕 연애 먹이사슬 시스템

### 순환 구조
```
에겐녀 → 에겐남 → 테토녀 → 테토남 → 에겐녀 (순환)
  🌸  →   🎨  →   🔥  →   💪  →   🌸
```

### 각 유형별 선호 경향
- **테토남**: 에겐녀에게 끌림 (부드러운 여성성과 섬세한 감수성)
- **에겐남**: 테토녀에게 끌림 (추진력과 에너지)
- **테토녀**: 테토남에게 끌림 (더 강한 양기와 남성적 매력)
- **에겐녀**: 에겐남에게 끌림 (감수성과 정서적 이해)

---

## 📝 테스트 질문 구조

### 질문 카테고리 (총 10개)

1. **사교성**: 친구들과 있을 때의 모습
2. **스트레스 해소**: 스트레스 대처 방식
3. **연애 스타일**: 연애할 때의 행동 패턴
4. **패션/외모**: 외모 관리에 대한 태도
5. **갈등 대처**: 갈등 상황 해결 방식
6. **여가 활동**: 여가 시간 활용 방법
7. **의사결정**: 결정을 내리는 방식
8. **이상형**: 선호하는 이성 유형
9. **SNS 사용**: 소셜 미디어 활용 패턴
10. **적응력**: 새로운 환경 적응 방식

### 점수 시스템
- 각 선택지는 `teto`와 `egen` 점수를 가짐
- 점수 범위: 0~3점
- 최종 점수 비교로 성격 유형 결정
- 성별과 점수를 조합하여 4가지 유형 중 하나 도출

---

## 🎨 UI/UX 특징

### 디자인 시스템

#### 색상 팔레트
- **배경 그라데이션**: #667eea → #764ba2 (보라 계열)
- **테토남**: #e74c3c (빨강)
- **에겐남**: #9b59b6 (보라)
- **테토녀**: #f39c12 (주황)
- **에겐녀**: #e91e63 (핑크)

#### 컴포넌트 스타일
- **Container**: 전체 화면 그라데이션 배경
- **TestContainer**: 흰색 카드 형태 (border-radius: 20px)
- **OptionButton**: 호버 효과와 부드러운 트랜지션
- **ProgressBar**: 진행률 표시 (그라데이션 바)
- **ResultContainer**: 유형별 색상 배경

### 반응형 디자인
- 모바일 최적화 (@media max-width: 768px)
- 폰트 크기 자동 조정
- 패딩 및 여백 최적화
- 원형 먹이사슬 다이어그램 크기 조정

### 애니메이션
- 버튼 호버 효과 (transform, box-shadow)
- 진행률 바 트랜지션 (0.3s ease)
- 부드러운 페이지 전환

---

## 🔧 주요 코드 구조

### App.tsx 핵심 로직

#### 1. **상태 관리**
```typescript
const [currentStep, setCurrentStep] = useState<'intro' | 'gender' | 'test' | 'result'>('intro');
const [currentQuestion, setCurrentQuestion] = useState(0);
const [scores, setScores] = useState({ teto: 0, egen: 0 });
const [selectedGender, setSelectedGender] = useState<'male' | 'female' | null>(null);
const [result, setResult] = useState<PersonalityType | null>(null);
const [shuffledOptions, setShuffledOptions] = useState<any[]>([]);
```

#### 2. **선택지 랜덤화**
```typescript
const shuffleArray = (array: any[]) => {
  const shuffled = [...array];
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
  }
  return shuffled;
};
```

#### 3. **결과 계산 로직**
```typescript
const handleAnswer = (optionScores: { [key: string]: number }) => {
  // 점수 누적
  const newScores = { ...scores };
  Object.keys(optionScores).forEach(key => {
    newScores[key as keyof typeof scores] += optionScores[key];
  });
  
  // 마지막 질문이면 결과 계산
  if (currentQuestion === questions.length - 1) {
    const isTeto = newScores.teto > newScores.egen;
    const resultId = selectedGender === 'male' 
      ? (isTeto ? 'teto_male' : 'egen_male')
      : (isTeto ? 'teto_female' : 'egen_female');
    // 결과 설정
  }
};
```

#### 4. **원형 먹이사슬 시각화**
- CSS transform을 이용한 원형 배치
- 삼각함수(cos, sin)로 위치 계산
- 활성 유형 강조 표시

---

## 📱 AdSense 통합

### AdSense 컴포넌트
- Google AdSense 광고 통합
- 결과 화면에 광고 표시
- 반응형 광고 설정

### 설정 파일
- `public/ads.txt`: AdSense 인증 파일
- `public/index.html`: AdSense 스크립트 로드

---

## 🚀 실행 방법

### 개발 환경 실행
```bash
npm install
npm start
```
- 브라우저에서 http://localhost:3000 접속

### 프로덕션 빌드
```bash
npm run build
```
- `build/` 폴더에 최적화된 파일 생성

### 테스트 실행
```bash
npm test
```

---

## 📊 TypeScript 설정

### tsconfig.json 주요 설정
- **target**: ES5 (구형 브라우저 호환)
- **lib**: DOM, DOM.Iterable, ESNext
- **jsx**: react-jsx (새로운 JSX 변환)
- **strict**: true (엄격한 타입 체크)
- **module**: ESNext
- **moduleResolution**: Node

---

## 🎯 프로젝트 특징

### 장점
1. **TypeScript 사용**: 타입 안정성 확보
2. **Styled Components**: CSS-in-JS로 스타일 관리 용이
3. **반응형 디자인**: 모바일/데스크톱 모두 지원
4. **사용자 경험**: 직관적인 UI와 부드러운 애니메이션
5. **재미 요소**: 연애 먹이사슬 개념으로 흥미 유발
6. **랜덤화**: 선택지 순서 랜덤으로 편향 방지

### 개선 가능한 부분
1. **코드 분리**: App.tsx가 789줄로 너무 큼 → 컴포넌트 분리 필요
2. **상태 관리**: Context API나 Redux 도입 고려
3. **테스트 코드**: 단위 테스트 추가 필요
4. **접근성**: ARIA 속성 추가
5. **SEO**: 메타 태그 최적화
6. **다국어**: i18n 지원 고려

---

## 🔒 주의사항

### 면책 조항
- 재미를 위한 테스트로 과학적 근거 없음
- 실제 성격은 훨씬 복합적이고 다면적
- 결과 화면에 면책 조항 표시

### AdSense 설정
- `ca-pub-YOUR-PUBLISHER-ID` → 실제 Publisher ID로 교체 필요
- `YOUR-AD-SLOT-ID` → 실제 Ad Slot ID로 교체 필요

---

## 📈 배포 정보

### Vercel 배포
- `.vercel/` 폴더 존재
- Vercel 플랫폼에 배포 가능
- 자동 빌드 및 배포 설정

---

## 🎓 학습 포인트

이 프로젝트를 통해 배울 수 있는 것:

1. **React Hooks**: useState, useEffect 활용
2. **TypeScript**: 인터페이스, 타입 정의
3. **Styled Components**: CSS-in-JS 패턴
4. **상태 관리**: 복잡한 플로우 관리
5. **알고리즘**: 배열 셔플, 점수 계산
6. **수학**: 삼각함수를 이용한 원형 배치
7. **UX 디자인**: 진행률 표시, 애니메이션
8. **반응형 디자인**: 미디어 쿼리 활용

---

## 📝 결론

**테토-에겐 성격 유형 테스트**는 React와 TypeScript를 활용한 잘 구조화된 웹 애플리케이션입니다. 사용자 친화적인 UI/UX와 재미있는 콘텐츠로 성격 테스트를 제공하며, 연애 먹이사슬이라는 독특한 개념을 시각화하여 흥미를 더합니다.

현재는 단일 파일에 모든 로직이 집중되어 있지만, 컴포넌트 분리와 상태 관리 개선을 통해 더욱 확장 가능하고 유지보수하기 쉬운 구조로 발전시킬 수 있습니다.

---

## 📅 문서 작성 정보

- **작성일**: 2025-10-02
- **분석 도구**: Windsurf AI
- **프로젝트 버전**: 0.1.0
