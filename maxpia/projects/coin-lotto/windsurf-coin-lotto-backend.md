# Windsurf Coin Lotto Backend - 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Coin Lotto Backend  
**프로젝트 유형**: 블록체인 기반 복권/룰렛 게임 백엔드 서버  
**블록체인**: BNB Testnet  
**주요 기술 스택**: Node.js, Express, SQLite, Ethers.js, Solidity

## 🎯 프로젝트 목적

USDT(Tether) 기반의 온라인 복권 및 룰렛 게임 플랫폼의 백엔드 시스템입니다. 사용자는 USDT를 입금하여 다양한 게임에 참여하고, 당첨 시 보상을 받을 수 있습니다.

## 🏗️ 시스템 아키텍처

### 1. 기술 스택

#### Backend
- **Runtime**: Node.js
- **Framework**: Express.js v5.1.0
- **Database**: SQLite3 v5.1.7
- **Blockchain**: Ethers.js v6.15.0

#### Security & Authentication
- **Password Hashing**: bcrypt v6.0.0
- **JWT Authentication**: jsonwebtoken v9.0.2
- **Environment Variables**: dotenv v17.2.0

#### Smart Contract
- **Language**: Solidity ^0.8.20
- **Framework**: Hardhat
- **Standards**: OpenZeppelin Contracts (ERC20, Ownable)

### 2. 프로젝트 구조

```
backend/
├── server.js                 # 메인 서버 파일 (1674 lines)
├── rooms.js                  # 게임 룸 설정
├── lottery.db                # SQLite 데이터베이스
├── package.json              # 의존성 관리
├── .env                      # 환경 변수 (RPC URL, Private Key, JWT Secret)
├── smart-contract/           # 스마트 컨트랙트 디렉토리
│   ├── contracts/
│   │   └── Vault.sol        # USDT 보관 컨트랙트
│   ├── artifacts/           # 컴파일된 컨트랙트
│   └── hardhat.config.ts    # Hardhat 설정
├── config/                   # 설정 파일 (비어있음)
├── routes/                   # 라우트 파일 (비어있음)
└── utils/                    # 유틸리티 함수 (비어있음)
```

## 🎮 게임 시스템

### 1. USDT 복권 게임

#### 게임 룸 구성
프로젝트는 4개의 티켓 가격대별 룸을 제공합니다:

| Room ID | 룸 이름 | 티켓 가격 |
|---------|---------|-----------|
| 1 | 1 usdt | 1 USDT |
| 2 | 10 usdt | 10 USDT |
| 3 | 50 usdt | 50 USDT |
| 4 | 100 usdt | 100 USDT |

#### 게임 플로우
1. **글로벌 라운드 시작**: 1시간 단위로 새로운 글로벌 라운드 생성
2. **참가자 모집**: 사용자들이 원하는 룸에 티켓 구매로 참여
3. **자동 추첨**: 
   - 마감 시간 도달 시 자동으로 추첨 시작
   - 각 룸별로 랜덤하게 당첨자 선정
   - 100회의 스핀 애니메이션 로그 생성
4. **당첨금 지급**: 
   - 상금 풀의 98%를 당첨자에게 지급 (2% 수수료)
   - 사용자 잔액에 자동 입금
5. **라운드 종료**: 7초 후 DRAWING → CLOSED 상태로 전환
6. **다음 라운드**: 모든 룸이 종료되면 자동으로 새 글로벌 라운드 시작

### 2. 룰렛 게임

#### 게임 규칙
- **베팅 옵션**: LOW (1-5) 또는 HIGH (6-10)
- **라운드 시간**: 5분 5초
- **당첨 번호**: 1-10 중 랜덤 선택
- **배당 방식**: 
  - 승자 풀의 베팅 비율에 따라 패자 풀 분배
  - 5% 하우스 수수료 적용
  - 개별 베팅 비율 = (개인 베팅액 / 승자 풀 총액)
  - 개인 배당금 = 베팅액 + (패자 풀 × 95% × 개별 베팅 비율)

#### 룰렛 특징
- **독립 스케줄러**: USDT 게임과 완전 분리된 30초 간격 스케줄러
- **자동 추첨**: 5분 5초 경과 시 자동으로 추첨 실행
- **무효 라운드 처리**: LOW/HIGH 한쪽만 베팅 시 전액 환불
- **숫자 시퀀스**: 매 라운드마다 1-10 숫자를 랜덤 셔플하여 저장
- **애니메이션 테스트**: 관리자용 테스트 API 제공

