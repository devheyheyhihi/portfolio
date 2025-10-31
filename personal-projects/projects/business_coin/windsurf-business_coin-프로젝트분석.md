# Windsurf - Business Coin 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: Business Coin  
**설명**: 자체 P2P 네트워크를 갖춘 블록체인 기반 암호화폐 구현  
**버전**: 1.0.0  
**라이선스**: MIT

---

## 🎯 프로젝트 목적

Business Coin은 블록체인의 핵심 개념을 학습하고 구현하기 위한 교육용 암호화폐 프로젝트입니다. 실제 암호화폐의 주요 기능들을 간소화하여 구현하였으며, P2P 네트워크를 통한 분산 시스템의 동작 원리를 이해할 수 있습니다.

---

## 🏗️ 시스템 아키텍처

### 전체 구조
```
┌─────────────────────────────────────────────────────────┐
│                    Business Coin Node                    │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  API Server  │  │  Blockchain  │  │  P2P Server  │  │
│  │  (Express)   │  │    Core      │  │ (WebSocket)  │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                  │                  │          │
│         └──────────────────┴──────────────────┘          │
│                            │                             │
│                    ┌───────┴────────┐                    │
│                    │  Key Generator │                    │
│                    │   (ECC Wallet) │                    │
│                    └────────────────┘                    │
└─────────────────────────────────────────────────────────┘
                            │
                            │ WebSocket
                            ▼
              ┌─────────────────────────┐
              │   Other Blockchain      │
              │        Nodes            │
              └─────────────────────────┘
```

---

## 📁 프로젝트 구조

```
business_coin/
├── src/
│   ├── index.js          # 애플리케이션 진입점
│   ├── blockchain.js     # 블록체인 코어 로직
│   ├── api.js           # HTTP API 서버
│   ├── p2p.js           # P2P 네트워크 서버
│   └── keygenerator.js  # 지갑 키 생성 및 관리
├── wallets/             # 지갑 파일 저장 디렉토리
│   └── wallet.json      # 기본 지갑
├── package.json         # 프로젝트 의존성 및 설정
└── README.md           # 프로젝트 문서
```

---

## 🔧 핵심 컴포넌트 분석

### 1. Blockchain Core (`blockchain.js`)

#### 1.1 Transaction 클래스
```javascript
class Transaction {
  - fromAddress: 발신자 주소 (공개키)
  - toAddress: 수신자 주소 (공개키)
  - amount: 전송 금액
  - timestamp: 트랜잭션 생성 시간
  - signature: 디지털 서명
}
```

**주요 기능**:
- `calculateHash()`: SHA256 해시 계산
- `signTransaction(signingKey)`: ECC 기반 디지털 서명
- `isValid()`: 서명 검증

#### 1.2 Block 클래스
```javascript
class Block {
  - timestamp: 블록 생성 시간
  - transactions: 트랜잭션 배열
  - previousHash: 이전 블록 해시
  - hash: 현재 블록 해시
  - nonce: 작업 증명을 위한 난스 값
}
```

**주요 기능**:
- `calculateHash()`: 블록 해시 계산
- `mineBlock(difficulty)`: 작업 증명(PoW) 채굴
- `hasValidTransactions()`: 블록 내 트랜잭션 검증

#### 1.3 Blockchain 클래스
```javascript
class Blockchain {
  - chain: 블록 체인 배열
  - difficulty: 채굴 난이도 (기본값: 2)
  - pendingTransactions: 대기 중인 트랜잭션
  - miningReward: 채굴 보상 (기본값: 100)
}
```

**주요 기능**:
- `createGenesisBlock()`: 제네시스 블록 생성
- `minePendingTransactions()`: 대기 트랜잭션 채굴
- `addTransaction()`: 트랜잭션 추가 및 검증
- `getBalanceOfAddress()`: 주소 잔액 조회
- `isChainValid()`: 블록체인 무결성 검증

---

