# 🚀 Cursor Pi Random Chat - Backend

Pi Network 기반 실시간 랜덤 채팅 애플리케이션의 백엔드 서버입니다.

## 📋 프로젝트 개요

- **프레임워크**: Express.js + TypeScript
- **실시간 통신**: Socket.io
- **데이터베이스**: Firebase Firestore
- **인증**: Pi Network API
- **배포**: Google Cloud Run
- **언어**: TypeScript

## 🚀 주요 기능

### 🔐 인증 시스템
- **Pi Network 인증**: Pi API를 통한 사용자 검증
- **JWT 토큰 관리**: 안전한 세션 관리
- **사용자 프로필**: Firestore 기반 프로필 저장

### 💬 실시간 매칭 & 채팅
- **매칭 큐 시스템**: FIFO 알고리즘 기반 사용자 매칭
- **실시간 메시징**: Socket.io를 통한 즉시 메시지 전송
- **채팅방 관리**: 자동 방 생성 및 메시지 저장

### 💰 결제 시스템
- **Pi Payment 통합**: Pi Network 결제 처리
- **결제 검증**: 안전한 트랜잭션 검증
- **결제 내역 관리**: Firestore 기반 결제 기록

### 🛡️ 보안 & 성능
- **Rate Limiting**: API 호출 제한
- **CORS 설정**: 안전한 크로스 오리진 요청
- **Helmet**: 보안 헤더 설정
- **에러 처리**: 체계적인 에러 관리

## 📁 프로젝트 구조

```
backend/src/
├── server.ts              # 메인 서버 엔트리포인트
├── config/                # 설정 파일
│   └── firebase.ts        # Firebase 초기화
├── controllers/           # 비즈니스 로직 컨트롤러
│   ├── userController.ts  # 사용자 관리
│   ├── chatController.ts  # 채팅 관리
│   ├── paymentController.ts # 결제 처리
│   └── socketController.ts # Socket.io 이벤트 처리
├── middleware/            # 미들웨어
│   ├── authMiddleware.ts  # 인증 미들웨어
│   ├── corsMiddleware.ts  # CORS 설정
│   ├── rateLimitMiddleware.ts # Rate Limiting
│   └── socketAuthMiddleware.ts # Socket 인증
├── routes/                # API 라우트
│   ├── userRoutes.ts      # 사용자 API
│   ├── chatRoutes.ts      # 채팅 API
│   └── paymentRoutes.ts   # 결제 API
├── services/              # 비즈니스 서비스
│   └── matchingService.ts # 매칭 시스템
├── types/                 # TypeScript 타입 정의
│   ├── user.ts           # 사용자 타입
│   ├── pi.ts             # Pi Network 타입
│   └── socket.ts         # Socket.io 타입
└── utils/                 # 유틸리티 함수
    └── piApi.ts          # Pi API 클라이언트
```

## 🛠️ 설치 및 실행

### 필수 요구사항
- Node.js 18.0.0 이상
- Firebase 프로젝트
- Pi Network 개발자 계정

### 설치
```bash
# 의존성 설치
npm install

# 또는
yarn install
```

### 환경 변수 설정
`env.yaml` 파일을 생성하고 다음 변수들을 설정하세요:

```yaml
# Pi Network 설정
PI_API_KEY: "your_pi_api_key"
PI_API_URL: "https://api.minepi.com"
PI_SANDBOX: "true"

# Firebase 설정
FIREBASE_PROJECT_ID: "your_project_id"
FIREBASE_PRIVATE_KEY: "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
FIREBASE_CLIENT_EMAIL: "firebase-adminsdk-xxxxx@your_project.iam.gserviceaccount.com"

# 서버 설정
PORT: "3002"
NODE_ENV: "development"
FRONTEND_URL: "http://localhost:3000"

# API 키 (보안)
FRONTEND_API_KEY: "frontend-api-key-2024"
```

### 개발 서버 실행
```bash
# 개발 모드 (nodemon 사용)
npm run dev

# 또는
yarn dev
```

서버가 실행되면 http://localhost:3002 에서 확인할 수 있습니다.

### 빌드 및 배포
```bash
# TypeScript 빌드
npm run build

# 프로덕션 서버 실행
npm run start

# Google Cloud Run 배포
gcloud run deploy pi-backend \
  --source . \
  --region asia-northeast3 \
  --allow-unauthenticated \
  --env-vars-file=env.yaml
```

## 🔌 API 엔드포인트

### 🔐 사용자 API (`/api/users`)
```http
GET    /api/users/:uid          # 사용자 프로필 조회
POST   /api/users               # 사용자 프로필 생성
PUT    /api/users/:uid          # 사용자 프로필 수정
POST   /api/users/sync          # Pi Network 사용자 동기화
GET    /api/users/me            # 현재 사용자 정보 (Pi Token 필요)
```

### 💬 채팅 API (`/api/chats`)
```http
GET    /api/chats/:roomId/messages    # 채팅방 메시지 조회
POST   /api/chats/:roomId/messages    # 메시지 전송
POST   /api/chats/rooms               # 채팅방 생성
```

### 💰 결제 API (`/api/payments`)
```http
POST   /api/payments/approve     # 결제 승인
POST   /api/payments/complete    # 결제 완료
GET    /api/payments/incomplete  # 미완료 결제 조회
```

