# Windsurf Coin Lotto Frontend

## 📋 프로젝트 개요

**Coin Lotto**는 블록체인 기반의 실시간 복권 게임 플랫폼입니다. 사용자들은 USDT를 사용하여 다양한 룸에 참여하고, 자동화된 추첨 시스템을 통해 당첨자가 결정됩니다. 또한 룰렛 게임 모드도 제공하여 다양한 게임 경험을 제공합니다.

### 주요 특징
- 🎰 **다중 룸 시스템**: 1 USDT, 10 USDT, 50 USDT, 100 USDT 티켓 가격의 4개 룸
- 🎡 **룰렛 게임**: HIGH/LOW 베팅 방식의 실시간 룰렛 게임
- ⏱️ **실시간 타이머**: 각 라운드의 마감 시간을 실시간으로 표시
- 🔐 **JWT 기반 인증**: 안전한 사용자 인증 시스템
- 💰 **오프체인 잔액 관리**: 빠른 게임 진행을 위한 오프체인 USDT 잔액
- 📱 **반응형 디자인**: 모바일과 데스크톱 모두 지원
- 🎨 **실시간 애니메이션**: 추첨 과정의 생동감 있는 시각화

---

## 🛠️ 기술 스택

### 프레임워크 & 라이브러리
- **Next.js 15.4.3** - React 기반 풀스택 프레임워크
- **React 19.1.0** - UI 라이브러리
- **TypeScript 5** - 타입 안정성

### 블록체인 & Web3
- **Wagmi 2.16.1** - React Hooks for Ethereum
- **Viem 2.33.2** - TypeScript Ethereum 라이브러리
- **Ethers.js 6.15.0** - Ethereum 상호작용
- **@web3modal/wagmi 5.1.11** - 지갑 연결 UI
- **Hardhat 2.26.1** - 스마트 컨트랙트 개발 도구

### 상태 관리 & 데이터
- **@tanstack/react-query 5.84.1** - 서버 상태 관리
- **jwt-decode 4.0.0** - JWT 토큰 디코딩
- **Context API** - 전역 상태 관리 (인증)

### 스타일링
- **Tailwind CSS 4.1.11** - 유틸리티 기반 CSS 프레임워크
- **PostCSS 8.5.6** - CSS 전처리
- **Autoprefixer 10.4.21** - CSS 벤더 프리픽스 자동 추가

### 개발 도구
- **ESLint 9** - 코드 품질 관리
- **@nomicfoundation/hardhat-toolbox** - Hardhat 플러그인 모음

---

## 📁 프로젝트 구조

```
frontend/
├── src/
│   ├── abis/                      # 스마트 컨트랙트 ABI
│   │   ├── Lottery.json          # 복권 컨트랙트 ABI
│   │   ├── Usdt.json             # USDT 토큰 컨트랙트 ABI
│   │   └── Vault.json            # Vault 컨트랙트 ABI
│   │
│   ├── app/                       # Next.js App Router
│   │   ├── admin/                # 관리자 페이지
│   │   │   └── page.tsx          # 추첨 실행, 출금 승인, 룰렛 관리
│   │   ├── history/              # 게임 히스토리
│   │   │   └── page.tsx
│   │   ├── room/                 # 게임 룸
│   │   │   └── [roomId]/
│   │   │       └── page.tsx      # 동적 룸 페이지
│   │   ├── layout.tsx            # 루트 레이아웃 (AuthProvider)
│   │   ├── page.tsx              # 메인 페이지
│   │   ├── globals.css           # 전역 스타일
│   │   └── favicon.ico
│   │
│   ├── components/                # 재사용 가능한 컴포넌트
│   │   ├── BettingCompleteMessage.tsx    # 베팅 완료 메시지
│   │   ├── BettingConfirmModal.tsx       # 베팅 확인 모달
│   │   ├── EntrySection.tsx              # 룸 입장 섹션
│   │   ├── FloatingBar.tsx               # 플로팅 바
│   │   ├── GameExplainModal.tsx          # 게임 설명 모달
│   │   ├── GlobalRoundBar.tsx            # 글로벌 라운드 표시
│   │   ├── Header.tsx                    # 헤더 (잔액, 로그인)
│   │   ├── LoginModal.tsx                # 로그인 모달
│   │   ├── MobileBanner.tsx              # 모바일 배너
│   │   ├── MyPageModal.tsx               # 마이페이지 (입출금, 히스토리)
│   │   ├── RouletteHistoryModal.tsx      # 룰렛 히스토리
│   │   ├── RouletteModal.tsx             # 룰렛 게임 모달
│   │   ├── RouletteRoom.tsx              # 룰렛 룸 입장
│   │   ├── RouletteSection.tsx           # 룰렛 섹션
│   │   ├── RoundCanceledMessage.tsx      # 라운드 취소 메시지
│   │   ├── SideMenu.tsx                  # 사이드 메뉴
│   │   ├── SignupModal.tsx               # 회원가입 모달
│   │   ├── SpinningNumbers.tsx           # 숫자 애니메이션
│   │   ├── Timer.tsx                     # 카운트다운 타이머
│   │   ├── WinnerInfoBar.tsx             # 당첨자 정보 바
│   │   └── WinnersModal.tsx              # 당첨자 모달
│   │
│   ├── contexts/                  # React Context
│   │   └── AuthContext.tsx       # 인증 상태 관리
│   │
│   ├── constants/                 # 상수 정의
│   ├── hooks/                     # 커스텀 훅
│   ├── providers/                 # Provider 컴포넌트
│   ├── types/                     # TypeScript 타입 정의
│   └── config.ts                  # 환경 설정
│
├── public/                        # 정적 파일
│   ├── bg.png                    # 배경 이미지
│   └── icon_room_*.webm          # 룸 아이콘 비디오
│
├── .env.local                     # 환경 변수 (gitignored)
├── .gitignore
├── eslint.config.mjs
├── next.config.js
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── tailwind.config.js
└── tsconfig.json
```