### 2. API Server (`api.js`)

Express 기반 RESTful API 서버로 사용자와 블록체인 간 인터페이스 제공

#### API 엔드포인트

| 메서드 | 경로 | 설명 |
|--------|------|------|
| GET | `/blockchain` | 전체 블록체인 정보 조회 |
| POST | `/transaction` | 새로운 트랜잭션 생성 |
| POST | `/mine` | 블록 채굴 |
| GET | `/balance/:address` | 지갑 잔액 조회 |
| POST | `/wallet` | 새 지갑 생성 |
| GET | `/wallet/:name?` | 지갑 정보 조회 |
| GET | `/peers` | 연결된 피어 목록 조회 |
| POST | `/peers` | 새 피어 연결 |

#### 주요 특징
- CORS 지원으로 크로스 오리진 요청 허용
- Body-parser를 통한 JSON 요청 처리
- 트랜잭션 서명 검증
- P2P 서버와 통합된 동작

---

### 3. P2P Network (`p2p.js`)

WebSocket 기반 분산 네트워크 구현

#### 메시지 타입
```javascript
MESSAGE_TYPE = {
  QUERY_LATEST: 최신 블록 요청
  QUERY_ALL: 전체 블록체인 요청
  RESPONSE_BLOCKCHAIN: 블록체인 응답
  QUERY_TRANSACTION_POOL: 트랜잭션 풀 요청
  RESPONSE_TRANSACTION_POOL: 트랜잭션 풀 응답
  NEW_TRANSACTION: 새 트랜잭션 브로드캐스트
}
```

#### 주요 기능
- **피어 연결 관리**: 동적 피어 추가/제거
- **블록체인 동기화**: 최장 체인 규칙(Longest Chain Rule) 적용
- **트랜잭션 전파**: 새 트랜잭션을 네트워크에 브로드캐스트
- **합의 메커니즘**: 체인 길이 기반 합의

#### 동작 원리
1. 노드 시작 시 환경변수 `PEERS`에서 피어 목록 읽기
2. WebSocket 연결 수립
3. 최신 블록 및 트랜잭션 풀 동기화
4. 새로운 블록/트랜잭션 발생 시 모든 피어에게 브로드캐스트

---

### 4. Key Generator (`keygenerator.js`)

타원 곡선 암호화(ECC) 기반 지갑 관리

#### 사용 암호화 알고리즘
- **곡선**: secp256k1 (비트코인과 동일)
- **라이브러리**: elliptic.js

#### 주요 기능
- `generateKeyPair()`: 공개키/개인키 쌍 생성
- `saveKeyPair()`: 지갑을 JSON 파일로 저장
- `loadKeyPair()`: 저장된 지갑 불러오기
- `getKeysFromWallet()`: EC 키 객체 반환

#### 지갑 파일 구조
```json
{
  "privateKey": "hex 형식의 개인키",
  "publicKey": "hex 형식의 공개키 (주소로 사용)"
}
```

---

## 🔐 보안 메커니즘

### 1. 디지털 서명
- **알고리즘**: ECDSA (Elliptic Curve Digital Signature Algorithm)
- **목적**: 트랜잭션 위조 방지
- **검증**: 공개키로 서명 검증

### 2. 작업 증명 (Proof of Work)
- **난이도**: 해시 앞자리 0의 개수로 조절
- **목적**: 블록 생성 비용 증가로 악의적 공격 방지
- **현재 난이도**: 2 (개발/테스트용)

### 3. 체인 무결성 검증
- 각 블록의 해시 검증
- 이전 블록 해시 연결 검증
- 모든 트랜잭션 서명 검증

---

## 🛠️ 기술 스택

### 런타임 & 프레임워크
- **Node.js**: JavaScript 런타임
- **Express.js**: HTTP API 서버