## 🔐 스마트 컨트랙트

### Vault.sol

**배포 주소**: `0x43aFfaE1C51B04e772E69EfDd0453B0dC89EC3E6` (BNB Testnet)

#### 주요 기능

1. **deposit(uint256 _amount)**
   - 사용자가 USDT를 Vault에 입금
   - 사전에 approve 필요
   - Deposit 이벤트 발생

2. **withdraw(address _recipient, uint256 _amount)** (onlyOwner)
   - 관리자만 출금 가능
   - 출금 승인 시 사용자에게 USDT 전송
   - Withdrawal 이벤트 발생

3. **getBalance()**
   - Vault의 현재 USDT 잔액 조회

#### 보안 특징
- OpenZeppelin의 Ownable 패턴 사용
- 입출금 금액 검증
- 잔액 부족 시 revert
- 이벤트 로깅으로 투명성 확보

## 💾 데이터베이스 스키마

### 주요 테이블

#### 1. users (사용자 인증)
```sql
- id: INTEGER PRIMARY KEY
- username: TEXT UNIQUE (로그인 ID)
- password: TEXT (bcrypt 해시)
- nickname: TEXT (표시 이름)
- wallet_address: TEXT UNIQUE (지갑 주소)
- status: TEXT (PENDING/ACTIVE)
- created_at: DATETIME
```

#### 2. user_balances (잔액 관리)
```sql
- user_address: TEXT PRIMARY KEY
- balance: REAL (USDT 잔액)
```

#### 3. transactions (거래 내역)
```sql
- id: INTEGER PRIMARY KEY
- user_address: TEXT
- type: TEXT (DEPOSIT/WITHDRAWAL/PLAY/WIN/ROULETTE_BET/ROULETTE_WIN/ROULETTE_REFUND)
- amount: REAL
- related_round_id: INTEGER
- tx_hash: TEXT (온체인 트랜잭션 해시)
- timestamp: DATETIME
```

#### 4. global_rounds (글로벌 라운드)
```sql
- id: INTEGER PRIMARY KEY
- status: TEXT (OPEN/CLOSED)
- createdAt: DATETIME
- endedAt: DATETIME
- deadline: DATETIME (1시간 후)
```

#### 5. room_rounds (룸별 라운드)
```sql
- id: INTEGER PRIMARY KEY
- global_round_id: INTEGER
- room_id: TEXT
- status: TEXT (OPEN/DRAWING/CLOSED)
- players_snapshot: TEXT (JSON)
- winner: TEXT (당첨자 주소)
- prizePool: TEXT
- drawAt: DATETIME
```

#### 6. participations (참가 기록)
```sql
- id: INTEGER PRIMARY KEY
- round_id: INTEGER
- user_address: TEXT
- createdAt: DATETIME
```

#### 7. logs (게임 로그)
```sql
- id: INTEGER PRIMARY KEY
- round_id: INTEGER
- type: TEXT (SYSTEM/JOIN/SPIN/WINNER)
- message: TEXT
- timestamp: DATETIME
```

#### 8. withdrawals (출금 요청)
```sql
- id: INTEGER PRIMARY KEY
- user_address: TEXT
- amount: REAL
- status: TEXT (PENDING/APPROVED/REJECTED)
- requested_at: DATETIME
- processed_at: DATETIME
- tx_hash: TEXT
```

#### 9. roulette_rounds (룰렛 라운드)
```sql
- id: INTEGER PRIMARY KEY
- round_number: INTEGER UNIQUE
- winning_number: INTEGER (1-10)
- winning_type: TEXT (LOW/HIGH/INVALID)
- number_sequence: TEXT (JSON 배열)
- total_low_amount: DECIMAL
- total_high_amount: DECIMAL
- status: TEXT (betting/drawing/completed/invalid)
- start_time: DATETIME
- end_time: DATETIME
- created_at: DATETIME
```

#### 10. roulette_bets (룰렛 베팅)
```sql
- id: INTEGER PRIMARY KEY
- user_address: TEXT
- round_id: INTEGER
- bet_type: TEXT (LOW/HIGH)
- bet_amount: DECIMAL
- is_winner: BOOLEAN
- payout_amount: DECIMAL
- created_at: DATETIME
```

## 🔌 API 엔드포인트

### 인증 API

#### POST /auth/register
사용자 회원가입
```json
Request: {
  "username": "user123",
  "password": "password",
  "nickname": "닉네임",
  "wallet_address": "0x..."
}
Response: {
  "message": "User registered successfully",
  "userId": 1
}
```

