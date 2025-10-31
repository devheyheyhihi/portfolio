# Windsurf - Fire Chicken 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Fire Chicken Coin  
**프로젝트 타입**: Solana 블록체인 기반 밈코인 웹사이트  
**프레임워크**: Next.js 15.1.4 (App Router)  
**언어**: TypeScript  
**스타일링**: Tailwind CSS  

---

## 🏗️ 프로젝트 구조

```
fire_chicken/
├── app/
│   ├── components/          # 재사용 가능한 컴포넌트
│   │   ├── Header.tsx       # 네비게이션 헤더
│   │   ├── SectionContainer.tsx  # 섹션 래퍼 컴포넌트
│   │   └── mobile/          # 모바일 전용 컴포넌트
│   ├── section/             # 페이지 섹션들
│   │   ├── Section1.tsx     # 홈 섹션
│   │   ├── Section2.tsx     # About 섹션
│   │   ├── Section3.tsx     # How to Get 섹션 (Raydium 연동)
│   │   ├── Section4.tsx     # Tokenomics 섹션
│   │   ├── Section5.tsx     # Roadmap 섹션
│   │   └── SectionMobile.tsx # 모바일 통합 섹션
│   ├── multisend/           # 토큰 대량 전송 기능
│   │   └── page.tsx         # Phantom 지갑 연동 멀티센드
│   ├── multisend2/          # 대체 멀티센드 구현
│   ├── commingsoon/         # Coming Soon 페이지
│   ├── tr/                  # 기타 페이지
│   ├── layout.tsx           # 루트 레이아웃
│   ├── page.tsx             # 메인 페이지
│   └── globals.css          # 글로벌 스타일
├── public/                  # 정적 자산
│   ├── mobile/              # 모바일 전용 이미지
│   ├── section1-bg.png      # 섹션 배경 이미지들
│   ├── *.webm               # 배경 애니메이션 비디오
│   └── fire_chicken_*.png   # 브랜드 이미지
├── test-ledger/             # Ledger 테스트 관련
├── package.json             # 프로젝트 의존성
├── tailwind.config.ts       # Tailwind 설정 (커스텀 애니메이션)
├── tsconfig.json            # TypeScript 설정
└── next.config.ts           # Next.js 설정
```

---

## 🔧 기술 스택

### Core Technologies
- **Next.js 15.1.4**: React 프레임워크 (App Router 사용)
- **React 19.0.0**: UI 라이브러리
- **TypeScript 5.7.3**: 타입 안정성

### Blockchain & Wallet
- **@solana/web3.js (^1.98.0)**: Solana 블록체인 상호작용
- **@solana/spl-token (^0.4.13)**: SPL 토큰 표준 구현
- **@phantom/wallet-sdk (^0.0.23)**: Phantom 지갑 연동

### Styling & UI
- **Tailwind CSS (^3.4.17)**: 유틸리티 기반 CSS 프레임워크
- **tailwind-merge (^2.6.0)**: Tailwind 클래스 병합 유틸리티
- **react-tooltip (^5.28.0)**: 툴팁 컴포넌트

### Utilities
- **xlsx (^0.18.5)**: Excel 파일 처리 (멀티센드 기능용)

---

## 🎨 주요 기능

### 1. 랜딩 페이지 (메인 사이트)

#### 반응형 디자인
- **PC 버전**: 스냅 스크롤 기능이 있는 5개 섹션
- **모바일 버전**: 통합된 단일 스크롤 페이지

#### 섹션 구성
1. **Section 1 (HOME)**: 
   - 헤더 네비게이션
   - 브랜드 소개

2. **Section 2 (ABOUT)**:
   - Fire Chicken 코인 소개

3. **Section 3 (HOW TO GET)**:
   - Raydium DEX 연동
   - 토큰 구매 가이드
   - Raydium 바로가기 버튼 (외부 링크)

4. **Section 4 (TOKENOMICS)**:
   - 토큰 경제학 정보
   - 토큰 분배 차트