### 🏥 헬스체크
```http
GET    /health                   # 서버 상태 확인
```

## 🔌 Socket.io 이벤트

### 클라이언트 → 서버
```typescript
// 매칭 시스템
'join_matching_queue'    // 매칭 대기열 참여
'cancel_matching'        // 매칭 취소

// 채팅 시스템
'join_room'             // 채팅방 참여
'leave_room'            // 채팅방 나가기
'send_message'          // 메시지 전송

// 테스트
'test_event'            // 연결 테스트
```

### 서버 → 클라이언트
```typescript
// 매칭 시스템
'match_found'           // 매칭 성공
'matching_cancelled'    // 매칭 취소됨
'matching_queue_joined' // 대기열 참여 완료
'queue_status'          // 대기열 상태

// 채팅 시스템
'message_received'      // 메시지 수신
'room_joined'          // 채팅방 참여 완료
'user_online'          // 사용자 온라인
'user_offline'         // 사용자 오프라인
```

## 🎯 매칭 시스템

### 매칭 알고리즘
- **FIFO (First In, First Out)**: 먼저 대기한 사용자부터 매칭
- **실시간 처리**: Socket.io를 통한 즉시 매칭 알림
- **자동 정리**: 연결 해제된 사용자 자동 제거

### 매칭 플로우
1. 사용자가 `join_matching_queue` 이벤트 전송
2. 서버가 대기열에 사용자 추가
3. 2명이 모이면 자동으로 매칭 및 채팅방 생성
4. 양쪽 사용자에게 `match_found` 이벤트 전송

## 🛡️ 보안 기능

### 인증 미들웨어
```typescript
// Pi Network 토큰 검증
const authMiddleware = async (req, res, next) => {
  const token = req.headers.authorization;
  const user = await verifyPiToken(token);
  req.user = user;
  next();
};
```

### Rate Limiting
```typescript
// API 호출 제한 (분당 100회)
const limiter = rateLimit({
  windowMs: 1 * 60 * 1000, // 1분
  max: 100,
  message: 'Too many requests'
});
```

### CORS 설정
```typescript
// 허용된 도메인만 접근 가능
const corsOptions = {
  origin: [
    'http://localhost:3000',
    'https://your-frontend-domain.com'
  ],
  credentials: true
};
```

## 🔧 주요 서비스

### MatchingService
- 매칭 대기열 관리
- 사용자 매칭 로직
- 채팅방 자동 생성

### SocketController
- Socket.io 이벤트 처리
- 실시간 통신 관리
- 연결 상태 모니터링

### UserController
- 사용자 프로필 관리
- Pi Network 연동
- 인증 처리

### PaymentController
- Pi Payment 처리
- 결제 검증
- 트랜잭션 관리

## 📊 모니터링 & 로깅

### 로그 레벨
- `🔌` 연결 관련 로그
- `🎯` 매칭 시스템 로그
- `💬` 채팅 메시지 로그
- `💰` 결제 관련 로그
- `❌` 에러 로그

### 헬스체크
```bash
# 서버 상태 확인
curl http://localhost:3002/health

# 응답 예시
{
  "status": "healthy",
  "timestamp": "2024-01-01T00:00:00.000Z",
  "uptime": 3600,
  "version": "1.0.0"
}
```

## 🧪 테스트

### 단위 테스트
```bash
# 테스트 실행
npm run test

# 커버리지 확인
npm run test:coverage
```

### API 테스트
```bash
# 사용자 생성 테스트
curl -X POST http://localhost:3002/api/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_pi_token" \
  -d '{"display_name": "Test User"}'

# Socket.io 연결 테스트
# 브라우저 개발자 도구에서:
const socket = io('http://localhost:3002');
socket.emit('test_event', { message: 'Hello' });
```

## 🚨 문제 해결

### Firebase 연결 실패
```bash
# Firebase 설정 확인
echo $FIREBASE_PROJECT_ID
echo $FIREBASE_CLIENT_EMAIL

# 권한 확인
gcloud auth list
```

### Socket.io 연결 실패
```bash
# 포트 확인
netstat -an | grep 3002

# CORS 설정 확인
curl -H "Origin: http://localhost:3000" \
     -H "Access-Control-Request-Method: GET" \
     -H "Access-Control-Request-Headers: X-Requested-With" \
     -X OPTIONS http://localhost:3002
```

### Pi API 연결 실패
```bash
# Pi API 키 확인
curl -H "Authorization: Key $PI_API_KEY" \
     https://api.minepi.com/v2/me

# 샌드박스 모드 확인
echo $PI_SANDBOX
```

## 📈 성능 최적화

### 데이터베이스 최적화
- Firestore 인덱스 설정
- 쿼리 최적화
- 캐싱 전략

### Socket.io 최적화
- 네임스페이스 분리
- 룸 기반 메시지 전송
- 연결 풀링

### 메모리 관리
- 매칭 큐 정리
- 비활성 연결 제거
- 가비지 컬렉션 최적화

## 📄 라이선스

MIT License

## 👥 개발팀

Pi Random Chat Team

---

**개발 환경**: Node.js 18+, Express.js, TypeScript, Socket.io  
**배포**: Google Cloud Run  
**데이터베이스**: Firebase Firestore  
**실시간 통신**: Socket.io  
**인증**: Pi Network API
