# 🎯 Cursor Pi Random Chat - Frontend

Pi Network 기반 실시간 랜덤 채팅 애플리케이션의 프론트엔드입니다.

## 📋 프로젝트 개요

- **프레임워크**: Next.js 15.5.3 (App Router)
- **언어**: TypeScript
- **스타일링**: Tailwind CSS 4.0
- **실시간 통신**: Socket.io Client
- **상태 관리**: React Hooks
- **인증**: Pi Network SDK

## 🚀 주요 기능

### 🔐 인증 시스템
- **Pi Network 로그인**: Pi SDK를 통한 안전한 인증
- **로컬 테스트 모드**: 개발용 테스트 계정 지원
- **프로필 관리**: 사용자 프로필 생성 및 수정

### 💬 실시간 채팅
- **매칭 시스템**: FIFO 알고리즘 기반 사용자 매칭
- **실시간 메시징**: Socket.io를 통한 즉시 메시지 전송
- **채팅방 관리**: 자동 방 생성 및 관리

### 🎨 사용자 인터페이스
- **반응형 디자인**: 모바일 우선 설계
- **매칭 애니메이션**: 부드러운 매칭 대기 애니메이션
- **하단 네비게이션**: 직관적인 탭 기반 네비게이션

## 📁 프로젝트 구조

```
frontend/src/
├── app/                    # Next.js App Router 페이지
│   ├── page.tsx           # 홈페이지 (로그인)
│   ├── chats/             # 채팅 페이지
│   ├── settings/          # 설정 페이지
│   ├── test-chat/         # 테스트 채팅 페이지
│   └── test-chat2/        # 고급 테스트 페이지
├── components/            # 재사용 가능한 컴포넌트
│   ├── AppContainer.tsx   # 메인 앱 컨테이너
│   ├── MatchingScreen.tsx # 매칭 화면
│   ├── ChatScreen.tsx     # 채팅 화면
│   ├── HomeClient.tsx     # 홈 클라이언트 로직
│   └── ...               # 기타 UI 컴포넌트
├── hooks/                 # 커스텀 React Hooks
│   ├── useSocket.ts       # Socket.io 연결 관리
│   └── useChatAPI.ts      # 채팅 API 관리
├── utils/                 # 유틸리티 함수
│   ├── piSDK.ts          # Pi Network SDK 래퍼
│   └── imageStorage.ts    # 이미지 저장 유틸리티
├── types/                 # TypeScript 타입 정의
│   ├── user.ts           # 사용자 타입
│   ├── pi.ts             # Pi Network 타입
│   └── lounge.ts         # 라운지 타입
└── config/               # 설정 파일
    └── firebase.ts       # Firebase 설정
```

## 🛠️ 설치 및 실행

### 필수 요구사항
- Node.js 18.0.0 이상
- npm 또는 yarn

### 설치
```bash
# 의존성 설치
npm install

# 또는
yarn install
```

### 환경 변수 설정
`.env.local` 파일을 생성하고 다음 변수들을 설정하세요:

```env
# Pi Network 설정
NEXT_PUBLIC_PI_API_KEY=your_pi_api_key
NEXT_PUBLIC_PI_SANDBOX=true

# 백엔드 서버 URL
NEXT_PUBLIC_BACKEND_URL=http://localhost:3002
NEXT_PUBLIC_SOCKET_URL=http://localhost:3002

# Firebase 설정 (선택사항)
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
```

### 개발 서버 실행
```bash
# 개발 모드 (Turbopack 사용)
npm run dev

# 또는
yarn dev
```

서버가 실행되면 http://localhost:3000 에서 확인할 수 있습니다.

### 빌드 및 배포
```bash
# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm run start
```

## 🔧 주요 컴포넌트

### AppContainer.tsx
- 전체 앱의 네비게이션 상태 관리
- 화면 전환 로직 처리
- 하단 탭 네비게이션 제어

### MatchingScreen.tsx
- 실시간 매칭 시스템
- 매칭 대기 애니메이션
- Socket.io를 통한 매칭 이벤트 처리

### ChatScreen.tsx
- 실시간 채팅 인터페이스
- 메시지 전송/수신
- 채팅방 관리

### HomeClient.tsx
- Pi Network 로그인 처리
- 사용자 인증 상태 관리
- 프로필 생성/수정 플로우

## 🔌 Socket.io 이벤트

### 클라이언트 → 서버
- `join_matching_queue`: 매칭 대기열 참여
- `cancel_matching`: 매칭 취소
- `join_room`: 채팅방 참여
- `send_message`: 메시지 전송

### 서버 → 클라이언트
- `match_found`: 매칭 성공
- `matching_cancelled`: 매칭 취소됨
- `message_received`: 메시지 수신
- `queue_status`: 대기열 상태 업데이트

## 🧪 테스트 모드

개발 환경에서는 Pi Network 없이도 테스트할 수 있습니다:

1. **기존 사용자 테스트**: `local-test-1` 계정
2. **신규 사용자 테스트**: `local-test-2` 계정
3. **랜덤 테스트 사용자**: 자동 생성되는 테스트 계정

## 📱 지원 기능

- ✅ Pi Network 인증
- ✅ 실시간 매칭
- ✅ 실시간 채팅
- ✅ 프로필 관리
- ✅ 반응형 디자인
- ✅ 오프라인 감지
- ✅ 에러 처리

## 🔍 디버깅

개발자 도구 콘솔에서 다음 로그들을 확인할 수 있습니다:

- `🔌` Socket.io 연결 상태
- `🎯` 매칭 시스템 이벤트
- `💬` 채팅 메시지 전송/수신
- `🔐` 인증 관련 이벤트

## 🚨 문제 해결

### Socket 연결 실패
```bash
# 백엔드 서버가 실행 중인지 확인
curl http://localhost:3002/health

# 환경 변수 확인
echo $NEXT_PUBLIC_SOCKET_URL
```

### Pi Network 로그인 실패
1. Pi Browser에서 실행 중인지 확인
2. Pi SDK 초기화 로그 확인
3. 환경 변수 설정 확인

### 빌드 에러
```bash
# 타입 체크
npm run lint

# 의존성 재설치
rm -rf node_modules package-lock.json
npm install
```

## 📄 라이선스

MIT License

## 👥 개발팀

Pi Random Chat Team

---

**개발 환경**: Node.js 18+, Next.js 15, TypeScript, Tailwind CSS  
**배포**: Vercel (권장)  
**실시간 통신**: Socket.io  
**인증**: Pi Network SDK
