# QCC Explorer 프로젝트 분석 보고서

## 프로젝트 개요
QCC Explorer는 블록체인 익스플로러 애플리케이션으로, Solana 블록체인을 기반으로 한 트랜잭션 및 블록 정보를 조회하고 시각화하는 웹 애플리케이션입니다. 이 프로젝트는 React 기반으로 개발되었으며, 현대적인 웹 기술 스택을 활용하여 사용자 친화적인 인터페이스를 제공합니다.

## 기술 스택

### 핵심 기술
- **프론트엔드**: React 18, TypeScript
- **상태 관리**: React Query (TanStack Query)
- **스타일링**: Tailwind CSS, Styled Components
- **UI 컴포넌트**: Radix UI, Hero Icons, Lucide Icons
- **차트 라이브러리**: Recharts
- **HTTP 클라이언트**: Axios
- **라우팅**: React Router v6
- **애니메이션**: Framer Motion

### 개발 도구
- **빌드 도구**: Create React App
- **패키지 관리자**: Yarn
- **타입 시스템**: TypeScript
- **코드 포맷팅**: ESLint, Prettier

## 프로젝트 구조

```
src/
├── api/                # API 관련 로직
├── components/         # 재사용 가능한 UI 컴포넌트
├── hooks/             # 커스텀 React 훅
├── libs/              # 유틸리티 및 헬퍼 함수
├── pages/             # 페이지 컴포넌트
├── routes/            # 라우팅 설정
├── types/             # TypeScript 타입 정의
├── App.tsx            # 메인 애플리케이션 컴포넌트
└── index.tsx          # 애플리케이션 진입점
```

## 주요 기능

1. **블록체인 데이터 조회**
   - 블록, 트랜잭션, 계정 정보 조회
   - 네트워크 상태 모니터링
   
2. **데이터 시각화**
   - 트랜잭션 흐름 차트
   - 네트워크 활동 지표 대시보드
   
3. **사용자 인터페이스**
   - 반응형 디자인
   - 다크 모드 지원
   - 대화형 차트 및 테이블

## 설치 및 실행 방법

### 필수 조건
- Node.js (v14 이상)
- Yarn 패키지 매니저

### 설치
```bash
git clone [repository-url]
cd qcc-explorer
yarn install
```

### 개발 서버 실행
```bash
yarn start
```

### 프로덕션 빌드
```bash
yarn build
```

## 환경 변수 설정
프로젝트 루트에 `.env` 파일을 생성하고 다음 변수들을 설정해야 합니다:

```env
REACT_APP_API_URL=your_api_url_here
REACT_APP_NETWORK=mainnet|testnet|devnet
```

## 기여 방법
1. 이슈를 생성하여 변경 사항을 논의합니다.
2. 포크를 생성하고 기능 브랜치를 만듭니다.
3. 변경 사항을 커밋하고 PR을 생성합니다.
4. 코드 리뷰를 거쳐 메인 브랜치에 병합합니다.

## 라이선스
이 프로젝트는 [라이선스 유형] 라이선스 하에 있습니다. 자세한 내용은 LICENSE 파일을 참조하세요.

## 연락처
프로젝트 관련 문의는 [이메일/연락처 정보]로 문의해주세요.
