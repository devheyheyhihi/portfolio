# Windsurf - Sasuel Wallet 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: Sasuel Wallet (사슬 지갑)  
**프로젝트 타입**: 웹 기반 암호화폐 지갑 애플리케이션  
**분석 날짜**: 2025-10-20  
**프로젝트 버전**: 0.1.0

### 프로젝트 설명
Sasuel Wallet은 SASEUL GOLD 블록체인 네트워크를 위한 웹 기반 암호화폐 지갑입니다. 사용자 친화적인 인터페이스와 높은 보안성을 갖추고 있으며, 블록체인 네트워크와의 원활한 상호작용을 지원합니다.

---

## 🛠 기술 스택

### 프론트엔드 프레임워크
- **Next.js 14.2.5** - React 기반 풀스택 프레임워크 (App Router 사용)
- **React 18** - UI 라이브러리
- **TypeScript 5.6.2** - 타입 안정성

### 스타일링
- **Tailwind CSS 3.4.1** - 유틸리티 우선 CSS 프레임워크
- **Pretendard** - 한글 폰트
- **Radix UI** - 접근성 높은 UI 컴포넌트 라이브러리
  - Avatar, Checkbox, Dialog, Dropdown Menu, Icons
  - Label, Navigation Menu, Progress, Radio Group
  - Scroll Area, Select, Separator, Slot, Switch, Tabs, Tooltip
- **class-variance-authority** - 컴포넌트 변형 관리
- **tailwindcss-animate** - 애니메이션 유틸리티
- **lucide-react** - 아이콘 라이브러리

### 상태 관리
- **Jotai 2.10.0** - 원자적 상태 관리
- **@tanstack/react-query 5.59.13** - 서버 상태 관리 및 캐싱

### 블록체인 & 암호화
- **saseul 2.8.1** - SASEUL 블록체인 SDK
- **bip39 3.1.0** - 니모닉 생성 및 검증
- **tweetnacl 1.0.3** - 암호화 라이브러리
- **crypto-js 4.2.0** - 암호화 유틸리티
- **encrypt-storage 2.14.6** - 로컬 스토리지 암호화
- **react-secure-storage 1.3.2** - 보안 스토리지

### 데이터 시각화
- **Recharts 2.13.3** - 차트 라이브러리
- **react-countup 6.5.3** - 숫자 애니메이션

### 유틸리티
- **axios 1.7.7** - HTTP 클라이언트
- **next-intl 3.19.0** - 다국어 지원 (한국어, 영어, 일본어)
- **next-themes 0.4.3** - 테마 관리 (라이트/다크 모드)
- **date-fns 4.1.0** - 날짜 처리
- **decimal.js 10.4.3** - 정밀한 숫자 계산
- **qrcode.react 4.0.1** - QR 코드 생성
- **file-saver 2.0.5** - 파일 다운로드
- **js-cookie 3.0.5** - 쿠키 관리
- **react-hook-form 7.53.2** - 폼 관리
- **react-hot-toast 2.4.1** - 토스트 알림

### 개발 도구
- **ESLint** - 코드 린팅
- **Prettier** - 코드 포맷팅
- **Knip 5.30.6** - 미사용 코드 탐지
- **@svgr/webpack** - SVG를 React 컴포넌트로 변환

---

## 📁 프로젝트 구조

