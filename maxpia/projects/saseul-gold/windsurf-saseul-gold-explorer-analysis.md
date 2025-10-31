# Windsurf - Saseul Gold Explorer 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: Saseul Gold Explorer (bgeo-explorer)  
**버전**: 0.0.0  
**타입**: 블록체인 익스플로러 웹 애플리케이션  
**기술 스택**: React + TypeScript + Vite

### 프로젝트 설명
Saseul Gold 블록체인 네트워크의 데이터를 탐색하고 시각화하는 블록체인 익스플로러입니다. 블록, 트랜잭션, 주소, 마이닝 정보, 스왑 내역 등을 조회하고 분석할 수 있는 웹 기반 대시보드를 제공합니다.

---

## 🏗️ 기술 스택

### 핵심 프레임워크 & 라이브러리
- **React** 18.2.0 - UI 프레임워크
- **TypeScript** 5.5.3 - 타입 안정성
- **Vite** 5.4.8 - 빌드 도구 및 개발 서버
- **React Router DOM** 6.4.0 - 라우팅

### 상태 관리 & 데이터 페칭
- **Jotai** 2.10.1 - 원자적 상태 관리
- **Jotai Location** 0.5.5 - 라우팅 상태 관리
- **TanStack React Query** 5.59.15 - 서버 상태 관리 및 캐싱
- **Axios** 0.27.2 - HTTP 클라이언트

### UI 컴포넌트 & 스타일링
- **Tailwind CSS** 3.4.15 - 유틸리티 기반 CSS 프레임워크
- **Radix UI** - 접근성 높은 UI 컴포넌트
  - Dialog, Dropdown Menu, Slot, Tooltip
- **Framer Motion** 11.11.9 - 애니메이션
- **Styled Components** 5.3.6 - CSS-in-JS
- **Lucide React** 0.453.0 - 아이콘
- **React Icons** 4.7.1 - 추가 아이콘

### 데이터 시각화
- **Recharts** 2.13.0 - 차트 라이브러리
- **Leaflet** 1.9.4 - 지도 시각화
- **React Leaflet** 4.2.1 - React용 Leaflet 래퍼

### 유틸리티
- **date-fns** 4.1.0 - 날짜 처리
- **crypto-js** 4.2.0 - 암호화
- **tweetnacl** 1.0.3 - 암호화 라이브러리
- **react-hot-toast** 2.4.1 - 토스트 알림
- **react-spinners** 0.13.6 - 로딩 스피너

---

## 📁 프로젝트 구조