### 핵심 라이브러리
| 라이브러리 | 버전 | 용도 |
|-----------|------|------|
| crypto-js | ^4.1.1 | SHA256 해싱 |
| elliptic | ^6.5.4 | ECC 암호화 |
| ws | ^8.14.2 | WebSocket 통신 |
| uuid | ^9.0.1 | 노드 ID 생성 |
| body-parser | ^1.20.2 | HTTP 요청 파싱 |

### 개발 도구
- **nodemon**: 개발 중 자동 재시작

---

## 🚀 실행 방법

### 단일 노드 실행
```bash
npm install
npm start
```
- HTTP API: http://localhost:3000
- P2P 포트: 6001

### 다중 노드 네트워크 구성
```bash
# 첫 번째 노드
npm start

# 두 번째 노드 (다른 터미널)
PEERS=ws://localhost:6001 HTTP_PORT=3001 P2P_PORT=6002 npm start

# 세 번째 노드 (다른 터미널)
PEERS=ws://localhost:6001,ws://localhost:6002 HTTP_PORT=3002 P2P_PORT=6003 npm start
```

---

## 💡 사용 예시

### 1. 지갑 생성
```bash
curl -X POST http://localhost:3000/wallet
```

**응답 예시**:
```json
{
  "message": "지갑이 생성되었습니다",
  "address": "04a1b2c3...",
  "privateKey": "d4e5f6...",
  "walletPath": "/path/to/wallets/wallet.json"
}
```

### 2. 트랜잭션 생성
```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "fromAddress": "04a1b2c3...",
  "toAddress": "04d4e5f6...",
  "amount": 50,
  "privateKey": "d4e5f6..."
}' http://localhost:3000/transaction
```

### 3. 블록 채굴
```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "rewardAddress": "04a1b2c3..."
}' http://localhost:3000/mine
```

### 4. 잔액 조회
```bash
curl http://localhost:3000/balance/04a1b2c3...
```

### 5. 블록체인 조회
```bash
curl http://localhost:3000/blockchain
```

---

## 🔄 트랜잭션 생명주기

```
1. 사용자가 트랜잭션 생성
   ↓
2. 개인키로 트랜잭션 서명
   ↓
3. API 서버가 서명 검증
   ↓
4. pendingTransactions에 추가
   ↓
5. P2P 네트워크로 브로드캐스트
   ↓
6. 다른 노드들도 트랜잭션 수신 및 검증
   ↓
7. 채굴자가 블록 채굴 시작
   ↓
8. 작업 증명 완료 (nonce 찾기)
   ↓
9. 새 블록이 체인에 추가
   ↓
10. 블록이 네트워크에 브로드캐스트
   ↓
11. 다른 노드들이 블록 검증 및 체인 동기화
```

---

## 🌐 P2P 네트워크 동작

### 노드 연결 과정
```
Node A (6001)  ←→  Node B (6002)  ←→  Node C (6003)
     ↓                                      ↑
     └──────────────────────────────────────┘
```

### 블록 전파 과정
1. Node A가 블록 채굴
2. Node A → Node B, Node C로 브로드캐스트
3. Node B, C가 블록 검증
4. 검증 성공 시 자신의 체인에 추가
5. 체인 길이 비교 (최장 체인 선택)

---

## ⚠️ 현재 제한사항 및 개선 방향

### 제한사항
1. **보안**
   - 개인키가 API 요청에 평문으로 전송됨
   - HTTPS 미지원
   - 지갑 파일이 암호화되지 않음

2. **확장성**
   - 메모리 기반 저장 (재시작 시 데이터 손실)
   - 데이터베이스 미사용
   - 대용량 트랜잭션 처리 미흡

3. **합의 메커니즘**
   - 단순 최장 체인 규칙 (포크 해결 미흡)
   - 동시 채굴 시 충돌 가능성

4. **네트워크**
   - NAT 통과 미지원
   - 피어 발견 메커니즘 부재
   - 네트워크 분할 복구 미흡

### 개선 방향
1. **데이터 영속성**
   - LevelDB 또는 MongoDB 통합
   - 블록체인 데이터 파일 저장

