# Windsurf 프로젝트 분석: 모두의 예체능 (MoyeApp)

## 📋 프로젝트 개요

**프로젝트명**: 모두의 예체능 (edonistastMoye)  
**플랫폼**: React Native (iOS & Android)  
**버전**: 0.0.1  
**React Native 버전**: 0.70.6  
**React 버전**: 18.1.0

## 🎯 프로젝트 목적

모두의 예체능은 예술 및 체육 활동 관련 레슨을 제공하는 모바일 애플리케이션입니다. 사용자들이 다양한 카테고리의 레슨을 검색하고, 예약하며, 강사와 소통할 수 있는 플랫폼을 제공합니다.

## 🏗️ 프로젝트 구조

```
MoyeApp/
├── android/                    # Android 네이티브 코드
├── ios/                        # iOS 네이티브 코드
├── src/                        # 소스 코드
│   ├── App.tsx                # 앱 진입점
│   ├── assets/                # 이미지 및 정적 리소스
│   ├── const/                 # 상수 정의
│   ├── Event/                 # 이벤트 관리
│   ├── interface/             # TypeScript 인터페이스
│   ├── models/                # 데이터 모델
│   ├── store/                 # 상태 관리 (Zustand)
│   ├── styles/                # 스타일 정의
│   ├── type/                  # TypeScript 타입 정의
│   ├── util/                  # 유틸리티 함수
│   └── views/                 # UI 컴포넌트 및 화면
│       ├── camera/            # 카메라 관련
│       ├── components/        # 재사용 가능한 컴포넌트
│       ├── navigation/        # 네비게이션 설정
│       ├── picture/           # 이미지 관련
│       └── screens/           # 화면 컴포넌트
│           ├── AuthScreen/    # 인증 화면
│           ├── CategoryScreen/# 카테고리 화면
│           ├── FavoriteScreen/# 즐겨찾기 화면
│           ├── HomeScreen/    # 홈 화면
│           ├── LessonScreen/  # 레슨 화면
│           ├── MypageScreen/  # 마이페이지
│           ├── SearchScreen/  # 검색 화면
│           └── TermsScreen/   # 약관 화면
├── __tests__/                 # 테스트 파일
├── schema.gql                 # GraphQL 스키마
├── package.json               # 의존성 관리
├── tsconfig.json              # TypeScript 설정
└── README.md                  # 프로젝트 문서
```

## 🛠️ 기술 스택

### 핵심 프레임워크
- **React Native**: 0.70.6
- **React**: 18.1.0
- **TypeScript**: 4.9.3

### 상태 관리
- **Zustand**: 4.2.0 - 경량 상태 관리
- **Jotai**: 1.10.0 - Atomic 상태 관리
- **Immer**: 9.0.16 - 불변성 관리

### 네비게이션
- **@react-navigation/native**: 6.1.1
- **@react-navigation/native-stack**: 6.9.6
- **@react-navigation/bottom-tabs**: 6.5.1
- **@react-navigation/drawer**: 6.5.5
- **@react-navigation/material-top-tabs**: 6.5.1
- **@react-navigation/stack**: 6.3.10

### UI 라이브러리
- **styled-components**: 5.3.6 - CSS-in-JS
- **react-native-paper**: 4.12.5 - Material Design
- **@rneui/themed**: 4.0.0-rc.7 - React Native Elements

### Firebase 통합
- **@react-native-firebase/app**: 16.5.2
- **@react-native-firebase/analytics**: 16.5.2
- **@react-native-firebase/auth**: 16.5.2
- **@react-native-firebase/crashlytics**: 16.5.2
- **@react-native-firebase/messaging**: 16.5.2 (FCM)
- **@react-native-firebase/remote-config**: 16.5.2
- **@react-native-firebase/dynamic-links**: 16.5.2

### 소셜 로그인
- **@react-native-google-signin/google-signin**: 9.0.2
- **@react-native-seoul/kakao-login**: 5.2.6
- **@react-native-seoul/naver-login**: 3.0.0-rc.2
- **@invertase/react-native-apple-authentication**: 2.2.2

### 데이터 통신
- **axios**: 1.2.1 - HTTP 클라이언트
- **graphql**: 16.6.0 - GraphQL 클라이언트
- **graphql-ws**: 5.11.2 - GraphQL WebSocket

### 유틸리티
- **dayjs**: 1.11.6 - 날짜/시간 처리
- **lodash**: 4.17.21 - 유틸리티 함수
- **jwt-decode**: 3.1.2 - JWT 토큰 디코딩
- **numbro**: 2.3.6 - 숫자 포맷팅
- **geolib**: 3.3.3 - 위치 계산