```
sasuel-wallet/
├── public/                    # 정적 파일
├── messages/                  # 다국어 리소스
│   ├── ko.json               # 한국어
│   ├── en.json               # 영어
│   └── ja.json               # 일본어
├── src/
│   ├── api/                  # API 관련 코드
│   │   ├── saseul-gold/     # SASEUL GOLD 블록체인 API
│   │   ├── saseul-old/      # 레거시 SASEUL API
│   │   └── token/           # 토큰 관련 API
│   ├── app/                  # Next.js App Router
│   │   ├── (private)/       # 인증 필요 페이지
│   │   │   ├── page.tsx     # 메인 대시보드
│   │   │   ├── recipients/  # 주소록
│   │   │   ├── settings/    # 설정
│   │   │   └── transactions/ # 거래 내역
│   │   ├── (public)/        # 공개 페이지
│   │   │   └── import/      # 지갑 가져오기
│   │   ├── components/      # 공통 컴포넌트
│   │   ├── assets/          # SVG 아이콘
│   │   ├── layout.tsx       # 루트 레이아웃
│   │   └── globals.css      # 전역 스타일
│   ├── atoms/               # Jotai 상태 관리
│   │   ├── walletAtom.ts    # 지갑 상태
│   │   ├── recipientsAtom.ts # 주소록 상태
│   │   └── apiAtoms.ts      # API 상태
│   ├── components/          # 재사용 가능한 컴포넌트
│   │   ├── ui/             # UI 기본 컴포넌트
│   │   ├── providers/      # Context Providers
│   │   └── common/         # 공통 컴포넌트
│   ├── hooks/              # 커스텀 훅
│   │   ├── useApi.ts       # API 호출 훅
│   │   ├── useToken.ts     # 토큰 관련 훅
│   │   ├── useNetwork.ts   # 네트워크 정보 훅
│   │   ├── useWalletData.tsx # 지갑 데이터 훅
│   │   ├── useBalanceHistory.ts # 잔액 히스토리 훅
│   │   └── useTimeSeriesData.ts # 시계열 데이터 훅
│   ├── constants/          # 상수 정의
│   │   ├── chain.ts        # 체인 설정
│   │   ├── chains.tsx      # 체인 목록
│   │   ├── wallet.ts       # 지갑 설정
│   │   └── saseul-old.ts   # 레거시 설정
│   ├── types/              # TypeScript 타입 정의
│   ├── lib/                # 유틸리티 함수
│   ├── i18n/               # 국제화 설정
│   └── middleware.ts       # Next.js 미들웨어
├── tailwind.config.ts      # Tailwind 설정
├── tsconfig.json           # TypeScript 설정
├── next.config.mjs         # Next.js 설정
├── components.json         # shadcn/ui 설정
├── package.json            # 의존성 관리
├── vercel.json            # Vercel 배포 설정
└── sasuel-wallet-requirements.md # 기능 요구사항 문서
```

---

## 🎯 주요 기능

### 1. 메인 대시보드 (`/`)
- **자산 요약 정보**
  - 보유 자산 목록 및 총 자산 가치 표시
  - 지갑 주소 및 QR 코드 표시 (복사 기능)
  - 자산별 잔액 및 가치 변동 그래프
  
- **네트워크 상태 카드**
  - 블록수, 트랜잭션수, 노드수, 난이도 정보
  - 실시간 미니 그래프
  
- **최근 거래 내역**
  - 최근 5개 트랜잭션 요약
  - 거래 유형별 아이콘 표시
  
- **뉴스 피드**
  - 암호화폐 관련 최신 뉴스 및 공지사항

### 2. 지갑 관리 (`/import`)
- **지갑 생성**
  - BIP39 표준 준수 니모닉 생성
  - 12/24 단어 선택 옵션
  
- **지갑 복구**
  - 니모닉 구문 또는 개인키를 통한 복구
  - 입력 검증 및 오류 처리
  
- **키 백업**
  - 암호화된 니모닉/개인키 파일 백업
  - AES-256 암호화 및 패스워드 보호

### 3. 트랜잭션 기능 (`/transactions`)
- **송금 기능**
  - 수신자 주소 입력 및 금액 설정
  - 주소 검증 및 잔액 확인
  - 가스비 설정 및 거래 미리보기
  
- **수신 기능**
  - 내 주소 QR 코드 생성
  - 공유 링크 생성
  
- **트랜잭션 목록**
  - 모든 트랜잭션 내역 시간순 정렬
  - 페이지네이션 또는 무한 스크롤
  
- **트랜잭션 필터링**
  - 날짜, 금액, 유형별 필터링