#### POST /auth/login
사용자 로그인 (JWT 토큰 발급)
```json
Request: {
  "username": "user123",
  "password": "password"
}
Response: {
  "message": "Login successful",
  "token": "eyJhbGc...",
  "user": {
    "id": 1,
    "username": "user123",
    "nickname": "닉네임",
    "walletAddress": "0x...",
    "status": "ACTIVE"
  }
}
```

### 잔액 관리 API

#### GET /balance/:address
사용자 잔액 조회
```json
Response: {
  "balance": 100.5
}
```

#### POST /users/register
사용자 잔액 계정 생성 (레거시)

### USDT 복권 게임 API

#### GET /rooms
게임 룸 목록 조회
```json
Response: [
  {
    "id": "1",
    "name": "1 usdt",
    "ticketPrice": "1"
  },
  ...
]
```

#### GET /rooms/status
룸별 참가자 수 조회
```json
Response: [
  {
    "id": "1",
    "name": "1 usdt",
    "participantCount": 5
  },
  ...
]
```

#### GET /lottery-status/:roomId
특정 룸의 현재 상태 조회
```json
Response: {
  "id": 1,
  "global_round_id": 5,
  "room_id": "1",
  "status": "OPEN",
  "winner": null,
  "prizePool": "10.0000",
  "deadline": "2025-10-13T12:00:00.000Z",
  "players": ["0x123...", "0x456..."],
  "logs": [...],
  "ticketPrice": "1",
  "name": "1 usdt"
}
```

#### POST /participate/:roomId (인증 필요)
게임 참가
```json
Request: {
  // JWT 토큰에서 자동으로 사용자 정보 추출
}
Response: {
  "message": "Participation successful",
  "newBalance": 99.0
}
```

#### POST /start-draw-all
모든 룸 추첨 시작 (수동 트리거)

#### POST /admin/open-next-round
다음 글로벌 라운드 시작 (관리자)

### 룰렛 게임 API

#### GET /api/roulette/current
현재 룰렛 라운드 정보
```json
Response: {
  "round": {
    "id": 10,
    "round_number": 10,
    "status": "betting",
    "end_time": "2025-10-13T11:10:00.000Z",
    "number_sequence": "[3,7,1,9,2,8,4,10,5,6]",
    "total_low_amount": "50.00",
    "total_high_amount": "30.00"
  },
  "timeRemaining": 245,
  "deadline": "2025-10-13T11:10:00.000Z",
  "isRecentInvalid": false
}
```

#### POST /api/roulette/bet (인증 필요)
룰렛 베팅
```json
Request: {
  "bet_type": "LOW",  // or "HIGH"
  "amount": 10
}
Response: {
  "success": true,
  "message": "Bet placed successfully",
  "newBalance": 90,
  "bet": {
    "type": "LOW",
    "amount": 10,
    "round": 10
  }
}
```

#### GET /api/roulette/user-bets (인증 필요)
사용자의 룰렛 베팅 내역 (최근 50개)

#### GET /api/roulette/pool-status
현재 베팅 풀 상태
```json
Response: {
  "round_number": 10,
  "low_pool": "50.00",
  "high_pool": "30.00",
  "total_pool": 80,
  "end_time": "2025-10-13T11:10:00.000Z",
  "number_sequence": [3,7,1,9,2,8,4,10,5,6]
}
```

#### POST /api/roulette/draw
룰렛 추첨 실행 (테스트용)

#### GET /api/roulette/result
최신 룰렛 결과 조회

#### GET /api/roulette/result/:roundId
특정 라운드 결과 조회

#### GET /api/roulette/current-bets
현재 라운드 베팅 현황 (플로팅 바용)
```json
Response: {
  "round_number": 10,
  "round_id": 10,
  "low_bets": [...],
  "high_bets": [...],
  "total_low": 50,
  "total_high": 30
}
```

#### GET /api/roulette/history
룰렛 히스토리 (페이지네이션)
```
Query: ?page=0&limit=20
```

#### POST /api/roulette/test-animation (관리자)
룰렛 애니메이션 테스트
```json
Request: {
  "winningNumber": 7
}
```

#### GET /api/roulette/test-status
테스트 상태 확인

#### DELETE /api/roulette/test-status
테스트 중지

### 사용자 히스토리 API

#### GET /user-history/:address
사용자 게임 참가 내역

#### GET /transactions/:address
사용자 거래 내역

#### GET /winners/:roomId
특정 룸의 당첨자 목록

#### GET /winner-bar-data
전체 당첨자 데이터 (차트용)