2. **보안 강화**
   - HTTPS 지원
   - 지갑 암호화
   - JWT 기반 인증

3. **성능 최적화**
   - UTXO 모델 도입
   - 머클 트리 구현
   - 트랜잭션 인덱싱

4. **네트워크 개선**
   - DHT 기반 피어 발견
   - NAT 통과 (STUN/TURN)
   - 네트워크 파티션 감지 및 복구

5. **사용자 인터페이스**
   - 웹 기반 지갑 UI
   - 블록 탐색기
   - 실시간 네트워크 모니터링

---

## 📊 성능 특성

### 채굴 시간
- **난이도 1**: ~수 밀리초
- **난이도 2**: ~수십 밀리초
- **난이도 3**: ~수백 밀리초
- **난이도 4**: ~수 초

### 네트워크 지연
- **로컬 네트워크**: < 10ms
- **인터넷**: 환경에 따라 다름

### 블록 크기
- 트랜잭션 수에 따라 가변
- JSON 직렬화로 인한 오버헤드

---

## 🎓 학습 포인트

이 프로젝트를 통해 다음을 학습할 수 있습니다:

1. **블록체인 기초**
   - 블록 구조 및 체인 연결
   - 해시 함수의 역할
   - 작업 증명 메커니즘

2. **암호화**
   - 타원 곡선 암호화 (ECC)
   - 디지털 서명 및 검증
   - 공개키/개인키 쌍

3. **분산 시스템**
   - P2P 네트워크 구축
   - 합의 알고리즘
   - 데이터 동기화

4. **웹 개발**
   - RESTful API 설계
   - WebSocket 통신
   - Node.js 애플리케이션 구조

---

## 🔍 코드 품질 분석

### 강점
✅ 명확한 모듈 분리 (관심사의 분리)  
✅ 객체 지향 설계  
✅ 주석과 로깅이 적절함  
✅ 표준 암호화 라이브러리 사용  

### 개선 가능 영역
⚠️ 에러 처리 강화 필요  
⚠️ 단위 테스트 부재  
⚠️ 입력 유효성 검사 보완  
⚠️ 타입 검증 (TypeScript 도입 고려)  

---

## 📚 참고 자료

### 블록체인 개념
- [Bitcoin Whitepaper](https://bitcoin.org/bitcoin.pdf)
- [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook)

### 암호화
- [Elliptic Curve Cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography)
- [secp256k1 Curve](https://en.bitcoin.it/wiki/Secp256k1)

### 구현 참고
- [Naivecoin](https://github.com/lhartikk/naivecoin)
- [Blockchain in Node.js](https://github.com/SavjeeTutorials/SavjeeCoin)

---

## 🤝 기여 방법

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다.

---

## 👨‍💻 개발 환경

- **OS**: macOS / Linux / Windows
- **Node.js**: v14 이상 권장
- **npm**: v6 이상

---

## 🎯 프로젝트 목표 달성도

| 목표 | 상태 | 비고 |
|------|------|------|
| 블록체인 구현 | ✅ 완료 | 기본 기능 구현 |
| ECC 지갑 | ✅ 완료 | secp256k1 사용 |
| P2P 네트워크 | ✅ 완료 | WebSocket 기반 |
| HTTP API | ✅ 완료 | RESTful 설계 |
| 작업 증명 | ✅ 완료 | 가변 난이도 |
| 트랜잭션 서명 | ✅ 완료 | ECDSA 서명 |
| 데이터 영속성 | ⏳ 미완 | 메모리 기반 |
| 웹 UI | ⏳ 미완 | CLI만 제공 |

---

## 📞 문의

프로젝트에 대한 질문이나 제안사항이 있으시면 이슈를 등록해주세요.

---

**작성일**: 2025-10-02  
**분석 도구**: Windsurf AI  
**프로젝트 버전**: 1.0.0