### 4. 주소록 기능 (`/recipients`)
- **수취인 관리**
  - 자주 사용하는 주소 추가/편집/삭제
  - 주소 그룹화
  - 로컬 스토리지 암호화 저장
  
- **주소 라벨링**
  - 주소에 이름/메모 부여
  - 이모지 지원 및 컬러 태그

### 5. 설정 (`/settings`)
- **언어 설정**
  - 다국어 지원 (한국어/영어/일본어)
  - 자동 언어 감지
  
- **테마 설정**
  - 라이트/다크 모드
  - 시스템 설정 연동
  
- **보안 설정**
  - 자동 로그아웃 시간 설정
  - 로그인 방식 설정

---

## 🔒 보안 기능

### 클라이언트 측 보안
- **개인키 보호**
  - 개인키는 절대 서버로 전송되지 않음
  - 클라이언트에서만 처리
  
- **데이터 암호화**
  - AES-256 암호화로 로컬 스토리지 보호
  - encrypt-storage 및 react-secure-storage 사용
  
- **세션 관리**
  - 자동 세션 타임아웃
  - 다중 장치 로그인 관리

### 네트워크 보안
- **HTTPS 강제**
- **XSS, CSRF 방어**
- **입력 검증 및 살균**

---

## 🎨 UI/UX 특징

### 디자인 시스템
- **Neumorphism 디자인**
  - 부드러운 그림자 효과
  - 입체감 있는 UI 요소
  - 커스텀 box-shadow 정의
  
- **컬러 시스템**
  - Gold 테마 (SASEUL GOLD 브랜딩)
  - 다크/라이트 모드 지원
  - CSS 변수 기반 테마 관리

### 반응형 디자인
- **모바일 우선 접근법**
- **다양한 화면 크기 대응**
- **터치 제스처 지원**
- **PWA 지원 가능**

### 접근성
- **WCAG 2.1 AA 준수 목표**
- **Radix UI로 접근성 보장**
- **키보드 네비게이션 지원**

---

## 📊 상태 관리 아키텍처

### Jotai Atoms
```typescript
// walletAtom.ts - 지갑 상태 관리
- 지갑 주소
- 공개키/개인키
- 잔액 정보

// recipientsAtom.ts - 주소록 상태 관리
- 저장된 주소 목록
- 주소 라벨 및 메모

// apiAtoms.ts - API 상태 관리
- API 엔드포인트 설정
- 네트워크 상태
```

### React Query
- **서버 상태 캐싱**
- **자동 리페칭**
- **낙관적 업데이트**
- **에러 핸들링**

---

## 🌐 API 구조

### SASEUL GOLD API (`/api/saseul-gold`)
- **crypto.ts** - 암호화 관련 함수
- **index.ts** - 메인 API 함수들
- **types.ts** - 타입 정의

### SASEUL OLD API (`/api/saseul-old`)
- **getSaseulOldBalance.ts** - 잔액 조회
- **getSaseulOldHistory.ts** - 거래 내역 조회
- **sendSaseulOld.ts** - 송금 기능