### 출금 관리 API

#### POST /withdrawals/request
출금 요청
```json
Request: {
  "userAddress": "0x...",
  "amount": 50
}
Response: {
  "message": "Withdrawal request submitted successfully",
  "newBalance": 50
}
```

#### GET /admin/withdrawals (관리자)
모든 출금 요청 조회

#### POST /admin/withdrawals/approve (관리자)
출금 승인 및 온체인 처리
```json
Request: {
  "withdrawalId": 1
}
Response: {
  "message": "Withdrawal approved and processed",
  "txHash": "0x..."
}
```

## 🔄 자동화 시스템

### 1. USDT 복권 자동화

#### 자동 추첨 (autoStartDrawAll)
- 글로벌 라운드 마감 시간 도달 시 자동 실행
- 각 룸별로 참가자가 1명 이상이면 추첨 진행
- 100회 스핀 애니메이션 로그 생성
- 당첨자 선정 및 98% 배당금 지급
- 7초 후 자동으로 CLOSED 상태로 전환

#### 자동 라운드 생성 (autoOpenNextRound)
- 모든 룸이 CLOSED 상태가 되면 자동 실행
- 현재 글로벌 라운드를 CLOSED로 변경
- 새로운 글로벌 라운드 생성 (1시간 마감)
- 각 룸별 room_rounds 생성

### 2. 룰렛 자동화

#### 룰렛 스케줄러 (startRouletteScheduler)
- 30초마다 실행되는 독립 스케줄러
- 만료된 라운드 자동 추첨
- 5초 이내 만료 예정 시 알림

#### 자동 라운드 관리 (getCurrentRouletteRound)
- 만료된 라운드 자동 추첨 처리
- 무효 라운드 감지 및 환불 처리
- 활성 라운드 없을 시 자동 생성
- 무효 라운드 후 3초 대기 시간 적용

#### 추첨 후 처리 (drawRouletteRound)
- 1초 후 라운드를 completed로 변경
- 자동으로 새 라운드 생성

### 3. 블록체인 이벤트 리스너

#### USDT Transfer 이벤트
```javascript
usdtContract.on("Transfer", async (from, to, value, event) => {
  // Vault 컨트랙트로의 전송만 처리
  // 사용자 잔액 자동 업데이트
  // PENDING 상태 사용자 자동 ACTIVE 전환
  // 거래 내역 기록
});
```

## 🔒 보안 기능

### 1. 인증 및 권한
- **JWT 토큰**: 1일 유효기간
- **bcrypt 해싱**: 비밀번호 암호화 (salt rounds: 10)
- **authenticateToken 미들웨어**: 보호된 엔드포인트 접근 제어

### 2. 데이터 무결성
- **트랜잭션**: 모든 금액 관련 작업에 BEGIN/COMMIT/ROLLBACK 사용
- **Foreign Keys**: PRAGMA foreign_keys = ON
- **인덱스**: 성능 최적화 및 데이터 무결성

### 3. 입력 검증
- 금액 양수 검증
- 잔액 부족 검증
- 베팅 타입 검증 (LOW/HIGH)
- 라운드 상태 검증

## 📊 주요 비즈니스 로직

### 1. 수수료 구조

#### USDT 복권
- **하우스 수수료**: 2%
- **당첨자 배당**: 상금 풀의 98%

#### 룰렛
- **하우스 수수료**: 5%
- **승자 배당**: 패자 풀의 95%를 베팅 비율에 따라 분배

### 2. 당첨 확률
- **USDT 복권**: 1 / 참가자 수 (균등 확률)
- **룰렛**: 베팅 타입에 따라 50% (LOW: 1-5, HIGH: 6-10)

### 3. 무효 처리
- **USDT 복권**: 참가자 1명 미만 시 라운드 스킵
- **룰렛**: LOW/HIGH 한쪽만 베팅 시 전액 환불

## 🚀 서버 시작 프로세스

1. **환경 변수 로드**: dotenv로 .env 파일 읽기
2. **데이터베이스 연결**: SQLite 데이터베이스 오픈
3. **스키마 초기화**: 테이블 생성 및 인덱스 생성
4. **글로벌 라운드 확인**: 활성 라운드 없으면 생성
5. **룰렛 라운드 확인**: 활성 라운드 없으면 생성
6. **룰렛 스케줄러 시작**: 30초 간격 자동화
7. **블록체인 리스너 시작**: USDT Transfer 이벤트 감지
8. **서버 리스닝**: 포트 3001에서 대기