---

## 🎮 주요 기능

### 1. 인증 시스템 (AuthContext)
- **JWT 기반 인증**: 로그인 시 JWT 토큰 발급 및 로컬 스토리지 저장
- **자동 로그인**: 페이지 새로고침 시 토큰 검증 후 자동 로그인
- **토큰 만료 처리**: 만료된 토큰 자동 감지 및 로그아웃
- **사용자 상태 관리**: PENDING(지갑 미등록) / ACTIVE(지갑 등록) 상태 관리

```typescript
interface User {
  id: number;
  username: string;
  nickname: string;
  walletAddress: string;
  status: 'PENDING' | 'ACTIVE';
}
```

### 2. 메인 페이지 (/)
- **4개의 게임 룸 표시**: 각 룸의 참가자 수 실시간 업데이트
- **룰렛 게임 입장**: 별도의 룰렛 게임 모드
- **글로벌 라운드 표시**: 현재 진행 중인 글로벌 라운드 번호
- **실시간 당첨자 정보**: 최근 당첨자 정보 스크롤
- **마감 시간 타이머**: 각 게임의 마감 시간 표시

### 3. 게임 룸 (/room/[roomId])
- **실시간 로그 시스템**: 
  - 참가자 입장 로그 (JOIN)
  - 시스템 메시지 (SYSTEM)
  - 당첨자 발표 (WINNER)
  - 프로세스 진행 (PROCESS)
  - 에러 메시지 (ERROR)
  
- **애니메이션 효과**:
  - 추첨 시작 시 5초 카운트다운
  - 로그 순차적 표시 (50ms 간격)
  - 자동 스크롤

- **참가자 관리**:
  - 현재 참가자 목록 실시간 표시
  - 과거 당첨자 히스토리
  - 참가 버튼 (티켓 가격 표시)

### 4. 룰렛 게임 (RouletteModal)
- **HIGH/LOW 베팅**: 1-5 (LOW), 6-10 (HIGH)
- **실시간 풀 정보**: LOW/HIGH 각각의 베팅 총액 표시
- **애니메이션**: 숫자 스피닝 애니메이션
- **무효 라운드 처리**: 한쪽에만 베팅이 있을 경우 환불
- **히스토리**: 과거 라운드 결과 조회

### 5. 마이페이지 (MyPageModal)
- **잔액 관리**:
  - 오프체인 USDT 잔액 표시
  - 입금 (Deposit): 온체인 → 오프체인
  - 출금 (Withdraw): 오프체인 → 온체인 (관리자 승인 필요)

- **지갑 연결**:
  - Web3Modal을 통한 지갑 연결
  - BNB Smart Chain Testnet 지원
  - 지갑 주소 등록 및 관리

- **게임 히스토리**:
  - 참가 내역
  - 당첨 내역
  - 입출금 내역

### 6. 관리자 페이지 (/admin)
- **글로벌 라운드 제어**:
  - 모든 룸 동시 추첨 실행
  - 다음 라운드 오픈

- **룰렛 게임 관리**:
  - 현재 라운드 상태 모니터링
  - 수동 추첨 실행
  - 애니메이션 테스트 (1-10 숫자)
  - 최근 결과 조회