### Token API (`/api/token`)
- **client.ts** - API 클라이언트
- **formatters/** - 데이터 포맷터
- **raw/** - Raw API 호출
- **types.ts** - 타입 정의
- **utils.ts** - 유틸리티 함수

---

## 🚀 개발 및 배포

### 개발 스크립트
```bash
npm run dev      # 개발 서버 실행
npm run build    # 프로덕션 빌드
npm run start    # 프로덕션 서버 실행
npm run lint     # 린팅 실행
npm run knip     # 미사용 코드 탐지
```

### 배포 환경
- **Vercel** - CI/CD 파이프라인
- **단계별 환경** - 개발, 스테이징, 프로덕션
- **자동화된 테스트 및 검증**

---

## 📈 성능 요구사항

- **페이지 로드 시간**: 2초 이내
- **트랜잭션 처리 응답 시간**: 5초 이내
- **동시 접속자 지원**: 최소 1,000명

---

## 🔄 개발 단계 및 우선순위

### 1단계 (MVP) - 2개월
- ✅ 지갑 생성 및 복구
- ✅ 기본 자산 조회
- ✅ 송금 및 수신 기능
- ✅ 기본 보안 기능

### 2단계 - 2개월
- 🔄 주소록 관리
- 🔄 트랜잭션 상세 내역
- 🔄 네트워크 정보 및 통계
- ✅ 다국어 및 테마 지원

### 3단계 - 2개월
- ⏳ 토큰 관리
- ⏳ 거래소 연동
- ⏳ 모바일 최적화
- ⏳ 알림 및 부가 기능

**범례**: ✅ 완료 | 🔄 진행 중 | ⏳ 예정

---

## 🧪 테스트 전략

### 단위 테스트
- 핵심 유틸리티 함수 테스트
- 암호화/복호화 함수 테스트
- React Testing Library 활용

### 통합 테스트
- API 통합 테스트
- 주요 사용자 흐름 테스트

### 보안 테스트
- 정적 코드 분석
- 취약점 스캐닝
- 침투 테스트

---

## 📝 코드 품질 관리

### 코드 스타일
- **ESLint** - 코드 품질 검사
- **Prettier** - 일관된 코드 포맷팅
- **TypeScript** - 타입 안정성

### 아키텍처 패턴
- **Atomic Design** - 컴포넌트 구성
- **App Router** - Next.js 14 라우팅
- **Server/Client Components** - 최적화된 렌더링

---

## 🔮 향후 개선 사항

### 기능 추가
- [ ] 커스텀 토큰 추가 및 관리
- [ ] 거래소 API 연동
- [ ] 가격 알림 기능
- [ ] 포커스 모드
- [ ] 고급 통계 대시보드

### 성능 최적화
- [ ] 코드 스플리팅 최적화
- [ ] 이미지 최적화
- [ ] 캐싱 전략 개선
- [ ] PWA 기능 추가

### 보안 강화
- [ ] 생체 인증 지원
- [ ] 하드웨어 지갑 연동
- [ ] 다중 서명 지원
- [ ] 정기 보안 감사

---

## 📚 참고 문서

- [Next.js 공식 문서](https://nextjs.org/docs)
- [Tailwind CSS 문서](https://tailwindcss.com/docs)
- [Radix UI 문서](https://www.radix-ui.com/docs)
- [Jotai 문서](https://jotai.org/)
- [React Query 문서](https://tanstack.com/query/latest)
- [BIP39 표준](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

---

## 👥 프로젝트 정보

**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/01-blockchain-projects/maxpia-project/sasuel-wallet`

**주요 설정 파일**:
- `package.json` - 의존성 및 스크립트
- `tsconfig.json` - TypeScript 설정
- `tailwind.config.ts` - Tailwind CSS 설정
- `next.config.mjs` - Next.js 설정
- `components.json` - shadcn/ui 설정
- `vercel.json` - Vercel 배포 설정

---

## 💡 개발 팁

### 로컬 개발 시작하기
```bash
# 의존성 설치
npm install

# 개발 서버 실행
npm run dev

# 브라우저에서 http://localhost:3000 접속
```

### 환경 변수 설정
프로젝트에 필요한 환경 변수가 있다면 `.env.local` 파일을 생성하여 설정하세요.

### 코드 스타일 유지
```bash
# 린팅 실행
npm run lint

# 미사용 코드 탐지
npm run knip
```

---

## 🎯 프로젝트 목표

1. **직관적이고 사용하기 쉬운** 암호화폐 지갑 서비스 제공
2. **높은 수준의 보안**을 유지하며 개인 키 및 자산 보호
3. **블록체인 네트워크의 실시간 정보**와 통계 제공
4. **다양한 기기와 환경**에서 일관된 사용자 경험 제공

---

*이 문서는 Windsurf AI에 의해 자동 생성되었습니다.*  
*마지막 업데이트: 2025-10-20*