## 🌐 블록체인 설정

### 네트워크
- **체인**: BNB Testnet
- **RPC URL**: https://data-seed-prebsc-1-s1.binance.org:8545/

### 컨트랙트 주소
- **Vault**: 0x43aFfaE1C51B04e772E69EfDd0453B0dC89EC3E6
- **USDT**: 0x337610d27c682E347C9cD60BD4b3b107C9d34dDd

### 지갑
- **Private Key**: 환경 변수로 관리
- **역할**: Vault 컨트랙트 Owner (출금 권한)

## 📈 확장 가능성

### 현재 구조의 장점
1. **모듈화**: 게임 로직이 분리되어 있어 새로운 게임 추가 용이
2. **독립 스케줄러**: USDT 복권과 룰렛이 독립적으로 운영
3. **오프체인 처리**: 빠른 게임 진행 및 낮은 가스비
4. **이벤트 기반**: 블록체인 이벤트로 입금 자동 감지

### 개선 가능 영역
1. **routes/, utils/, config/ 디렉토리 활용**: 현재 비어있음
2. **관리자 인증**: 현재 관리자 API에 인증 없음
3. **에러 핸들링**: 더 세밀한 에러 처리 필요
4. **로깅 시스템**: 파일 기반 로깅 추가 고려
5. **수수료 추적**: fee_logs 테이블 추가 예정 (TODO 주석 있음)
6. **WebSocket**: 실시간 게임 상태 업데이트

## 🛠️ 개발 환경

### 필수 요구사항
- Node.js (v14 이상 권장)
- SQLite3
- BNB Testnet 지갑 (테스트용 BNB 필요)

### 환경 변수 (.env)
```
BNB_TESTNET_RPC_URL=https://data-seed-prebsc-1-s1.binance.org:8545/
PRIVATE_KEY=your_private_key_here
JWT_SECRET=your_jwt_secret_here
```

### 서버 실행
```bash
npm install
node server.js
```

### 포트
- **Backend Server**: 3001

## 📝 주요 파일 설명

### server.js (1674 lines)
프로젝트의 핵심 파일로 모든 로직이 포함되어 있습니다:
- 라인 1-50: 초기 설정 및 미들웨어
- 라인 51-190: 글로벌 라운드 관리 함수
- 라인 191-479: 룰렛 게임 로직
- 라인 481-610: 데이터베이스 초기화
- 라인 611-693: 블록체인 이벤트 리스너
- 라인 694-1274: API 엔드포인트 (인증, 잔액, 복권)
- 라인 1275-1670: 룰렛 API 엔드포인트
- 라인 1671-1674: 서버 시작

### rooms.js (31 lines)
게임 룸 설정 파일:
- 4개 룸 정의 (1, 10, 50, 100 USDT)
- initializeRoomsWithoutContracts() 함수

### Vault.sol (70 lines)
USDT 보관 스마트 컨트랙트:
- deposit(): 사용자 입금
- withdraw(): 관리자 출금
- getBalance(): 잔액 조회

## 🎯 프로젝트 특징

### 강점
1. **완전한 오프체인 게임 로직**: 빠르고 저렴한 게임 진행
2. **자동화된 라운드 관리**: 수동 개입 최소화
3. **투명한 거래 내역**: 모든 거래 데이터베이스 기록
4. **다양한 게임**: 복권과 룰렛 동시 운영
5. **실시간 이벤트 감지**: 블록체인 입금 자동 반영
6. **보안**: JWT 인증, bcrypt 해싱, 트랜잭션 관리

### 주의사항
1. **중앙화된 구조**: 백엔드 서버가 게임 결과 결정
2. **테스트넷**: 실제 자금 사용 불가
3. **Private Key 노출**: .env 파일 보안 관리 필수
4. **SQLite 한계**: 대규모 트래픽 시 PostgreSQL/MySQL 고려

## 📚 참고 정보

### 의존성 버전
- express: ^5.1.0
- ethers: ^6.15.0
- sqlite3: ^5.1.7
- bcrypt: ^6.0.0
- jsonwebtoken: ^9.0.2
- cors: ^2.8.5
- dotenv: ^17.2.0

### 데이터베이스 크기
- lottery.db: 479,232 bytes (약 468 KB)
- lottery.db.bak: 184,320 bytes (백업)

---

**문서 작성일**: 2025-10-13  
**프로젝트 버전**: 1.0.0  
**작성자**: Windsurf AI  
**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/01-blockchain-projects/maxpia-project/coin_lotto/backend`
