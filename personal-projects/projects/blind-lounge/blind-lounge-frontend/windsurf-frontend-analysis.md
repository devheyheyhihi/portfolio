# 🎨 Frontend Analysis - Pi Random Chat

## 📋 목차
- [개요](#개요)
- [기술 스택](#기술-스택)
- [프로젝트 구조](#프로젝트-구조)
- [주요 컴포넌트](#주요-컴포넌트)
- [상태 관리](#상태-관리)
- [API 통신](#api-통신)
- [Socket.io 통신](#socketio-통신)
- [Pi Network 통합](#pi-network-통합)
- [라우팅](#라우팅)
- [스타일링](#스타일링)
- [환경 설정](#환경-설정)
- [개선 제안](#개선-제안)

---

## 개요

Pi Random Chat의 프론트엔드는 **Next.js 15 App Router**를 기반으로 한 모던 React 애플리케이션입니다. Pi Browser에 최적화된 모바일 퍼스트 디자인으로 구현되었으며, 실시간 채팅과 블록체인 결제 기능을 제공합니다.

### 핵심 특징
- ✅ Next.js 15 (App Router) - 최신 React 19.1.0
- ✅ TypeScript - 완전한 타입 안정성
- ✅ Tailwind CSS v4 - 모던 UI 스타일링
- ✅ Socket.io Client - 실시간 양방향 통신
- ✅ Pi Network SDK - 블록체인 결제 통합
- ✅ 모바일 최적화 - Pi Browser 호환

---

## 기술 스택

### 핵심 의존성
```json
{
  "dependencies": {
    "next": "15.5.3",           // React 프레임워크
    "react": "19.1.0",          // UI 라이브러리
    "react-dom": "19.1.0",      // React DOM 렌더러
    "axios": "^1.12.2",         // HTTP 클라이언트
    "socket.io-client": "^4.8.1", // WebSocket 클라이언트
    "pi-backend": "^0.1.3"      // Pi Network SDK
  }
}
```

### 개발 도구
```json
{
  "devDependencies": {
    "@tailwindcss/postcss": "^4",
    "tailwindcss": "^4",
    "typescript": "^5",
    "@types/node": "^20",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    "eslint": "^9",
    "eslint-config-next": "15.5.3"
  }
}
```

---

## 프로젝트 구조

```
frontend/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── page.tsx           # 홈 페이지 (/)
│   │   ├── layout.tsx         # 루트 레이아웃
│   │   ├── globals.css        # 글로벌 스타일
│   │   ├── chats/             # 채팅 목록 페이지
│   │   │   └── page.tsx
│   │   ├── settings/          # 설정 페이지
│   │   │   └── page.tsx
│   │   ├── privacy/           # 개인정보 처리방침
│   │   │   └── page.tsx
│   │   ├── terms/             # 이용약관
│   │   │   └── page.tsx
│   │   ├── test-chat/         # 테스트 채팅 (개발용)
│   │   │   └── page.tsx
│   │   └── test-chat2/        # 테스트 채팅 2 (개발용)
│   │       └── page.tsx
│   │
│   ├── components/            # React 컴포넌트 (22개)
│   │   ├── HomeClient.tsx              # 메인 클라이언트 컴포넌트
│   │   ├── AppContainer.tsx            # 앱 컨테이너 래퍼
│   │   ├── Header.tsx                  # 헤더 컴포넌트
│   │   ├── BottomTabNavigation.tsx     # 하단 탭 네비게이션
│   │   │
│   │   ├── ProfileCreateScreen.tsx     # 프로필 생성 화면
│   │   ├── ProfileScreen.tsx           # 프로필 조회 화면
│   │   ├── MyProfilePage.tsx           # 내 프로필 페이지
│   │   ├── ProfilePopup.tsx            # 프로필 팝업
│   │   ├── FullProfilePopup.tsx        # 전체 프로필 팝업
│   │   │
│   │   ├── MainScreen.tsx              # 메인 화면
│   │   ├── MatchingScreen.tsx          # 매칭 대기 화면
│   │   ├── ChatScreen.tsx              # 채팅 화면
│   │   ├── StartChatScreen.tsx         # 채팅 시작 화면
│   │   │
│   │   ├── LoungeScreen.tsx            # 라운지 (커뮤니티)
│   │   ├── PostDetailScreen.tsx        # 게시글 상세
│   │   │
│   │   ├── NotificationScreen.tsx      # 알림 화면
│   │   ├── MessageDetailPopup.tsx      # 메시지 상세 팝업
│   │   ├── MessageSendPopup.tsx        # 메시지 전송 팝업
│   │   │
│   │   ├── PiCoinPopup.tsx             # Pi 코인 결제 팝업
│   │   ├── WarningPopup.tsx            # 경고 팝업
│   │   ├── DualRangeSlider.tsx         # 이중 범위 슬라이더
│   │   └── MatchPopup.tsx              # 매칭 팝업 (빈 파일)
│   │
│   ├── contexts/              # React Context
│   │   └── [context files]
│   │
│   ├── hooks/                 # Custom Hooks
│   │   └── [hook files]
│   │
│   ├── types/                 # TypeScript 타입 정의
│   │   └── [type files]
│   │
│   ├── utils/                 # 유틸리티 함수
│   │   ├── piSDK.ts          # Pi SDK 래퍼
│   │   └── [other utils]
│   │
│   ├── data/                  # Mock 데이터
│   │   └── [json files]
│   │
│   └── config/                # 설정 파일
│       └── [config files]
│
├── public/                    # 정적 파일
│   ├── images/
│   └── icons/
│
├── next.config.ts            # Next.js 설정
├── tailwind.config.js        # Tailwind CSS 설정
├── tsconfig.json             # TypeScript 설정
├── package.json              # 의존성 관리
└── pi.toml                   # Pi Network 앱 설정
```

---

## 주요 컴포넌트

### 1. **HomeClient.tsx** (메인 진입점)
**역할**: 앱의 메인 로직을 관리하는 클라이언트 컴포넌트

**주요 기능**:
- Pi Network 로그인 처리
- 사용자 프로필 체크
- 백엔드와 사용자 정보 동기화
- 프로필 생성 플로우 관리
- 약관 동의 처리

**상태 관리**:
```typescript
const [isLoggedIn, setIsLoggedIn] = useState(false);
const [showProfileCreate, setShowProfileCreate] = useState(false);
const [userInfo, setUserInfo] = useState<UserInfo | null>(null);
const [currentUser, setCurrentUser] = useState<{ uid: string; username: string } | null>(null);
```

**주요 함수**:
- `checkUserProfile()`: 사용자 프로필 존재 여부 확인
- `syncUserWithBackend()`: Pi API로 사용자 정보 검증 및 동기화
- `handlePiLogin()`: Pi Network 로그인 처리

### 2. **MatchingScreen.tsx** (매칭 시스템)
**역할**: 랜덤 매칭 대기 및 매칭 완료 처리

**주요 기능**:
- Socket.io 연결 관리
- 매칭 대기열 참가/취소
- 실시간 대기 인원 표시
- 매칭 완료 시 채팅방 이동

**Socket 이벤트**:
```typescript
// 전송
socket.emit('join_matching_queue', { uid, username, profile });
socket.emit('cancel_matching', { uid });

// 수신
socket.on('matching_queue_joined', () => {});
socket.on('queue_status', ({ queueCount, waitingTime }) => {});
socket.on('match_found', ({ roomId, otherUser }) => {});
```

### 3. **ChatScreen.tsx** (채팅 화면)
**역할**: 1:1 실시간 채팅 인터페이스

**주요 기능**:
- 실시간 메시지 송수신
- 메시지 히스토리 로드
- 타이핑 표시
- 읽음 처리
- 상대방 온라인 상태 표시

**Socket 이벤트**:
```typescript
// 전송
socket.emit('join_room', { room_id });
socket.emit('send_message', { room_id, content });
socket.emit('start_typing', { room_id });
socket.emit('stop_typing', { room_id });

// 수신
socket.on('message_received', (message) => {});
socket.on('user_typing', ({ uid }) => {});
socket.on('user_online', ({ uid }) => {});
```

### 4. **ProfileCreateScreen.tsx** (프로필 생성)
**역할**: 신규 사용자 프로필 작성

**주요 기능**:
- 프로필 이미지 업로드
- 기본 정보 입력 (이름, 나이, 위치)
- 관심사 선택
- 자기소개 작성
- 백엔드로 프로필 저장

**입력 필드**:
```typescript
interface ProfileData {
  display_name: string;
  age: number;
  location: string;
  bio: string;
  interests: string[];
  profile_image?: string;
}
```

### 5. **PiCoinPopup.tsx** (Pi 결제)
**역할**: Pi Network 결제 처리

**주요 기능**:
- Pi SDK 결제 요청
- 결제 승인 대기
- 백엔드 결제 검증
- 결제 완료 처리

**결제 플로우**:
```typescript
// 1. 결제 요청
const payment = await Pi.createPayment({
  amount: 1.0,
  memo: "Profile Unlock",
  metadata: { type: "profile_unlock", target_uid: targetUid }
});

// 2. 서버 승인 콜백
onReadyForServerApproval: async (paymentId) => {
  await axios.post(`${backendUrl}/api/payments/${paymentId}/approve`);
}

// 3. 서버 완료 콜백
onReadyForServerCompletion: async (paymentId) => {
  await axios.post(`${backendUrl}/api/payments/${paymentId}/complete`);
}
```

### 6. **BottomTabNavigation.tsx** (네비게이션)
**역할**: 하단 탭 네비게이션 바

**탭 구성**:
- 🏠 홈 (Home)
- 💬 채팅 (Chats)
- 🎭 라운지 (Lounge)
- 👤 프로필 (Profile)

---

## 상태 관리

### 로컬 상태 (useState)
대부분의 컴포넌트에서 React의 `useState` 훅을 사용하여 로컬 상태를 관리합니다.

```typescript
// 예시: HomeClient.tsx
const [isLoggedIn, setIsLoggedIn] = useState(false);
const [userInfo, setUserInfo] = useState<UserInfo | null>(null);
const [showProfileCreate, setShowProfileCreate] = useState(false);
```

### Context API
전역 상태 관리를 위해 React Context를 사용합니다.

```typescript
// contexts/ 디렉토리에 정의
// - 사용자 인증 상태
// - Socket.io 연결 상태
// - 테마 설정 등
```

### Props Drilling
부모-자식 간 데이터 전달은 props를 통해 이루어집니다.

```typescript
<ChatScreen 
  roomId={roomId}
  currentUser={currentUser}
  otherUser={otherUser}
/>
```

---

## API 통신

### Axios 설정
```typescript
import axios from 'axios';

const backendUrl = process.env.NEXT_PUBLIC_BACKEND_URL || 'http://localhost:3002';

// 공통 헤더
const headers = {
  'Authorization': 'Bearer frontend-api-key-2024',
  'Content-Type': 'application/json'
};
```

### 주요 API 엔드포인트

#### 1. 사용자 관리
```typescript
// 사용자 정보 동기화
POST /api/users/sync
Body: { access_token: string }

// 사용자 프로필 조회
GET /api/users/:uid

// 프로필 업데이트
PUT /api/users/:uid/profile
Body: { profile: ProfileData }
```

#### 2. 결제 관리
```typescript
// 결제 승인
POST /api/payments/:paymentId/approve
Body: { payment_id: string }

// 결제 완료
POST /api/payments/:paymentId/complete
Body: { payment_id: string }

// 결제 상태 조회
GET /api/payments/:paymentId
```

#### 3. 채팅 관리
```typescript
// 채팅방 목록 조회
GET /api/chat/rooms?uid={uid}

// 채팅방 메시지 조회
GET /api/chat/rooms/:roomId/messages
```

---

## Socket.io 통신

### 연결 설정
```typescript
import { io, Socket } from 'socket.io-client';

const socket = io(backendUrl, {
  transports: ['websocket', 'polling'],
  auth: {
    uid: currentUser.uid,
    username: currentUser.username
  }
});
```

### 이벤트 맵

#### 클라이언트 → 서버
| 이벤트 | 데이터 | 설명 |
|--------|--------|------|
| `authenticate` | `{ uid, access_token }` | 소켓 인증 |
| `join_matching_queue` | `{ uid, username, profile }` | 매칭 대기열 참가 |
| `cancel_matching` | `{ uid }` | 매칭 취소 |
| `join_room` | `{ room_id }` | 채팅방 입장 |
| `leave_room` | `{ room_id }` | 채팅방 퇴장 |
| `send_message` | `{ room_id, content }` | 메시지 전송 |
| `start_typing` | `{ room_id }` | 타이핑 시작 |
| `stop_typing` | `{ room_id }` | 타이핑 중지 |
| `mark_as_read` | `{ room_id, message_ids }` | 읽음 처리 |

#### 서버 → 클라이언트
| 이벤트 | 데이터 | 설명 |
|--------|--------|------|
| `matching_queue_joined` | `{ success }` | 대기열 참가 확인 |
| `queue_status` | `{ queueCount, waitingTime }` | 대기열 상태 |
| `match_found` | `{ roomId, otherUser }` | 매칭 완료 |
| `matching_cancelled` | `{ success }` | 매칭 취소 확인 |
| `room_joined` | `{ room_id, participants }` | 채팅방 입장 확인 |
| `message_received` | `{ message_id, content, ... }` | 메시지 수신 |
| `user_typing` | `{ uid, room_id }` | 타이핑 중 |
| `user_stop_typing` | `{ uid, room_id }` | 타이핑 중지 |
| `user_online` | `{ uid }` | 사용자 온라인 |
| `user_offline` | `{ uid }` | 사용자 오프라인 |
| `error` | `{ code, message }` | 에러 발생 |

### 연결 관리
```typescript
// 연결 이벤트
socket.on('connect', () => {
  console.log('✅ Socket connected:', socket.id);
});

socket.on('disconnect', () => {
  console.log('❌ Socket disconnected');
});

// 재연결 시도
socket.on('reconnect_attempt', () => {
  console.log('🔄 Attempting to reconnect...');
});

// 정리
useEffect(() => {
  return () => {
    socket.disconnect();
  };
}, []);
```

---

## Pi Network 통합

### Pi SDK 초기화
```typescript
// utils/piSDK.ts
import { Pi } from 'pi-backend';

export const piSDK = {
  authenticate: async () => {
    const scopes = ['username', 'payments'];
    const authResult = await Pi.authenticate(scopes, onIncompletePaymentFound);
    return authResult;
  },
  
  createPayment: async (paymentData) => {
    return await Pi.createPayment(paymentData, {
      onReadyForServerApproval,
      onReadyForServerCompletion,
      onCancel,
      onError
    });
  }
};
```

### 인증 플로우
```typescript
// 1. Pi 인증 요청
const authResult = await piSDK.authenticate();

// 2. Access Token 획득
const { accessToken, user } = authResult;

// 3. 백엔드로 검증
const response = await axios.post('/api/users/sync', {
  access_token: accessToken
});

// 4. 사용자 정보 저장
setUserInfo(response.data.user);
setIsLoggedIn(true);
```

### 결제 플로우
```typescript
// 1. 결제 생성
const payment = await Pi.createPayment({
  amount: 1.0,
  memo: "Profile Unlock",
  metadata: { 
    type: "profile_unlock",
    target_uid: targetUid 
  }
}, {
  // 2. 서버 승인 콜백
  onReadyForServerApproval: async (paymentId) => {
    const response = await axios.post(
      `${backendUrl}/api/payments/${paymentId}/approve`,
      { payment_id: paymentId },
      { headers }
    );
    
    if (!response.data.success) {
      throw new Error('Payment approval failed');
    }
  },
  
  // 3. 서버 완료 콜백
  onReadyForServerCompletion: async (paymentId, txid) => {
    const response = await axios.post(
      `${backendUrl}/api/payments/${paymentId}/complete`,
      { payment_id: paymentId, txid },
      { headers }
    );
    
    if (response.data.success) {
      // 프로필 잠금 해제 처리
      unlockProfile(targetUid);
    }
  },
  
  // 4. 취소 콜백
  onCancel: (paymentId) => {
    console.log('Payment cancelled:', paymentId);
  },
  
  // 5. 에러 콜백
  onError: (error, payment) => {
    console.error('Payment error:', error);
  }
});
```

---

## 라우팅

### App Router 구조
Next.js 15의 App Router를 사용하여 파일 기반 라우팅을 구현합니다.

```
app/
├── page.tsx              → /
├── layout.tsx            → 루트 레이아웃
├── chats/
│   └── page.tsx          → /chats
├── settings/
│   └── page.tsx          → /settings
├── privacy/
│   └── page.tsx          → /privacy
└── terms/
    └── page.tsx          → /terms
```

### 클라이언트 사이드 네비게이션
```typescript
import { useRouter } from 'next/navigation';

const router = useRouter();

// 페이지 이동
router.push('/chats');
router.replace('/settings');
router.back();
```

### 동적 라우팅 (필요 시 추가 가능)
```typescript
// app/chat/[roomId]/page.tsx
export default function ChatRoomPage({ 
  params 
}: { 
  params: { roomId: string } 
}) {
  return <ChatScreen roomId={params.roomId} />;
}
```

---

## 스타일링

### Tailwind CSS v4
모든 스타일링은 Tailwind CSS 유틸리티 클래스를 사용합니다.

```tsx
<div className="flex flex-col items-center justify-center min-h-screen bg-gray-100">
  <button className="px-6 py-3 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition">
    Click Me
  </button>
</div>
```

### 반응형 디자인
```tsx
<div className="
  w-full 
  sm:w-96 
  md:w-[480px] 
  lg:w-[600px]
  px-4 
  sm:px-6
">
  {/* 모바일 우선, 태블릿/데스크톱 대응 */}
</div>
```

### 커스텀 스타일 (globals.css)
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* 커스텀 유틸리티 */
@layer utilities {
  .safe-area-inset {
    padding-top: env(safe-area-inset-top);
    padding-bottom: env(safe-area-inset-bottom);
  }
}
```

### 다크 모드 (선택적)
```tsx
<div className="bg-white dark:bg-gray-900 text-black dark:text-white">
  {/* 다크 모드 대응 */}
</div>
```

---

## 환경 설정

### 환경 변수 (.env.local)
```env
# 백엔드 URL
NEXT_PUBLIC_BACKEND_URL=http://localhost:3002

# Pi Network 설정 (선택적)
NEXT_PUBLIC_PI_APP_ID=your_app_id
```

### Next.js 설정 (next.config.ts)
```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  // Turbopack 활성화
  experimental: {
    turbo: {
      // Turbopack 설정
    }
  },
  
  // 이미지 최적화
  images: {
    domains: ['firebasestorage.googleapis.com'],
  },
  
  // 환경 변수
  env: {
    BACKEND_URL: process.env.NEXT_PUBLIC_BACKEND_URL,
  }
};

export default nextConfig;
```

### TypeScript 설정 (tsconfig.json)
```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Pi Network 설정 (pi.toml)
```toml
[app]
name = "Pi Random Chat"
description = "Random chat application on Pi Network"
version = "1.0.0"

[frontend]
url = "https://nextjs-blindlounge.vercel.app"

[payment]
enabled = true
test_mode = true
```

---

## 개선 제안

### 🔴 긴급 (High Priority)

1. **에러 바운더리 추가**
   ```tsx
   // components/ErrorBoundary.tsx
   class ErrorBoundary extends React.Component {
     componentDidCatch(error, errorInfo) {
       // 에러 로깅
     }
     render() {
       if (this.state.hasError) {
         return <ErrorFallback />;
       }
       return this.props.children;
     }
   }
   ```

2. **로딩 상태 개선**
   ```tsx
   // app/loading.tsx
   export default function Loading() {
     return <LoadingSpinner />;
   }
   ```

3. **SEO 최적화**
   ```tsx
   // app/layout.tsx
   export const metadata = {
     title: 'Pi Random Chat',
     description: 'Random chat on Pi Network',
     openGraph: {
       title: 'Pi Random Chat',
       description: 'Connect with random people',
       images: ['/og-image.png'],
     }
   };
   ```

### 🟡 중요 (Medium Priority)

4. **상태 관리 라이브러리 도입**
   - Zustand 또는 Jotai 고려
   - 전역 상태 관리 단순화

5. **React Query 도입**
   ```typescript
   // API 캐싱 및 자동 리페치
   const { data, isLoading } = useQuery({
     queryKey: ['user', uid],
     queryFn: () => fetchUser(uid)
   });
   ```

6. **코드 스플리팅 최적화**
   ```tsx
   const ChatScreen = dynamic(() => import('@/components/ChatScreen'), {
     loading: () => <LoadingSpinner />
   });
   ```

### 🟢 개선 (Low Priority)

7. **테스트 코드 작성**
   ```typescript
   // __tests__/HomeClient.test.tsx
   describe('HomeClient', () => {
     it('should render login button', () => {
       render(<HomeClient />);
       expect(screen.getByText('Login with Pi')).toBeInTheDocument();
     });
   });
   ```

8. **Storybook 도입**
   - 컴포넌트 문서화
   - UI 개발 효율화

9. **접근성 개선**
   ```tsx
   <button 
     aria-label="Send message"
     role="button"
     tabIndex={0}
   >
     Send
   </button>
   ```

---

## 성능 최적화

### 1. 이미지 최적화
```tsx
import Image from 'next/image';

<Image 
  src="/profile.jpg"
  alt="Profile"
  width={100}
  height={100}
  priority={true}  // LCP 최적화
/>
```

### 2. 메모이제이션
```tsx
const MemoizedChatMessage = React.memo(ChatMessage);

const expensiveValue = useMemo(() => {
  return computeExpensiveValue(data);
}, [data]);

const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);
```

### 3. 가상 스크롤링
```tsx
// 긴 채팅 메시지 목록에 적용
import { VirtualList } from 'react-virtual';

<VirtualList
  height={600}
  itemCount={messages.length}
  itemSize={80}
  renderItem={({ index }) => <Message data={messages[index]} />}
/>
```

---

## 보안 고려사항

### 1. XSS 방지
```tsx
// 사용자 입력 sanitize
import DOMPurify from 'dompurify';

const sanitizedContent = DOMPurify.sanitize(userInput);
```

### 2. CSRF 방지
```typescript
// API 요청 시 토큰 포함
axios.defaults.headers.common['X-CSRF-Token'] = csrfToken;
```

### 3. 민감 정보 보호
```typescript
// 환경 변수 사용
const apiKey = process.env.NEXT_PUBLIC_API_KEY;

// 절대 하드코딩하지 않기
// ❌ const apiKey = "sk_live_123456";
```

---

## 디버깅 팁

### 1. React DevTools
- 컴포넌트 트리 확인
- Props/State 검사
- 리렌더링 추적

### 2. Socket.io 디버깅
```typescript
// 모든 이벤트 로깅
socket.onAny((event, ...args) => {
  console.log(`[Socket] ${event}:`, args);
});
```

### 3. Network 탭
- API 요청/응답 확인
- WebSocket 연결 상태 확인
- 성능 병목 지점 파악

---

## 배포

### Vercel 배포
```bash
# 1. Vercel CLI 설치
npm i -g vercel

# 2. 프로젝트 배포
vercel

# 3. 프로덕션 배포
vercel --prod
```

### 환경 변수 설정
Vercel Dashboard에서 환경 변수 추가:
- `NEXT_PUBLIC_BACKEND_URL`
- `NEXT_PUBLIC_PI_APP_ID`

### 빌드 최적화
```json
{
  "scripts": {
    "build": "next build --turbopack",
    "analyze": "ANALYZE=true next build"
  }
}
```

---

## 참고 자료

- [Next.js 공식 문서](https://nextjs.org/docs)
- [React 공식 문서](https://react.dev)
- [Tailwind CSS 문서](https://tailwindcss.com/docs)
- [Socket.io Client 문서](https://socket.io/docs/v4/client-api/)
- [Pi Network 개발자 문서](https://developers.minepi.com)

---

**작성일**: 2025-09-30  
**버전**: 1.0.0  
**작성자**: Windsurf AI Analysis
