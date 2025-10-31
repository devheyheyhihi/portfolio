# 🔧 Backend Analysis - Pi Random Chat

## 📋 목차
- [개요](#개요)
- [기술 스택](#기술-스택)
- [프로젝트 구조](#프로젝트-구조)
- [서버 아키텍처](#서버-아키텍처)
- [API 엔드포인트](#api-엔드포인트)
- [Socket.io 서버](#socketio-서버)
- [매칭 시스템](#매칭-시스템)
- [Firebase 통합](#firebase-통합)
- [Pi Network 통합](#pi-network-통합)
- [보안 & 인증](#보안--인증)
- [에러 처리](#에러-처리)
- [배포](#배포)
- [개선 제안](#개선-제안)

---

## 개요

Pi Random Chat의 백엔드는 **Express.js**를 기반으로 한 TypeScript 서버입니다. RESTful API와 WebSocket을 통해 실시간 채팅 및 랜덤 매칭 기능을 제공하며, Pi Network 블록체인 결제 시스템과 Firebase Firestore를 통합했습니다.

### 핵심 특징
- ✅ Express.js + TypeScript - 타입 안전한 서버
- ✅ Socket.io - 실시간 양방향 통신
- ✅ Firebase Firestore - NoSQL 데이터베이스
- ✅ Pi Network API - 블록체인 결제 검증
- ✅ 클래스 기반 아키텍처 - 모듈화된 구조
- ✅ 보안 미들웨어 - Helmet, CORS, Rate Limiting

---

## 기술 스택

### 핵심 의존성
```json
{
  "dependencies": {
    "express": "^4.18.2",           // 웹 프레임워크
    "socket.io": "^4.8.1",          // WebSocket 서버
    "firebase-admin": "^13.5.0",    // Firebase Admin SDK
    "axios": "^1.6.0",              // HTTP 클라이언트 (Pi API)
    "dotenv": "^16.3.1",            // 환경 변수 관리
    "helmet": "^7.1.0",             // 보안 헤더
    "cors": "^2.8.5",               // CORS 설정
    "joi": "^17.11.0",              // 입력 검증
    "morgan": "^1.10.0",            // HTTP 로깅
    "express-rate-limit": "^7.1.5"  // Rate limiting
  }
}
```

### 개발 도구
```json
{
  "devDependencies": {
    "typescript": "^5.3.3",
    "@types/express": "^4.17.21",
    "@types/node": "^20.10.0",
    "@types/cors": "^2.8.17",
    "@types/morgan": "^1.9.9",
    "@types/socket.io": "^3.0.1",
    "nodemon": "^3.0.2",            // 개발 서버 자동 재시작
    "ts-node": "^10.9.1",           // TypeScript 실행
    "jest": "^29.7.0",              // 테스트 프레임워크
    "eslint": "^8.54.0"             // 코드 린팅
  }
}
```

---

## 프로젝트 구조

```
backend/
├── src/
│   ├── server.ts                   # 메인 서버 (233줄)
│   │
│   ├── config/                     # 설정 파일
│   │   └── firebase.ts            # Firebase Admin SDK 초기화
│   │
│   ├── controllers/                # 비즈니스 로직 컨트롤러
│   │   ├── socketController.ts    # Socket.io 이벤트 핸들러 (535줄)
│   │   ├── paymentController.ts   # Pi 결제 처리
│   │   ├── userController.ts      # 사용자 관리
│   │   └── chatController.ts      # 채팅 관리
│   │
│   ├── services/                   # 비즈니스 서비스
│   │   └── matchingService.ts     # 매칭 알고리즘 (204줄)
│   │
│   ├── middleware/                 # Express 미들웨어
│   │   ├── corsMiddleware.ts      # CORS 설정
│   │   ├── authMiddleware.ts      # API 인증
│   │   ├── rateLimitMiddleware.ts # Rate limiting
│   │   └── socketAuthMiddleware.ts # Socket 인증
│   │
│   ├── routes/                     # API 라우트
│   │   ├── paymentRoutes.ts       # /api/payments
│   │   ├── userRoutes.ts          # /api/users
│   │   └── chatRoutes.ts          # /api/chat
│   │
│   ├── types/                      # TypeScript 타입 정의
│   │   ├── socket.ts              # Socket.io 타입
│   │   ├── user.ts                # 사용자 타입
│   │   └── pi.ts                  # Pi Network 타입
│   │
│   └── utils/                      # 유틸리티 함수
│       └── piApi.ts               # Pi API 래퍼
│
├── dist/                           # 컴파일된 JavaScript (빌드 결과)
├── node_modules/                   # 의존성 패키지
│
├── .env                            # 환경 변수 (gitignore)
├── env.example                     # 환경 변수 예시
├── .gitignore                      # Git 제외 파일
├── package.json                    # 의존성 관리
├── tsconfig.json                   # TypeScript 설정
├── Dockerfile                      # Docker 이미지 빌드
├── docker-compose.yml              # Docker Compose 설정
├── vercel.json                     # Vercel 배포 설정
└── README.md                       # 문서
```

---

## 서버 아키텍처

### 클래스 기반 서버 (server.ts)

```typescript
class Server {
  private app: Application;
  private httpServer: HttpServer;
  private io: SocketIOServer;
  private socketController: SocketController;
  private port: number;

  constructor() {
    this.app = express();
    this.httpServer = createServer(this.app);
    this.port = parseInt(process.env.PORT || '3002');
    
    this.initializeMiddlewares();
    this.initializeRoutes();
    this.initializeSocket();
    this.initializeErrorHandling();
  }

  public start(): void {
    this.httpServer.listen(this.port, () => {
      console.log(`🚀 Server running on port ${this.port}`);
    });
  }
}
```

### 초기화 순서

1. **미들웨어 초기화** (`initializeMiddlewares`)
   - Trust Proxy 설정 (Cloud Run 호환)
   - Helmet (보안 헤더)
   - CORS (교차 출처 리소스 공유)
   - Morgan (HTTP 로깅)
   - Body Parser (JSON/URL-encoded)

2. **라우트 초기화** (`initializeRoutes`)
   - Health Check 엔드포인트
   - API 라우트 등록
   - 404 핸들러

3. **Socket.io 초기화** (`initializeSocket`)
   - WebSocket 서버 생성
   - CORS 설정
   - 인증 미들웨어 적용
   - 이벤트 핸들러 등록

4. **에러 핸들링 초기화** (`initializeErrorHandling`)
   - 전역 에러 핸들러
   - 프로덕션/개발 환경별 에러 메시지

---

## API 엔드포인트

### Health Check
```
GET /health
GET /

응답:
{
  "success": true,
  "message": "Pi Random Chat Backend is running",
  "timestamp": "2025-09-30T04:11:30.000Z",
  "version": "1.0.0"
}
```

### 사용자 관리 (`/api/users`)

#### 1. 사용자 동기화
```
POST /api/users/sync

Headers:
  Authorization: Bearer frontend-api-key-2024
  Content-Type: application/json

Body:
{
  "access_token": "pi_access_token_here"
}

응답:
{
  "success": true,
  "user": {
    "uid": "pi_user_id",
    "username": "username",
    "app_id": "app_id",
    "profile": { ... },
    "created_at": "2025-09-30T04:11:30.000Z",
    "last_login": "2025-09-30T04:11:30.000Z"
  }
}
```

**처리 과정**:
1. Pi API로 access_token 검증
2. 사용자 정보 조회
3. Firestore에 사용자 생성/업데이트
4. 사용자 정보 반환

#### 2. 사용자 조회
```
GET /api/users/:uid

Headers:
  Authorization: Bearer frontend-api-key-2024

응답:
{
  "success": true,
  "user": { ... }
}
```

#### 3. 프로필 업데이트
```
PUT /api/users/:uid/profile

Headers:
  Authorization: Bearer frontend-api-key-2024
  Content-Type: application/json

Body:
{
  "profile": {
    "display_name": "John Doe",
    "age": 25,
    "location": "Seoul",
    "bio": "Hello!",
    "interests": ["music", "travel"],
    "profile_completed": true
  }
}

응답:
{
  "success": true,
  "user": { ... }
}
```

### 결제 관리 (`/api/payments`)

#### 1. 결제 상태 확인
```
GET /api/payments/health

응답:
{
  "success": true,
  "message": "Pi API connection is healthy",
  "pi_api_url": "https://api.minepi.com/v2"
}
```

#### 2. 결제 조회
```
GET /api/payments/:paymentId

Headers:
  Authorization: Bearer frontend-api-key-2024

응답:
{
  "success": true,
  "payment": {
    "identifier": "payment_id",
    "status": { ... },
    "transaction": { ... }
  }
}
```

#### 3. 결제 승인
```
POST /api/payments/:paymentId/approve

Headers:
  Authorization: Bearer frontend-api-key-2024
  Content-Type: application/json

Body:
{
  "payment_id": "payment_id_here"
}

응답:
{
  "success": true,
  "message": "Payment approved successfully",
  "payment_id": "payment_id_here"
}
```

**처리 과정**:
1. Pi API로 결제 정보 조회
2. 결제 유효성 검증
3. Pi API로 결제 승인 요청
4. Firestore에 결제 기록 저장

#### 4. 결제 완료
```
POST /api/payments/:paymentId/complete

Headers:
  Authorization: Bearer frontend-api-key-2024
  Content-Type: application/json

Body:
{
  "payment_id": "payment_id_here",
  "txid": "blockchain_transaction_id"
}

응답:
{
  "success": true,
  "message": "Payment completed successfully",
  "payment_id": "payment_id_here",
  "txid": "blockchain_transaction_id"
}
```

**처리 과정**:
1. Pi API로 결제 완료 요청
2. 블록체인 트랜잭션 검증
3. Firestore에 완료 상태 업데이트
4. 비즈니스 로직 실행 (프로필 언락 등)

### 채팅 관리 (`/api/chat`)

#### 1. 채팅방 목록 조회
```
GET /api/chat/rooms?uid={user_id}

Headers:
  Authorization: Bearer frontend-api-key-2024

응답:
{
  "success": true,
  "rooms": [
    {
      "id": "room_id",
      "type": "random_match",
      "participants": ["uid1", "uid2"],
      "last_message": { ... },
      "created_at": "2025-09-30T04:11:30.000Z"
    }
  ]
}
```

#### 2. 채팅방 메시지 조회
```
GET /api/chat/rooms/:roomId/messages?limit=50&before={timestamp}

Headers:
  Authorization: Bearer frontend-api-key-2024

응답:
{
  "success": true,
  "messages": [
    {
      "message_id": "msg_id",
      "room_id": "room_id",
      "sender_uid": "uid",
      "content": "Hello!",
      "timestamp": "2025-09-30T04:11:30.000Z",
      "read_by": { ... }
    }
  ]
}
```

---

## Socket.io 서버

### 서버 설정
```typescript
this.io = new SocketIOServer(this.httpServer, {
  cors: {
    origin: process.env.NODE_ENV === 'production' 
      ? ['https://nextjs-blindlounge.vercel.app', 'https://sandbox.minepi.com']
      : ['http://localhost:3000', 'http://localhost:3001'],
    methods: ['GET', 'POST'],
    credentials: true
  },
  transports: ['websocket', 'polling'],
  allowEIO3: true  // Cloud Run 호환성
});
```

### 인증 미들웨어
```typescript
// middleware/socketAuthMiddleware.ts
export const socketAuthMiddleware = async (
  socket: Socket, 
  next: (err?: Error) => void
) => {
  try {
    const { uid, username, access_token } = socket.handshake.auth;

    if (!uid || !username) {
      return next(new Error('Authentication required'));
    }

    // Socket 데이터에 사용자 정보 저장
    socket.data = {
      uid,
      username,
      authenticated: true,
      rooms: new Set<string>()
    };

    next();
  } catch (error) {
    next(new Error('Authentication failed'));
  }
};
```

### 이벤트 핸들러 (SocketController)

#### 연결 처리
```typescript
handleConnection = async (socket: Socket) => {
  console.log('🔌 New socket connection:', socket.id);

  if (requireAuth(socket)) {
    const authSocket = socket as AuthenticatedSocket;
    
    // 온라인 상태 업데이트
    await updateUserPresence(authSocket.data.uid, true, socket.id);
    
    // 다른 사용자들에게 알림
    socket.broadcast.emit('user_online', { uid: authSocket.data.uid });
  }

  // 이벤트 리스너 등록
  this.registerEventListeners(socket);

  // 연결 해제 처리
  socket.on('disconnect', () => this.handleDisconnect(socket));
};
```

#### 이벤트 리스너 등록
```typescript
private registerEventListeners = (socket: Socket) => {
  // 디버깅: 모든 이벤트 로깅
  socket.onAny((event, ...args) => {
    console.log(`📨 [Socket:${socket.id}] event=${event}`);
  });

  // 이벤트 핸들러 등록
  socket.on('authenticate', (data) => this.handleAuthenticate(socket, data));
  socket.on('join_matching_queue', (data, callback) => 
    this.handleJoinMatchingQueue(socket, data));
  socket.on('cancel_matching', (data) => this.handleCancelMatching(socket, data));
  socket.on('join_room', (data) => this.handleJoinRoom(socket, data));
  socket.on('leave_room', (data) => this.handleLeaveRoom(socket, data));
  socket.on('send_message', (data) => this.handleSendMessage(socket, data));
  socket.on('start_typing', (data) => this.handleStartTyping(socket, data));
  socket.on('stop_typing', (data) => this.handleStopTyping(socket, data));
  socket.on('mark_as_read', (data) => this.handleMarkAsRead(socket, data));
};
```

#### 메시지 전송 처리
```typescript
private handleSendMessage = async (
  socket: Socket,
  data: { room_id: string; content: string; message_type?: 'text' | 'image' | 'system' }
) => {
  const authSocket = socket as AuthenticatedSocket;
  const { room_id, content, message_type = 'text' } = data;

  // 사용자가 해당 룸에 있는지 확인
  if (!authSocket.data.rooms.has(room_id)) {
    socket.emit('error', {
      code: 'NOT_IN_ROOM',
      message: 'You are not in this room'
    });
    return;
  }

  // 메시지 데이터 생성
  const messageId = randomUUID();
  const messageData: ChatMessage = {
    message_id: messageId,
    room_id,
    sender_uid: authSocket.data.uid,
    content,
    message_type,
    timestamp: new Date(),
    read_by: {
      [authSocket.data.uid]: new Date()
    }
  };

  // Firestore에 메시지 저장
  await db.collection('messages').doc(messageId).set({
    ...messageData,
    timestamp: admin.firestore.FieldValue.serverTimestamp()
  });

  // 채팅방의 마지막 메시지 업데이트
  await db.collection('chat_rooms').doc(room_id).update({
    last_message: {
      content,
      sender_uid: authSocket.data.uid,
      timestamp: admin.firestore.FieldValue.serverTimestamp()
    }
  });

  // 룸의 모든 사용자에게 메시지 전송
  this.io.to(room_id).emit('message_received', {
    message_id: messageId,
    room_id,
    sender_uid: authSocket.data.uid,
    content,
    timestamp: new Date().toISOString(),
    sender_profile: { ... }
  });
};
```

---

## 매칭 시스템

### MatchingService 클래스

```typescript
export class MatchingService {
  private waitingUsers: Map<string, WaitingUser> = new Map();
  private io: Server;

  constructor(io: Server) {
    this.io = io;
  }
}
```

### 매칭 알고리즘 (FIFO)

```typescript
// 1. 대기열에 사용자 추가
async addToQueue(userInfo: WaitingUser): Promise<void> {
  // 중복 체크
  if (this.waitingUsers.has(userInfo.uid)) {
    return;
  }

  // 대기열에 추가
  this.waitingUsers.set(userInfo.uid, {
    ...userInfo,
    joinedAt: Date.now()
  });

  // 대기열 상태 브로드캐스트
  this.broadcastQueueStatus();

  // 즉시 매칭 시도
  await this.tryMatching();
}

// 2. 매칭 시도
private async tryMatching(): Promise<void> {
  const waitingList = Array.from(this.waitingUsers.values());
  
  if (waitingList.length >= 2) {
    // FIFO: 가장 먼저 온 두 명 매칭
    const sortedUsers = waitingList.sort((a, b) => a.joinedAt - b.joinedAt);
    const [user1, user2] = sortedUsers.slice(0, 2);

    // 매칭 결과 생성
    const matchResult = await this.createMatch(user1, user2);

    if (matchResult) {
      // 대기열에서 제거
      this.waitingUsers.delete(user1.uid);
      this.waitingUsers.delete(user2.uid);

      // 매칭 완료 알림
      this.notifyMatchFound(matchResult);

      // 대기열 상태 업데이트
      this.broadcastQueueStatus();
    }
  }
}

// 3. 채팅방 생성
private async createMatch(user1: WaitingUser, user2: WaitingUser): Promise<MatchResult | null> {
  const roomId = `match_${Date.now()}_${randomUUID().slice(0, 8)}`;

  // Firestore에 채팅방 생성
  await db.collection('chat_rooms').doc(roomId).set({
    id: roomId,
    type: 'random_match',
    participants: [user1.uid, user2.uid],
    participant_info: {
      [user1.uid]: {
        username: user1.username,
        profile: user1.profile || {}
      },
      [user2.uid]: {
        username: user2.username,
        profile: user2.profile || {}
      }
    },
    created_at: new Date(),
    last_message_at: new Date(),
    is_active: true
  });

  return { roomId, user1, user2 };
}

// 4. 매칭 완료 알림
private notifyMatchFound(matchResult: MatchResult): void {
  const { roomId, user1, user2 } = matchResult;

  // User1에게 전송
  this.io.to(user1.socketId).emit('match_found', {
    roomId,
    otherUser: {
      uid: user2.uid,
      username: user2.username,
      profile: user2.profile || {}
    }
  });

  // User2에게 전송
  this.io.to(user2.socketId).emit('match_found', {
    roomId,
    otherUser: {
      uid: user1.uid,
      username: user1.username,
      profile: user1.profile || {}
    }
  });
}
```

### 대기열 상태 브로드캐스트
```typescript
private broadcastQueueStatus(): void {
  const queueCount = this.waitingUsers.size;
  
  // 모든 대기 중인 사용자에게 상태 전송
  for (const user of this.waitingUsers.values()) {
    this.io.to(user.socketId).emit('queue_status', {
      queueCount,
      waitingTime: Date.now() - user.joinedAt
    });
  }
}
```

---

## Firebase 통합

### Firebase Admin SDK 초기화
```typescript
// config/firebase.ts
import admin from 'firebase-admin';
import serviceAccount from '../../pi-blind-lounge-backend-firebase-adminsdk.json';

if (!admin.apps.length) {
  admin.initializeApp({
    credential: admin.credential.cert(serviceAccount as admin.ServiceAccount),
    projectId: 'pi-blind-lounge-backend'
  });
}

export const db = admin.firestore();
export const auth = admin.auth();
export const storage = admin.storage();
```

### Firestore 데이터 구조

#### users 컬렉션
```typescript
{
  uid: string;                    // Pi Network 사용자 ID
  username: string;               // Pi Network 사용자명
  app_id: string;                 // Pi 앱 ID
  receiving_email: boolean;       // 이메일 수신 동의
  profile: {
    display_name: string;         // 표시 이름
    bio?: string;                 // 자기소개
    age?: number;                 // 나이
    location?: string;            // 위치
    interests?: string[];         // 관심사
    profile_image?: string;       // 프로필 이미지 URL
    profile_completed: boolean;   // 프로필 완성 여부
  };
  created_at: Timestamp;          // 생성 시간
  last_login: Timestamp;          // 마지막 로그인
  updated_at: Timestamp;          // 업데이트 시간
  online: boolean;                // 온라인 상태
  socket_id?: string;             // Socket.io ID
}
```

#### chat_rooms 컬렉션
```typescript
{
  id: string;                     // 채팅방 ID
  type: 'random_match';           // 채팅방 타입
  participants: string[];         // 참가자 UID 배열
  participant_info: {
    [uid: string]: {
      username: string;
      profile: object;
    }
  };
  created_at: Timestamp;          // 생성 시간
  last_message_at: Timestamp;     // 마지막 메시지 시간
  last_message?: {
    content: string;
    sender_uid: string;
    timestamp: Timestamp;
  };
  is_active: boolean;             // 활성 상태
}
```

#### messages 컬렉션
```typescript
{
  message_id: string;             // 메시지 ID (UUID)
  room_id: string;                // 채팅방 ID
  sender_uid: string;             // 발신자 UID
  content: string;                // 메시지 내용
  message_type: 'text' | 'image' | 'system';  // 메시지 타입
  timestamp: Timestamp;           // 전송 시간
  read_by: {
    [uid: string]: Timestamp;     // 읽음 처리 시간
  };
}
```

#### payments 컬렉션
```typescript
{
  payment_id: string;             // Pi 결제 ID
  user_uid: string;               // 결제자 UID
  amount: number;                 // 결제 금액 (Pi)
  memo: string;                   // 결제 메모
  metadata: {
    type: string;                 // 결제 타입
    target_uid?: string;          // 대상 UID (프로필 언락 등)
  };
  status: 'pending' | 'approved' | 'completed' | 'cancelled';
  txid?: string;                  // 블록체인 트랜잭션 ID
  created_at: Timestamp;          // 생성 시간
  approved_at?: Timestamp;        // 승인 시간
  completed_at?: Timestamp;       // 완료 시간
}
```

### Firestore 쿼리 예시
```typescript
// 사용자 조회
const userDoc = await db.collection('users').doc(uid).get();
const userData = userDoc.data();

// 채팅방 목록 조회
const roomsSnapshot = await db.collection('chat_rooms')
  .where('participants', 'array-contains', uid)
  .orderBy('last_message_at', 'desc')
  .limit(20)
  .get();

// 메시지 조회
const messagesSnapshot = await db.collection('messages')
  .where('room_id', '==', roomId)
  .orderBy('timestamp', 'desc')
  .limit(50)
  .get();

// 배치 업데이트
const batch = db.batch();
messageIds.forEach(messageId => {
  const messageRef = db.collection('messages').doc(messageId);
  batch.update(messageRef, {
    [`read_by.${uid}`]: new Date()
  });
});
await batch.commit();
```

---

## Pi Network 통합

### Pi API 래퍼 (utils/piApi.ts)
```typescript
import axios from 'axios';

const PI_API_BASE_URL = process.env.PI_API_BASE_URL || 'https://api.minepi.com/v2';
const PI_API_KEY = process.env.PI_API_KEY;

export const piApi = {
  // 사용자 정보 조회
  async getUser(accessToken: string) {
    const response = await axios.get(`${PI_API_BASE_URL}/me`, {
      headers: {
        'Authorization': `Bearer ${accessToken}`
      }
    });
    return response.data;
  },

  // 결제 정보 조회
  async getPayment(paymentId: string) {
    const response = await axios.get(
      `${PI_API_BASE_URL}/payments/${paymentId}`,
      {
        headers: {
          'Authorization': `Key ${PI_API_KEY}`
        }
      }
    );
    return response.data;
  },

  // 결제 승인
  async approvePayment(paymentId: string) {
    const response = await axios.post(
      `${PI_API_BASE_URL}/payments/${paymentId}/approve`,
      {},
      {
        headers: {
          'Authorization': `Key ${PI_API_KEY}`
        }
      }
    );
    return response.data;
  },

  // 결제 완료
  async completePayment(paymentId: string, txid: string) {
    const response = await axios.post(
      `${PI_API_BASE_URL}/payments/${paymentId}/complete`,
      { txid },
      {
        headers: {
          'Authorization': `Key ${PI_API_KEY}`
        }
      }
    );
    return response.data;
  }
};
```

### 결제 플로우 처리

#### 1. 결제 승인 (paymentController.ts)
```typescript
export const approvePayment = async (req: Request, res: Response) => {
  try {
    const { paymentId } = req.params;

    // 1. Pi API로 결제 정보 조회
    const payment = await piApi.getPayment(paymentId);

    // 2. 결제 유효성 검증
    if (payment.status.developer_approved) {
      return res.status(400).json({
        success: false,
        error: { message: 'Payment already approved' }
      });
    }

    // 3. Pi API로 결제 승인
    await piApi.approvePayment(paymentId);

    // 4. Firestore에 결제 기록 저장
    await db.collection('payments').doc(paymentId).set({
      payment_id: paymentId,
      user_uid: payment.user_uid,
      amount: payment.amount,
      memo: payment.memo,
      metadata: payment.metadata,
      status: 'approved',
      approved_at: admin.firestore.FieldValue.serverTimestamp()
    }, { merge: true });

    res.json({
      success: true,
      message: 'Payment approved successfully',
      payment_id: paymentId
    });

  } catch (error) {
    console.error('❌ Payment approval error:', error);
    res.status(500).json({
      success: false,
      error: { message: 'Payment approval failed' }
    });
  }
};
```

#### 2. 결제 완료 (paymentController.ts)
```typescript
export const completePayment = async (req: Request, res: Response) => {
  try {
    const { paymentId } = req.params;
    const { txid } = req.body;

    // 1. Pi API로 결제 완료
    await piApi.completePayment(paymentId, txid);

    // 2. Firestore에 완료 상태 업데이트
    await db.collection('payments').doc(paymentId).update({
      status: 'completed',
      txid,
      completed_at: admin.firestore.FieldValue.serverTimestamp()
    });

    // 3. 비즈니스 로직 실행 (프로필 언락 등)
    const paymentDoc = await db.collection('payments').doc(paymentId).get();
    const paymentData = paymentDoc.data();

    if (paymentData?.metadata?.type === 'profile_unlock') {
      const targetUid = paymentData.metadata.target_uid;
      // 프로필 언락 처리 로직
      await unlockProfile(paymentData.user_uid, targetUid);
    }

    res.json({
      success: true,
      message: 'Payment completed successfully',
      payment_id: paymentId,
      txid
    });

  } catch (error) {
    console.error('❌ Payment completion error:', error);
    res.status(500).json({
      success: false,
      error: { message: 'Payment completion failed' }
    });
  }
};
```

---

## 보안 & 인증

### 1. Helmet (보안 헤더)
```typescript
this.app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  crossOriginEmbedderPolicy: false
}));
```

### 2. CORS (교차 출처 리소스 공유)
```typescript
// middleware/corsMiddleware.ts
import cors from 'cors';

const allowedOrigins = process.env.ALLOWED_ORIGINS?.split(',') || [
  'http://localhost:3000',
  'https://nextjs-blindlounge.vercel.app'
];

export const corsMiddleware = cors({
  origin: (origin, callback) => {
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization']
});
```

### 3. API 인증 미들웨어
```typescript
// middleware/authMiddleware.ts
export const authMiddleware = (req: Request, res: Response, next: NextFunction) => {
  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({
      success: false,
      error: { code: 'UNAUTHORIZED', message: 'Authentication required' }
    });
  }

  const token = authHeader.substring(7);
  const expectedToken = process.env.FRONTEND_API_KEY;

  if (token !== expectedToken) {
    return res.status(403).json({
      success: false,
      error: { code: 'FORBIDDEN', message: 'Invalid API key' }
    });
  }

  next();
};
```

### 4. Rate Limiting
```typescript
// middleware/rateLimitMiddleware.ts
import rateLimit from 'express-rate-limit';

export const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15분
  max: 100,                   // 최대 100회 요청
  message: {
    success: false,
    error: {
      code: 'RATE_LIMIT_EXCEEDED',
      message: 'Too many requests, please try again later'
    }
  },
  standardHeaders: true,
  legacyHeaders: false
});

// 특정 엔드포인트에 적용
app.use('/api/', apiLimiter);
```

### 5. Socket.io 인증
```typescript
// middleware/socketAuthMiddleware.ts
export const socketAuthMiddleware = async (
  socket: Socket, 
  next: (err?: Error) => void
) => {
  try {
    const { uid, username, access_token } = socket.handshake.auth;

    // 필수 필드 검증
    if (!uid || !username) {
      return next(new Error('Authentication required'));
    }

    // 테스트 사용자가 아닌 경우 access_token 검증
    if (!uid.startsWith('test-user-') && access_token) {
      // Pi API로 토큰 검증 (선택적)
      // const user = await piApi.getUser(access_token);
    }

    // Socket 데이터에 사용자 정보 저장
    socket.data = {
      uid,
      username,
      authenticated: true,
      rooms: new Set<string>()
    };

    next();
  } catch (error) {
    next(new Error('Authentication failed'));
  }
};
```

---

## 에러 처리

### 전역 에러 핸들러
```typescript
// server.ts
this.app.use((error: any, req: Request, res: Response, next: any) => {
  console.error('❌ Unhandled error:', error);
  
  res.status(error.status || 500).json({
    success: false,
    error: {
      code: error.code || 'INTERNAL_ERROR',
      message: process.env.NODE_ENV === 'production' 
        ? 'Internal server error' 
        : error.message || 'Something went wrong'
    }
  });
});
```

### 커스텀 에러 클래스
```typescript
// utils/errors.ts
export class ApiError extends Error {
  constructor(
    public statusCode: number,
    public code: string,
    message: string
  ) {
    super(message);
    this.name = 'ApiError';
  }
}

// 사용 예시
throw new ApiError(404, 'USER_NOT_FOUND', 'User not found');
```

### Try-Catch 패턴
```typescript
export const someController = async (req: Request, res: Response) => {
  try {
    // 비즈니스 로직
    const result = await someService();
    
    res.json({
      success: true,
      data: result
    });
  } catch (error) {
    console.error('❌ Error:', error);
    
    res.status(500).json({
      success: false,
      error: {
        code: 'OPERATION_FAILED',
        message: error instanceof Error ? error.message : 'Unknown error'
      }
    });
  }
};
```

---

## 배포

### Docker 배포

#### Dockerfile
```dockerfile
FROM node:18-alpine

WORKDIR /app

# 의존성 설치
COPY package*.json ./
RUN npm ci --only=production

# 소스 코드 복사
COPY . .

# TypeScript 컴파일
RUN npm run build

# 포트 노출
EXPOSE 3002

# 서버 시작
CMD ["npm", "start"]
```

#### docker-compose.yml
```yaml
version: '3.8'

services:
  backend:
    build: .
    ports:
      - "3002:3002"
    environment:
      - NODE_ENV=production
      - PORT=3002
      - PI_API_KEY=${PI_API_KEY}
      - FRONTEND_API_KEY=${FRONTEND_API_KEY}
      - ALLOWED_ORIGINS=${ALLOWED_ORIGINS}
    restart: unless-stopped
```

### Google Cloud Run 배포

#### 1. Docker 이미지 빌드
```bash
gcloud builds submit --tag gcr.io/PROJECT_ID/pi-random-chat-backend
```

#### 2. Cloud Run 배포
```bash
gcloud run deploy pi-random-chat-backend \
  --image gcr.io/PROJECT_ID/pi-random-chat-backend \
  --platform managed \
  --region asia-northeast3 \
  --allow-unauthenticated \
  --set-env-vars "NODE_ENV=production,PORT=3002" \
  --set-secrets "PI_API_KEY=pi-api-key:latest"
```

### Vercel 배포 (Serverless)

#### vercel.json
```json
{
  "version": 2,
  "builds": [
    {
      "src": "src/server.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "src/server.ts"
    }
  ],
  "env": {
    "NODE_ENV": "production"
  }
}
```

---

## 개선 제안

### 🔴 긴급 (High Priority)

1. **테스트 코드 작성**
   ```typescript
   // __tests__/controllers/userController.test.ts
   describe('UserController', () => {
     it('should sync user with backend', async () => {
       const response = await request(app)
         .post('/api/users/sync')
         .send({ access_token: 'test_token' });
       
       expect(response.status).toBe(200);
       expect(response.body.success).toBe(true);
     });
   });
   ```

2. **로깅 시스템 강화**
   ```typescript
   import winston from 'winston';

   const logger = winston.createLogger({
     level: 'info',
     format: winston.format.json(),
     transports: [
       new winston.transports.File({ filename: 'error.log', level: 'error' }),
       new winston.transports.File({ filename: 'combined.log' })
     ]
   });
   ```

3. **환경 변수 검증**
   ```typescript
   import Joi from 'joi';

   const envSchema = Joi.object({
     NODE_ENV: Joi.string().valid('development', 'production', 'test').required(),
     PORT: Joi.number().default(3002),
     PI_API_KEY: Joi.string().required(),
     FRONTEND_API_KEY: Joi.string().required()
   });

   const { error, value } = envSchema.validate(process.env);
   if (error) {
     throw new Error(`Config validation error: ${error.message}`);
   }
   ```

### 🟡 중요 (Medium Priority)

4. **Redis 캐싱 추가**
   ```typescript
   import Redis from 'ioredis';

   const redis = new Redis(process.env.REDIS_URL);

   // 사용자 정보 캐싱
   const cachedUser = await redis.get(`user:${uid}`);
   if (cachedUser) {
     return JSON.parse(cachedUser);
   }

   const user = await db.collection('users').doc(uid).get();
   await redis.setex(`user:${uid}`, 3600, JSON.stringify(user.data()));
   ```

5. **매칭 서비스 분산 처리**
   ```typescript
   // Redis Pub/Sub로 매칭 서비스 분산
   const matchingQueue = new Bull('matching', {
     redis: process.env.REDIS_URL
   });

   matchingQueue.process(async (job) => {
     const { user1, user2 } = job.data;
     await createMatch(user1, user2);
   });
   ```

6. **API 문서화 (Swagger)**
   ```typescript
   import swaggerJsdoc from 'swagger-jsdoc';
   import swaggerUi from 'swagger-ui-express';

   const swaggerOptions = {
     definition: {
       openapi: '3.0.0',
       info: {
         title: 'Pi Random Chat API',
         version: '1.0.0'
       }
     },
     apis: ['./src/routes/*.ts']
   };

   const swaggerSpec = swaggerJsdoc(swaggerOptions);
   app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
   ```

### 🟢 개선 (Low Priority)

7. **GraphQL API 추가**
   ```typescript
   import { ApolloServer } from 'apollo-server-express';

   const server = new ApolloServer({
     typeDefs,
     resolvers,
     context: ({ req }) => ({ req })
   });

   await server.start();
   server.applyMiddleware({ app });
   ```

8. **WebRTC 음성/영상 통화**
   ```typescript
   // Socket.io로 WebRTC 시그널링
   socket.on('webrtc_offer', (data) => {
     socket.to(data.room_id).emit('webrtc_offer', data);
   });

   socket.on('webrtc_answer', (data) => {
     socket.to(data.room_id).emit('webrtc_answer', data);
   });
   ```

9. **AI 기반 매칭**
   ```typescript
   // 사용자 프로필 기반 유사도 계산
   const matchScore = calculateMatchScore(user1.profile, user2.profile);
   
   // 높은 점수의 사용자끼리 매칭
   const bestMatch = findBestMatch(currentUser, waitingUsers);
   ```

---

## 모니터링 & 디버깅

### 1. 헬스 체크
```typescript
app.get('/health', (req, res) => {
  res.json({
    success: true,
    uptime: process.uptime(),
    memory: process.memoryUsage(),
    timestamp: new Date().toISOString()
  });
});
```

### 2. 성능 모니터링
```typescript
import responseTime from 'response-time';

app.use(responseTime((req, res, time) => {
  console.log(`${req.method} ${req.url} - ${time}ms`);
}));
```

### 3. 에러 추적 (Sentry)
```typescript
import * as Sentry from '@sentry/node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV
});

app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.errorHandler());
```

---

## 참고 자료

- [Express.js 공식 문서](https://expressjs.com/)
- [Socket.io 공식 문서](https://socket.io/docs/v4/)
- [Firebase Admin SDK 문서](https://firebase.google.com/docs/admin/setup)
- [Pi Network 개발자 문서](https://developers.minepi.com)
- [TypeScript 공식 문서](https://www.typescriptlang.org/docs/)

---

**작성일**: 2025-09-30  
**버전**: 1.0.0  
**작성자**: Windsurf AI Analysis