```
saseul-gold-explorer/
├── public/                    # 정적 리소스 (이미지, 아이콘, 비디오)
├── resources/                 # 추가 리소스
├── src/
│   ├── api/                  # API 통신 레이어
│   │   ├── index.ts         # API 엔드포인트 및 함수들
│   │   └── crypto.ts        # 암호화 관련 유틸리티
│   ├── atoms/               # Jotai 상태 관리
│   │   ├── search.ts       # 검색 상태
│   │   └── session.ts      # 세션 상태
│   ├── components/          # 재사용 가능한 컴포넌트
│   │   ├── charts/         # 차트 컴포넌트
│   │   ├── common/         # 공통 컴포넌트
│   │   ├── notice/         # 공지사항 컴포넌트
│   │   ├── ui/             # UI 기본 컴포넌트
│   │   ├── BasicTable.tsx
│   │   ├── Footer.tsx
│   │   ├── Header.tsx
│   │   ├── Logo.tsx
│   │   ├── Navigation.tsx
│   │   ├── Pagination.tsx
│   │   ├── ScrollToTop.tsx
│   │   ├── SearchBar.tsx
│   │   └── TableItem.tsx
│   ├── constants/           # 상수 정의
│   │   ├── fetchPlaceholder.ts
│   │   ├── index.ts
│   │   └── navigation.ts
│   ├── hooks/              # 커스텀 훅
│   │   ├── useApi.ts      # API 호출 훅
│   │   ├── useNetwork.ts  # 네트워크 상태 훅
│   │   ├── useSwap.ts     # 스왑 관련 훅
│   │   └── useTimeSeriesData.ts
│   ├── lib/               # 유틸리티 함수
│   ├── pages/             # 페이지 컴포넌트
│   │   ├── BlockListPage/      # 블록 목록
│   │   ├── HomePage/           # 메인 대시보드
│   │   ├── MiningListPage/     # 마이닝 목록
│   │   ├── NoticePage/         # 공지사항
│   │   ├── RichlistPage/       # 부자 목록
│   │   ├── SearchedPage/       # 검색 결과
│   │   ├── SupplyPage/         # 공급량 정보
│   │   ├── SwapListPage/       # 스왑 목록
│   │   ├── TransactionsPage/   # 트랜잭션 목록
│   │   └── routes.tsx          # 라우트 정의
│   ├── types/             # TypeScript 타입 정의
│   │   ├── be.ts
│   │   ├── index.ts
│   │   ├── raw.ts
│   │   └── swap.ts
│   ├── App.tsx            # 메인 앱 컴포넌트
│   ├── App.css
│   ├── ErrorPage.tsx      # 에러 페이지
│   ├── main.tsx           # 앱 진입점
│   └── index.css          # 글로벌 스타일
├── dist/                  # 빌드 결과물
├── index.html             # HTML 템플릿
├── package.json           # 프로젝트 의존성
├── tsconfig.json          # TypeScript 설정
├── vite.config.ts         # Vite 설정
├── tailwind.config.js     # Tailwind CSS 설정
├── postcss.config.js      # PostCSS 설정
├── eslint.config.js       # ESLint 설정
└── vercel.json            # Vercel 배포 설정
```

---

## 🔌 API 엔드포인트

### 네트워크 정보
- **네트워크 베이스 URL**: `https://raw.saseulgold.org`
- **백엔드 베이스 URL**: `https://api.saseulgold.org`

### 주요 API 엔드포인트

#### 1. 요약 정보
- `GET /ext/summary` - 네트워크 요약 정보

#### 2. 블록 관련
- `GET /rawrequest/blocks` - 블록 목록 (페이지네이션)
- `GET /rawrequest/block/:height` - 특정 높이의 블록
- `GET /rawrequest/height` - 현재 블록 높이
- `GET /rawrequest/blocks/count` - 총 블록 수

#### 3. 트랜잭션 관련
- `GET /txs/all` - 모든 트랜잭션
- `GET /txs/:address` - 특정 주소의 트랜잭션
- `GET /txs/logs/:type` - 트랜잭션 로그

#### 4. 마이닝 관련
- `GET /mining/` - 활성 노드 수
- `GET /mining/total` - 총 노드 수
- `GET /mining/info` - 노드 위치 정보
- `GET /mining/log` - 마이닝 로그
- `GET /mining/detail/:txhash` - 마이닝 상세 정보
- `GET /mining/log/total` - 총 마이닝 로그 수
- `GET /mining/topholders` - 부자 목록

#### 5. 스왑 관련
- `GET /api/swap` - 스왑 히스토리
- `GET /api/swap/:address` - 특정 주소의 스왑 히스토리
- `GET /api/swap/total` - 총 스왑 수

#### 6. 토큰 관련
- `GET /rawrequest/tokens` - 토큰 정보

#### 7. 기타
- `GET https://api-ext.bgeoex.com/tps` - TPS (초당 트랜잭션) 데이터
- `POST /rawrequest/` - Raw 요청
- `POST /broadcast/` - 브로드캐스트

---

## 🎨 주요 기능

### 1. 홈페이지 (Dashboard)
- **네트워크 요약 카드**: 주요 네트워크 통계 표시
- **노드 맵**: 전 세계 노드 위치 시각화
- **검색 기능**: 블록, 트랜잭션, 주소, 마이닝 검색
- **최신 데이터 테이블**:
  - 최근 블록
  - 최근 트랜잭션
  - 마이닝 로그
  - 스왑 히스토리