5. **Section 5 (ROADMAP)**:
   - 프로젝트 로드맵

#### 애니메이션 & UX
- **커스텀 Tailwind 애니메이션**:
  - `fadeIn`: 페이드 인 효과
  - `grow`: 확대 애니메이션
  - `flyOut1-4`: 4방향 날아가는 효과
  - `shake/shake2`: 진동 효과
  - `contentShow/Hide`: 콘텐츠 표시/숨김

- **스무스 스크롤**: 네비게이션 클릭 시 부드러운 스크롤
- **호버 효과**: 네비게이션 아이템 스케일 변환

### 2. 멀티센드 기능 (`/multisend`)

#### 핵심 기능
- **Phantom 지갑 연동**: Solana 지갑 연결
- **Excel 파일 업로드**: 수령인 주소와 금액 일괄 업로드
- **ATA (Associated Token Account) 관리**:
  - ATA 존재 여부 확인
  - 없는 경우 자동 생성
  - 실패한 ATA 추적 및 재시도

#### 처리 프로세스
1. **Step 1: ATA 초기화**
   - 수령인 ATA 확인
   - 없는 경우 생성 트랜잭션 전송
   - 청크 단위 배치 처리 (기본 10개씩)

2. **Step 2: 토큰 전송**
   - ATA가 있는 수령인에게 토큰 전송
   - 실패/성공 추적
   - 진행률 표시

#### 고급 기능
- **청크 크기 조절**: 배치 처리 크기 커스터마이징
- **단일 처리 모드**: 하나씩 순차 처리
- **중지 기능**: 진행 중 중단 가능
- **실패 추적**: 
  - ATA 생성 실패 목록
  - 토큰 전송 실패 목록
- **Excel 다운로드**: 실패 목록 Excel 파일 다운로드

#### 주소 필터링
- 잘못된 형식의 주소 자동 필터링
- 필터링된 주소 목록 표시

---

## 🎯 사용자 플로우

### 일반 방문자
1. 랜딩 페이지 접속
2. 섹션별 정보 탐색 (스냅 스크롤)
3. "HOW TO GET" 섹션에서 Raydium 링크 클릭
4. 외부 DEX에서 토큰 구매

### 토큰 배포자 (멀티센드)
1. `/multisend` 페이지 접속
2. Phantom 지갑 연결
3. Excel 파일 업로드 (주소, 금액)
4. ATA 초기화 실행
5. 토큰 전송 실행
6. 실패 목록 다운로드 (필요시)

---

## 📱 반응형 디자인

### 브레이크포인트
- **모바일**: `< 640px` (sm 미만)
- **PC**: `≥ 640px` (sm 이상)

### 모바일 최적화
- 별도의 `SectionMobile` 컴포넌트
- 간소화된 네비게이션
- 터치 최적화된 UI
- 모바일 전용 이미지 자산

---

## 🔐 보안 & 블록체인

### 지갑 연동
- Phantom 지갑 Provider 사용
- 사용자 승인 기반 트랜잭션
- 공개키만 저장 (개인키 미접근)

### 트랜잭션 처리
- **Compute Budget 설정**: 가스비 최적화
- **에러 핸들링**: 실패 시 재시도 로직
- **배치 처리**: 네트워크 부하 분산

---

## 🚀 성능 최적화

### 이미지 최적화
- Next.js Image 컴포넌트 사용
- WebP 포맷 활용
- 적절한 이미지 크기 설정

### 애니메이션
- CSS 기반 애니메이션 (GPU 가속)
- 비디오 배경 (WebM 포맷)

### 코드 분할
- App Router 기반 자동 코드 분할
- 동적 임포트 가능

---

## 📦 의존성 관리

### 패키지 매니저
- npm, yarn 모두 지원
- `package-lock.json` 및 `yarn.lock` 존재

### 주요 의존성 버전
```json
{
  "next": "^15.1.4",
  "react": "^19.0.0",
  "@solana/web3.js": "^1.98.0",
  "@phantom/wallet-sdk": "^0.0.23",
  "tailwindcss": "^3.4.17"
}
```