- **출금 요청 관리**:
  - 대기 중인 출금 요청 목록
  - 출금 승인 (온체인 트랜잭션 실행)
  - 트랜잭션 해시 추적

---

## 🔧 환경 설정

### config.ts
```typescript
export const API_BASE_URL = process.env.NEXT_PUBLIC_API_BASE_URL || 'http://localhost:3001';

export const CHAIN_CONFIG = {
    id: process.env.NEXT_PUBLIC_CHAIN_ID || '0x61', // BNB Testnet
    name: process.env.NEXT_PUBLIC_CHAIN_NAME || 'BNB Smart Chain Testnet',
    rpcUrl: process.env.NEXT_PUBLIC_RPC_URL || 'https://data-seed-prebsc-1-s1.binance.org:8545/',
    currency: {
        name: 'Binance Coin',
        symbol: process.env.NEXT_PUBLIC_CURRENCY_SYMBOL || 'tBNB',
        decimals: 18,
    },
    blockExplorerUrl: process.env.NEXT_PUBLIC_BLOCK_EXPLORER_URL || 'https://testnet.bscscan.com',
};

export const VAULT_CONTRACT_ADDRESS = process.env.NEXT_PUBLIC_VAULT_CONTRACT_ADDRESS || "0x43aFfaE1C51B04e772E69EfDd0453B0dC89EC3E6";
```

### 필요한 환경 변수 (.env.local)
```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
NEXT_PUBLIC_CHAIN_ID=0x61
NEXT_PUBLIC_CHAIN_NAME=BNB Smart Chain Testnet
NEXT_PUBLIC_RPC_URL=https://data-seed-prebsc-1-s1.binance.org:8545/
NEXT_PUBLIC_CURRENCY_SYMBOL=tBNB
NEXT_PUBLIC_BLOCK_EXPLORER_URL=https://testnet.bscscan.com
NEXT_PUBLIC_VAULT_CONTRACT_ADDRESS=0x43aFfaE1C51B04e772E69EfDd0453B0dC89EC3E6
```

---

## 🚀 시작하기

### 설치
```bash
npm install
```

### 개발 서버 실행
```bash
npm run dev
```
- 개발 서버는 `http://localhost:3002`에서 실행됩니다.

### 빌드
```bash
npm run build
```

### 프로덕션 실행
```bash
npm start
```

### 린트
```bash
npm run lint
```

---

## 🔄 데이터 흐름

### 1. 게임 참가 플로우
```
사용자 로그인
    ↓
지갑 연결 (선택)
    ↓
USDT 입금 (온체인 → 오프체인)
    ↓
룸 선택 및 참가
    ↓
오프체인 잔액 차감
    ↓
실시간 로그 업데이트
    ↓
마감 시간 도달
    ↓
관리자 추첨 실행
    ↓
당첨자 발표
    ↓
상금 자동 지급 (오프체인)
```

### 2. 룰렛 게임 플로우
```
룰렛 모달 진입
    ↓
HIGH/LOW 선택
    ↓
베팅 금액 입력
    ↓
베팅 확인
    ↓
오프체인 잔액 차감
    ↓
실시간 풀 업데이트
    ↓
마감 시간 도달
    ↓
관리자 추첨 실행
    ↓
숫자 애니메이션 (1-10)
    ↓
당첨자 발표
    ↓
배당금 지급 (2배)
```

### 3. 출금 플로우
```
마이페이지 → 출금 요청
    ↓
출금 금액 입력
    ↓
오프체인 잔액 차감
    ↓
출금 요청 생성 (PENDING)
    ↓
관리자 페이지에서 확인
    ↓
관리자 승인
    ↓
온체인 트랜잭션 실행
    ↓
USDT 전송 (Vault → 사용자)
    ↓
상태 업데이트 (APPROVED)
```

---

## 🎨 UI/UX 특징

### 디자인 시스템
- **다크 테마**: `#1a1a2e` 기본 배경
- **그라디언트 효과**: 
  - 헤더: `from-[#2C32AE] to-[#120D0D]`
  - 텍스트: `from-[#1ACFF8] to-[#EBFD62]`
- **강조 색상**:
  - 주요 액션: `cyan-400`, `yellow-400`
  - 성공: `green-400`
  - 에러: `red-500`
  - 정보: `blue-400`

### 반응형 디자인
- **모바일 우선**: 모바일 화면을 기본으로 설계
- **브레이크포인트**: Tailwind의 `md:` (768px) 사용
- **모바일 배너**: 모바일 사용자를 위한 안내 메시지
- **플렉스 레이아웃**: 화면 크기에 따라 자동 조정