### UI 컴포넌트
- **react-native-calendars**: 1.1291.1 - 캘린더
- **react-native-image-picker**: 4.10.1 - 이미지 선택
- **react-native-fast-image**: 8.6.3 - 이미지 최적화
- **react-native-modal**: 13.0.1 - 모달
- **react-native-webview**: 11.26.1 - 웹뷰
- **react-native-vector-icons**: 9.2.0 - 아이콘
- **react-native-svg**: 13.6.0 - SVG 지원

### 결제
- **iamport-react-native**: 2.0.2 - 아임포트 결제

### 개발 도구
- **ESLint**: 8.28.0 - 코드 린팅
- **Prettier**: 2.8.1 - 코드 포맷팅
- **Husky**: 8.0.2 - Git hooks
- **lint-staged**: 13.1.0 - Staged 파일 린팅
- **Reactotron**: 5.0.3 - 디버깅 도구
- **Flipper**: 0.163.0 - 네이티브 디버깅

## 📱 주요 기능

### 1. 인증 시스템
- 소셜 로그인 (Google, Kakao, Naver, Apple)
- Firebase Authentication 통합
- JWT 기반 토큰 관리

### 2. 화면 구성
- **홈 화면**: 메인 대시보드
- **카테고리 화면**: 레슨 카테고리 탐색
- **검색 화면**: 레슨 검색 기능
- **레슨 화면**: 레슨 상세 정보 및 예약
- **즐겨찾기 화면**: 관심 레슨 관리
- **마이페이지**: 사용자 프로필 및 설정

### 3. 실시간 기능
- GraphQL Subscription을 통한 실시간 알림
- 채팅 기능 (강사-수강생 간 소통)
- Firebase Cloud Messaging (FCM) 푸시 알림

### 4. 결제 시스템
- 아임포트 결제 연동
- 다양한 결제 수단 지원

### 5. 미디어 처리
- 이미지 업로드 및 관리
- 카메라 통합
- 이미지 최적화 (Fast Image)

## 🔧 개발 환경 설정

### 필수 요구사항
- **Node.js**: LTS 버전
- **OpenJDK**: 11
- **Android Studio**: 최신 버전
- **Xcode**: (iOS 개발 시)
- **CocoaPods**: (iOS 의존성 관리)

### 설치 방법