---

## 🛠️ 개발 환경

### 스크립트
```bash
npm run dev      # 개발 서버 (Turbopack 사용)
npm run build    # 프로덕션 빌드
npm run start    # 프로덕션 서버
npm run lint     # ESLint 실행
```

### 개발 서버
- **포트**: 3000 (기본값)
- **핫 리로드**: 자동 새로고침
- **Turbopack**: 빠른 빌드 도구

---

## 🎨 디자인 시스템

### 색상 테마
- **주요 색상**: 빨강 계열 (`#9C0404`)
- **배경**: 다크 테마 기반
- **텍스트**: 흰색 기본

### 폰트
- **Geist Sans**: 본문 텍스트
- **Geist Mono**: 코드/숫자

### 레이아웃
- **스냅 스크롤**: 섹션별 전체 화면
- **Flexbox**: 레이아웃 구성
- **Grid**: 필요시 사용

---

## 🔄 상태 관리

### 클라이언트 상태
- React Hooks (`useState`, `useEffect`, `useRef`)
- 로컬 상태 관리 (전역 상태 관리 라이브러리 미사용)

### 주요 상태
- **지갑 연결 상태**: `publicKey`
- **파일 업로드**: `file`, `recipients`
- **처리 진행률**: `progress`, `status`
- **실패 추적**: `failedATAs`, `failedTokens`

---

## 🐛 에러 핸들링

### 지갑 연동
- Phantom 미설치 감지
- 연결 실패 처리

### 트랜잭션
- 네트워크 오류 재시도
- 실패 트랜잭션 로깅
- 사용자 피드백 제공

### 파일 처리
- 잘못된 주소 필터링
- Excel 파싱 에러 처리

---

## 📊 프로젝트 특징

### 강점
1. **완전한 반응형**: PC/모바일 최적화
2. **블록체인 통합**: Solana 네이티브 지원
3. **사용자 친화적**: 직관적인 UI/UX
4. **배치 처리**: 효율적인 대량 전송
5. **에러 복구**: 실패 추적 및 재시도

### 개선 가능 영역
1. **전역 상태 관리**: Zustand/Recoil 도입 고려
2. **테스트 코드**: 단위/통합 테스트 추가
3. **로깅**: 구조화된 로깅 시스템
4. **다국어 지원**: i18n 구현
5. **SEO 최적화**: 메타데이터 강화

---

## 🔮 향후 개선 방향

### 기능 추가
- [ ] 지갑 연결 상태 지속성 (로컬 스토리지)
- [ ] 트랜잭션 히스토리 조회
- [ ] 실시간 토큰 가격 표시
- [ ] 커뮤니티 기능 (소셜 링크)

### 기술 개선
- [ ] 서버 컴포넌트 활용 확대
- [ ] 이미지 최적화 강화
- [ ] 번들 사이즈 최적화
- [ ] 접근성(A11y) 개선

### 운영
- [ ] 모니터링 시스템 구축
- [ ] CI/CD 파이프라인
- [ ] 성능 메트릭 수집
- [ ] 에러 트래킹 (Sentry 등)

---

## 📝 결론

Fire Chicken 프로젝트는 **Solana 블록체인 기반의 밈코인 프로젝트**로, 다음과 같은 특징을 가지고 있습니다:

- ✅ **현대적인 웹 기술 스택** (Next.js 15, React 19)
- ✅ **완전한 블록체인 통합** (Phantom 지갑, SPL 토큰)
- ✅ **실용적인 유틸리티** (멀티센드 기능)
- ✅ **세련된 UI/UX** (애니메이션, 반응형)
- ✅ **확장 가능한 구조** (컴포넌트 기반)

프로젝트는 **프로덕션 준비 상태**이며, 추가 기능 개발과 최적화를 통해 더욱 발전시킬 수 있는 견고한 기반을 갖추고 있습니다.

---

**문서 작성일**: 2025-10-13  
**프로젝트 버전**: 0.1.0  
**작성자**: Windsurf AI Assistant