### 애니메이션
- **로딩 스피너**: 트랜잭션 처리 중 표시
- **로그 애니메이션**: 50ms 간격으로 순차 표시
- **숫자 스피닝**: 룰렛 결과 애니메이션
- **카운트다운**: 추첨 시작 5초 전 카운트다운
- **호버 효과**: 버튼 및 인터랙티브 요소

---

## 🔐 보안 고려사항

### 1. 인증
- JWT 토큰을 로컬 스토리지에 저장
- 모든 API 요청에 Bearer 토큰 포함
- 토큰 만료 시 자동 로그아웃

### 2. 트랜잭션
- 오프체인 잔액으로 빠른 게임 진행
- 입출금만 온체인 트랜잭션 사용
- 관리자 승인 필요한 출금 시스템

### 3. 스마트 컨트랙트
- Hardhat으로 개발 및 테스트
- ABI 파일을 통한 타입 안전성
- BNB Smart Chain Testnet 사용

---

## 📊 API 엔드포인트

### 인증
- `POST /auth/login` - 로그인
- `POST /auth/signup` - 회원가입

### 게임
- `GET /lottery-status/:roomId` - 룸 상태 조회
- `POST /participate/:roomId` - 게임 참가
- `GET /winners/:roomId` - 당첨자 조회
- `GET /rooms/status` - 모든 룸 상태

### 룰렛
- `GET /api/roulette/current` - 현재 라운드
- `POST /api/roulette/bet` - 베팅
- `POST /api/roulette/draw` - 추첨 (관리자)
- `GET /api/roulette/result` - 최근 결과
- `GET /api/roulette/history` - 히스토리

### 잔액
- `GET /balance/:address` - 잔액 조회
- `POST /deposit` - 입금
- `POST /withdraw` - 출금 요청

### 관리자
- `POST /start-draw-all` - 모든 룸 추첨
- `POST /admin/open-next-round` - 다음 라운드 오픈
- `GET /admin/withdrawals` - 출금 요청 목록
- `POST /admin/withdrawals/approve` - 출금 승인

---

## 🐛 알려진 이슈 및 개선 사항

### 현재 이슈
1. 출금 시 관리자 수동 승인 필요 (자동화 가능)
2. 모바일에서 지갑 연결 시 일부 지갑 호환성 문제
3. 실시간 업데이트를 위한 폴링 방식 (WebSocket으로 개선 가능)

### 개선 계획
1. **WebSocket 도입**: 실시간 업데이트를 위한 양방향 통신
2. **자동 출금**: 일정 금액 이하 자동 승인
3. **다국어 지원**: i18n 라이브러리 도입
4. **알림 시스템**: 당첨, 입출금 완료 시 알림
5. **통계 대시보드**: 사용자별 게임 통계
6. **소셜 기능**: 친구 초대, 리더보드

---

## 📝 개발 가이드

### 새로운 컴포넌트 추가
1. `src/components/` 폴더에 `.tsx` 파일 생성
2. TypeScript 인터페이스 정의
3. Tailwind CSS로 스타일링
4. 필요시 Context API 사용

### 새로운 페이지 추가
1. `src/app/` 폴더에 디렉토리 생성
2. `page.tsx` 파일 생성
3. 메타데이터 설정
4. 라우팅 자동 생성

### API 연동
1. `config.ts`에서 `API_BASE_URL` 사용
2. `fetch` 또는 `@tanstack/react-query` 사용
3. 에러 핸들링 필수
4. 로딩 상태 관리

### 스마트 컨트랙트 연동
1. ABI 파일을 `src/abis/` 폴더에 추가
2. `wagmi` 또는 `ethers.js` 사용
3. 트랜잭션 상태 관리
4. 에러 메시지 사용자 친화적으로 표시

---

## 🤝 기여 가이드

### 코드 스타일
- ESLint 규칙 준수
- TypeScript 타입 명시
- 컴포넌트 주석 작성
- 함수명은 camelCase, 컴포넌트명은 PascalCase

### 커밋 메시지
```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 수정
style: 코드 포맷팅
refactor: 코드 리팩토링
test: 테스트 추가
chore: 빌드 설정 등
```

### Pull Request
1. 기능 브랜치 생성
2. 변경사항 커밋
3. PR 생성 및 설명 작성
4. 코드 리뷰 대기
5. 승인 후 머지

---

## 📞 문의 및 지원

프로젝트 관련 문의사항이나 버그 리포트는 GitHub Issues를 통해 제출해주세요.

---

## 📄 라이선스

이 프로젝트는 개인 프로젝트이며, 상업적 사용 시 별도 협의가 필요합니다.

---

**Last Updated**: 2025-10-13  
**Version**: 0.1.0  
**Developer**: Windsurf Team