### 2. 블록 탐색
- 블록 목록 페이지 (페이지네이션)
- 블록 상세 정보
- 블록 높이 및 해시 검색

### 3. 트랜잭션 탐색
- 트랜잭션 목록
- 트랜잭션 상세 정보
- 주소별 트랜잭션 조회

### 4. 마이닝 정보
- 마이닝 로그 목록
- 마이닝 상세 정보
- 노드 통계

### 5. 스왑 기능
- 스왑 히스토리
- 토큰 정보 조회
- 주소별 스왑 내역

### 6. 부가 기능
- Richlist (상위 홀더 목록)
- 공급량 정보
- 공지사항
- 반응형 디자인

---

## 🔧 개발 환경 설정

### 필수 요구사항
- Node.js (권장: v18 이상)
- Yarn 1.22.22

### 설치 및 실행

```bash
# 의존성 설치
yarn install

# 개발 서버 실행
yarn dev

# 프로덕션 빌드
yarn build

# 빌드 결과 미리보기
yarn preview

# 린트 실행
yarn lint
```

### 환경 변수
프로젝트는 하드코딩된 API 엔드포인트를 사용하고 있습니다:
- Network Base URL: `https://raw.saseulgold.org`
- Backend Base URL: `https://api.saseulgold.org`
- TPS API: `https://api-ext.bgeoex.com/tps`

---

## 🎯 상태 관리 전략

### Jotai Atoms
- **search.ts**: 검색 관련 상태
- **session.ts**: 세션 및 사용자 상태

### React Query
- 서버 데이터 캐싱 및 동기화
- 자동 리페칭 및 백그라운드 업데이트
- 로딩 및 에러 상태 관리

### Custom Hooks
- **useApi**: API 호출 및 데이터 페칭
- **useNetwork**: 네트워크 상태 모니터링
- **useSwap**: 스왑 관련 데이터 처리
- **useTimeSeriesData**: 시계열 데이터 처리

---

## 🎨 UI/UX 특징

### 디자인 시스템
- **색상 테마**: 골드/마블 테마
- **타이포그래피**: Pretendard 폰트
- **반응형**: 모바일, 태블릿, 데스크톱 지원

### 컴포넌트 라이브러리
- Radix UI 기반 접근성 높은 컴포넌트
- Tailwind CSS 유틸리티 클래스
- 커스텀 UI 컴포넌트 (Badge, Button, Card, Input, Table 등)

### 애니메이션
- Framer Motion을 활용한 부드러운 전환
- 스크롤 기반 패럴랙스 효과
- 호버 및 인터랙션 애니메이션

---

## 🚀 배포

### Vercel 설정
프로젝트는 Vercel을 통해 배포되도록 설정되어 있습니다 (`vercel.json` 참조).

### 빌드 프로세스
1. TypeScript 컴파일
2. Vite 번들링
3. 정적 파일 생성 (`dist/` 디렉토리)

---

## 📊 데이터 플로우

```
사용자 인터랙션
    ↓
React Component
    ↓
Custom Hook (useApi, useNetwork, etc.)
    ↓
React Query (캐싱 레이어)
    ↓
API Layer (axios)
    ↓
Backend API (Saseul Gold Network)
    ↓
데이터 반환 및 UI 업데이트
```

---

## 🔐 보안 고려사항

### 암호화
- `crypto-js`: 데이터 암호화
- `tweetnacl`: 공개키 암호화

### API 인증
- Bearer 토큰 기반 인증 (현재 플레이스홀더 사용)

---

## 📝 타입 시스템

### 주요 타입 정의
- **Block**: 블록 데이터 구조
- **Transaction**: 트랜잭션 데이터 구조
- **Summary**: 네트워크 요약 정보
- **TPSData**: TPS 데이터
- **SwapHistory**: 스왑 히스토리
- **TokensResponse**: 토큰 정보

---

## 🐛 에러 처리

### 전역 에러 처리
- `ErrorPage.tsx`: 라우팅 에러 페이지
- React Query 에러 바운더리
- Axios 인터셉터를 통한 에러 처리