#### Windows 환경
1. **Chocolatey 설치**
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process
   [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

2. **Node.js 및 OpenJDK 설치**
   ```powershell
   choco install -y nodejs-lts openjdk11
   ```

3. **Android Studio 설치**
   - https://developer.android.com/studio 에서 다운로드
   - 환경 변수 설정:
     - `ANDROID_HOME`: `%LOCALAPPDATA%\Android\Sdk`
     - `PATH`에 `%LOCALAPPDATA%\Android\Sdk\platform-tools` 추가

4. **가상 디바이스 설정**
   - Android Studio > Tools > Device Manager
   - Create Device > API Level 29 이상 선택

#### macOS 환경
```bash
# Homebrew를 통한 설치
brew install node
brew install openjdk@11

# iOS 의존성 설치
cd ios
gem install cocoapods
gem install ffi
pod install
```

### 프로젝트 설치
```bash
# 의존성 설치
yarn install

# iOS 의존성 설치 (macOS만)
yarn install:ios
```

## 🚀 실행 방법

### Android
```bash
# Metro 번들러 시작
yarn start

# Android 앱 실행 (개발 모드)
yarn android

# Android 앱 실행 (릴리즈 모드)
yarn release:android

# Android 번들 생성
yarn bundle:android
```

### iOS
```bash
# iOS 앱 실행 (개발 모드)
yarn ios

# iOS 앱 실행 (릴리즈 모드)
yarn release:ios

# iOS 빌드
yarn build:ios
```

## 🧹 유지보수 명령어

### 캐시 정리
```bash
# Watchman 캐시 정리
yarn clean:watchman

# 전체 캐시 정리
yarn clean:cache

# React Native 캐시 정리
yarn clean:react

# Android 빌드 정리
yarn clean:android

# iOS 빌드 정리
yarn clean:ios

# Lock 파일 정리
yarn clean:lock

# 전체 정리
yarn clean:all
```

### 코드 품질
```bash
# ESLint 실행
yarn lint

# Prettier 체크
yarn prettier:check

# Prettier 적용
yarn prettier:write

# 테스트 실행
yarn test
```

## 📊 GraphQL 스키마

프로젝트는 GraphQL을 사용하여 백엔드와 통신합니다.

### 주요 타입
- **Chat**: 채팅 메시지
- **notificationPubsubOutput**: 알림 데이터
- **chatPubsubOutput**: 채팅 실시간 데이터

### 주요 쿼리
- `notificationList`: 알림 목록 조회
- `findAllChats`: 채팅방 메시지 조회

### 주요 뮤테이션
- `changeReadStatus`: 알림 읽음 상태 변경
- `deleteNotification`: 알림 삭제
- `chat`: 채팅 메시지 전송

### 구독 (Subscription)
- `notification`: 실시간 알림 수신
- `openChat`: 실시간 채팅 메시지 수신

## 🎨 스타일링 시스템

### Path Alias 설정
프로젝트는 TypeScript path alias를 사용하여 import 경로를 단순화합니다:

```typescript
@image       → assets/images
@images/*    → assets/images/*
@style/*     → styles/*
@styleUtil   → util/screenUtil
@type        → type
@typed/*     → type/*
@store       → store
@stores/*    → store/*
@event       → Event
@events/*    → Event/*
@const       → const
@consts/*    → const/*
@util        → util
@utils/*     → util/*
@view        → views/*
@views/*     → views/*
@component   → views/components
@components/* → views/components/*
```

### 반응형 디자인
- 기준 해상도: 375 x 812 (iPhone X)
- `ScreenUtilInitilize`를 통한 반응형 처리
- 디바이스 크기에 따른 자동 스케일링

## 🔐 보안 고려사항

1. **API 키 관리**
   - Firebase 설정은 환경 변수로 관리
   - 소셜 로그인 키는 안전하게 저장

2. **인증 토큰**
   - JWT 토큰 기반 인증
   - AsyncStorage를 통한 안전한 저장

3. **네트워크 보안**
   - HTTPS 통신
   - Certificate Pinning 고려

## 📈 성능 최적화

1. **이미지 최적화**
   - react-native-fast-image 사용
   - 이미지 캐싱 전략

2. **번들 최적화**
   - Code splitting
   - Lazy loading

3. **상태 관리 최적화**
   - Zustand의 경량 상태 관리
   - 불필요한 리렌더링 방지

## 🐛 디버깅 도구

1. **Reactotron**
   - 상태 관리 모니터링
   - API 요청/응답 추적
   - 개발 모드에서만 활성화

2. **Flipper**
   - 네트워크 인스펙터
   - 레이아웃 디버깅
   - AsyncStorage 뷰어

3. **Firebase Analytics**
   - 사용자 행동 추적
   - 크래시 리포팅 (Crashlytics)

## 📝 코딩 컨벤션

### TypeScript
- 엄격한 타입 체크
- Interface 우선 사용
- Enum 대신 Union Type 권장

### 컴포넌트
- 함수형 컴포넌트 사용
- React Hooks 활용
- Props는 interface로 정의

### 스타일
- styled-components 사용
- 재사용 가능한 스타일 컴포넌트
- 테마 시스템 활용

### 파일 명명
- 컴포넌트: PascalCase (예: `HomeScreen.tsx`)
- 유틸리티: camelCase (예: `dateUtil.ts`)
- 상수: UPPER_SNAKE_CASE

## 🔄 CI/CD

### Git Hooks (Husky)
- **pre-commit**: ESLint 및 Prettier 자동 실행
- **lint-staged**: 변경된 파일만 검사

### 빌드 프로세스
1. 코드 린팅 및 포맷팅
2. TypeScript 컴파일 체크
3. 테스트 실행
4. 네이티브 빌드 (Android/iOS)

## 📦 주요 의존성 버전 관리

### 주의사항
- React Native 버전 업그레이드 시 네이티브 모듈 호환성 확인 필수
- Firebase 라이브러리는 동일 버전 유지 (16.5.2)
- Navigation 라이브러리는 호환 버전 사용

### 업데이트 전략
1. 마이너 버전 업데이트 우선
2. 메이저 버전 업데이트는 별도 브랜치에서 테스트
3. 네이티브 모듈은 변경 로그 확인 필수

## 🎯 향후 개선 방향

1. **성능 개선**
   - Hermes 엔진 활성화 검토
   - 번들 사이즈 최적화
   - 메모리 사용량 모니터링

2. **기능 확장**
   - 오프라인 모드 지원
   - 다국어 지원 (i18n)
   - 접근성 개선 (Accessibility)

3. **코드 품질**
   - 테스트 커버리지 증가
   - E2E 테스트 도입 (Detox)
   - 코드 리뷰 프로세스 강화

4. **DevOps**
   - 자동화된 배포 파이프라인
   - 성능 모니터링 (Sentry, New Relic)
   - A/B 테스팅 플랫폼

## 📞 문의 및 지원

프로젝트 관련 문의사항이나 이슈가 있을 경우:
1. GitHub Issues 등록
2. 개발팀 Slack 채널 문의
3. 기술 문서 참조

## 📄 라이선스

Private - 모든 권리 보유

---

**마지막 업데이트**: 2025-09-30  
**작성자**: Windsurf AI Assistant  
**프로젝트 버전**: 0.0.1