### 사용자 피드백
- `react-hot-toast`: 토스트 알림
- 로딩 스피너 (`react-spinners`)
- 스켈레톤 UI

---

## 🔄 최적화 전략

### 성능 최적화
- React.memo를 통한 불필요한 리렌더링 방지
- React Query 캐싱 전략
- 코드 스플리팅 (Vite 자동 처리)
- 이미지 최적화 (WebP 포맷 사용)

### 번들 최적화
- Vite의 트리 쉐이킹
- SWC를 통한 빠른 컴파일
- 동적 임포트

---

## 📚 주요 라우트

| 경로 | 페이지 | 설명 |
|------|--------|------|
| `/` | HomePage | 메인 대시보드 |
| `/search` | SearchedPage | 검색 결과 |
| `/:elementType/:hash` | SearchedPage | 상세 정보 (블록/트랜잭션/주소) |
| `/supply` | SupplyPage | 공급량 정보 |
| `/richlist` | RichlistPage | 부자 목록 |
| `/transactions` | TransactionsPage | 트랜잭션 목록 |
| `/blocks` | BlockListPage | 블록 목록 |
| `/mining` | MiningListPage | 마이닝 목록 |
| `/swaps` | SwapListPage | 스왑 목록 |
| `/notice` | NoticePage | 공지사항 |

---

## 🛠️ 개발 도구

### 린팅 & 포맷팅
- **ESLint**: 코드 품질 검사
- **TypeScript ESLint**: TypeScript 전용 린트 규칙

### 개발 서버
- **Vite Dev Server**: HMR (Hot Module Replacement) 지원
- **프록시 설정**: `/api` 경로를 `http://bgeoex.com:3000`으로 프록시

---

## 📈 향후 개선 사항

### 기능 추가
- [ ] 실시간 데이터 업데이트 (WebSocket)
- [ ] 고급 차트 및 분석 도구
- [ ] 사용자 맞춤 대시보드
- [ ] 알림 시스템
- [ ] 다국어 지원 (i18n)

### 기술 개선
- [ ] 환경 변수 관리 개선
- [ ] 테스트 커버리지 추가 (Jest, React Testing Library)
- [ ] CI/CD 파이프라인 구축
- [ ] 성능 모니터링 (Sentry, Analytics)
- [ ] PWA 지원

### UX 개선
- [ ] 다크 모드 지원
- [ ] 접근성 개선 (WCAG 준수)
- [ ] 모바일 최적화
- [ ] 로딩 성능 개선

---

## 👥 프로젝트 정보

### 외부 링크
- **공식 웹사이트**: https://saseulgold.org/
- **지갑**: https://wallet.saseulgold.org/
- **GitHub**: https://github.com/Saseulgold/saseulgold-network
- **Telegram**: https://t.me/SASEULGOLD

### 패키지 매니저
- Yarn 1.22.22

---

## 📄 라이선스

프로젝트는 Private 설정으로 되어 있습니다.

---

## 🔍 코드 품질

### TypeScript 설정
- Strict 모드 활성화
- 경로 별칭 (`@/`) 사용
- 여러 tsconfig 파일로 분리 (app, node)

### 코드 스타일
- ESLint 규칙 적용
- React Hooks 린트 규칙
- React Refresh 플러그인

---

## 💡 개발 팁

### 디버깅
- React DevTools 사용 권장
- React Query DevTools 활성화 가능
- Vite의 소스맵 지원

### 개발 워크플로우
1. 기능 브랜치 생성
2. 로컬에서 개발 및 테스트
3. 린트 및 타입 체크
4. 빌드 테스트
5. PR 생성 및 리뷰

---

## 📞 문의 및 지원

프로젝트 관련 문의사항은 Saseul Gold 커뮤니티를 통해 문의하시기 바랍니다.

- **Telegram**: https://t.me/SASEULGOLD
- **GitHub Issues**: https://github.com/Saseulgold/saseulgold-network/issues

---

*문서 작성일: 2025-10-17*  
*분석 도구: Windsurf AI*
