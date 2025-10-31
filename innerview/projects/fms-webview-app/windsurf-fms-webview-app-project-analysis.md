# Windsurf 프로젝트 분석 보고서

## 📋 프로젝트 개요

**프로젝트명**: FMS User App (더푸른)  
**타입**: React Native WebView 앱  
**버전**: 1.0.9 (versionCode: 2011)  
**플랫폼**: iOS, Android  

### 식별자
- **iOS Bundle ID**: `com.fms.thepureun`
- **Android Package**: `com.fms.thepureun`

---

## 🎯 프로젝트 목적

더푸른(The Pureun) FMS(Fleet Management System) 사용자 앱을 React Native WebView로 패키징한 하이브리드 모바일 애플리케이션입니다. 웹 기반 서비스를 네이티브 앱으로 제공하며, 다양한 하드웨어 기능(NFC, BLE, QR 스캔)을 통합합니다.

---

## 🏗️ 기술 스택

### 핵심 프레임워크
- **React Native**: 0.71.7
- **React**: 18.2.0
- **Node Version**: 명시됨 (.node-version 파일)

### 주요 라이브러리

#### 1. WebView & 네비게이션
- `react-native-webview`: ^12.0.2 - 웹 콘텐츠 표시

#### 2. Firebase 통합
- `@react-native-firebase/app`: ^17.5.0
- `@react-native-firebase/messaging`: ^17.5.0 - 푸시 알림
- `@react-native-firebase/dynamic-links`: ^17.5.0 - 딥링크

#### 3. 하드웨어 기능
- `react-native-nfc-manager`: ^3.14.5 - NFC 태그 읽기
- `react-native-ble-plx`: ^2.0.3 - 블루투스 저전력 통신
- `react-native-camera`: ^4.2.1 - 카메라 접근
- `react-native-qrcode-scanner`: ^1.5.5 - QR 코드 스캔

#### 4. 권한 & 디바이스
- `react-native-permissions`: ^3.8.4 - 권한 관리
- `react-native-device-info`: ^10.6.0 - 디바이스 정보

#### 5. UI/UX
- `react-native-bootsplash`: ^4.7.1 - 스플래시 스크린
- `react-native-swipe-gestures`: ^1.0.5 - 스와이프 제스처

#### 6. 기타
- `sp-react-native-in-app-updates`: ^1.3.1 - 인앱 업데이트
- `react-native-send-intent`: ^1.3.0 - Android 인텐트 처리
- `patch-package`: ^7.0.0 - 패키지 패치 관리

---

## 📁 프로젝트 구조

```
fms-webview-app/
├── App.js                    # 메인 애플리케이션 컴포넌트
├── index.js                  # 앱 진입점
├── constants/
│   └── index.js             # 상수 정의 (URL, 플랫폼 등)
├── android/                 # Android 네이티브 코드
│   ├── app/
│   │   └── build.gradle    # Android 빌드 설정
│   └── ...
├── ios/                     # iOS 네이티브 코드
│   ├── FMSRn/
│   ├── Podfile             # iOS 의존성
│   └── GoogleService-Info.plist
├── __tests__/              # 테스트 파일
├── patches/                # 패키지 패치
├── package.json            # 프로젝트 의존성
├── firebase.json           # Firebase 설정
├── babel.config.js         # Babel 설정
├── metro.config.js         # Metro 번들러 설정
├── tsconfig.json           # TypeScript 설정
└── fmsMobile.apk          # 빌드된 APK (58MB)
```

---

## 🔑 핵심 기능 분석

### 1. WebView 통합 (`App.js`)

#### 기본 설정
```javascript
WEBVIEW_DOMAIN: "https://the-pureun.co.kr"
STARTING_ROUTE: "/userApp/login"
```

#### WebView 특징
- DOM Storage 활성화
- 쿠키 공유 (sharedCookiesEnabled)
- JavaScript 활성화
- 혼합 콘텐츠 모드 (compatibility)
- 텍스트 줌 고정 (100%)

### 2. Firebase 푸시 알림

#### 구현 기능
- **FCM 토큰 관리**: 앱 시작 시 토큰 획득 및 서버 전송
- **포어그라운드 알림**: Alert로 표시
- **백그라운드 알림**: `onNotificationOpenedApp` 처리
- **Quit 상태 알림**: `getInitialNotification` 처리

#### 토큰 전송
```javascript
POST /userApp/setToken
Body: { data: { token: fcmToken } }
```

### 3. NFC 태그 읽기

#### 기능
- NDEF 기술 사용
- 15초 타임아웃
- WebView로 태그 ID 전송: `beacon_recognized_complete : ${tag.id}`
- 에러 처리: `beacon_recognized_error`

#### 메시지 프로토콜
- **시작**: WebView → Native: `'nfc_on'`
- **종료**: Native → WebView: `'beacon_nfc_off'`

### 4. BLE (Bluetooth Low Energy) 스캔

#### 기능
- 15초 자동 스캔
- 디바이스 발견 시 로깅
- 백그라운드 리스너

#### 메시지 프로토콜
- **시작**: WebView → Native: `'ble_on'`
- **종료**: Native → WebView: `'beacon_ble_off'`

### 5. QR 코드 스캔

#### 기능
- 카메라 권한 요청
- 오버레이 스캐너 UI
- URL 파싱 및 검증
- WebView로 데이터 전송: `qrCode : ${data}`

#### 메시지 프로토콜
- **시작**: WebView → Native: `'camera_on'`
- **종료**: WebView → Native: `'camera_off'`

### 6. 권한 관리

#### Android 권한
- POST_NOTIFICATIONS
- CAMERA
- WRITE_EXTERNAL_STORAGE
- READ_EXTERNAL_STORAGE / READ_MEDIA_IMAGES (Android 13+)
- BLUETOOTH_SCAN
- BLUETOOTH_CONNECT
- ACCESS_FINE_LOCATION
- ACCESS_COARSE_LOCATION

#### iOS 권한
- 알림 (Firebase Messaging)
- BLUETOOTH_PERIPHERAL
- CAMERA
- MICROPHONE
- PHOTO_LIBRARY
- PHOTO_LIBRARY_ADD_ONLY

### 7. 네비게이션 & 백 버튼 처리

#### Android 백 버튼
- 특정 URI에서 종료 확인 다이얼로그 표시
- 그 외: WebView 히스토리 백

#### iOS 스와이프 제스처
- 화면 왼쪽 200px 이내에서 우측 스와이프 시 뒤로 가기

### 8. 딥링크 & 외부 앱 연동

#### 지원 스킴
- `http://`, `https://`: WebView에서 로드
- `intent://` (Android): SendIntent로 처리
- 기타 커스텀 스킴: Linking API로 처리

### 9. 인앱 업데이트 (주석 처리됨)

- Google Play Store 버전 체크
- Flexible 업데이트 모드
- 현재 비활성화 상태

---

## 🔄 WebView ↔ Native 통신

### Native → WebView
```javascript
webRef.current.postMessage(message)
```

**메시지 예시:**
- `beacon_recognized_complete : ${tagId}`
- `beacon_recognized_error`
- `beacon_ble_off`
- `beacon_nfc_off`
- `qrCode : ${data}`

### WebView → Native
```javascript
window.ReactNativeWebView.postMessage(message)
```

**메시지 예시:**
- `camera_on` / `camera_off`
- `ble_on` / `ble_off`
- `nfc_on` / `nfc_off`

---

## 🔧 빌드 설정

### Android (build.gradle)

```gradle
applicationId: "com.fms.thepureun"
versionCode: 2011
versionName: "1.0.9"
compileSdkVersion: rootProject.ext.compileSdkVersion
minSdkVersion: rootProject.ext.minSdkVersion
targetSdkVersion: rootProject.ext.targetSdkVersion
```

#### 주요 의존성
- Google Services Plugin (Firebase)
- Gson 2.8.5
- SwipeRefreshLayout
- Core SplashScreen

#### 서명 설정
- Debug: 기본 debug.keystore
- Release: 환경 변수로 관리
  - `MYAPP_RELEASE_STORE_FILE`
  - `MYAPP_RELEASE_KEY_PASSWORD`
  - `MYAPP_RELEASE_KEY_ALIAS`
  - `MYAPP_RELEASE_STORE_PASSWORD`

### iOS

- CocoaPods 사용
- GoogleService-Info.plist 포함
- Xcode 프로젝트: FMSRn.xcodeproj

---

## 📱 앱 실행 흐름

### 1. 앱 시작
```
index.js → App.js → RNBootSplash (1.5초)
```

### 2. 초기화 단계
1. 권한 요청 (카메라, 블루투스, 위치, 알림 등)
2. Firebase 토큰 획득
3. 알림 리스너 등록
4. NFC Manager 시작
5. 인앱 업데이트 체크 (비활성화)
6. WebView 로드: `https://the-pureun.co.kr/userApp/login`

### 3. 런타임
- WebView 네비게이션 추적
- 백 버튼/제스처 처리
- Native 기능 요청 대기 (NFC, BLE, QR)
- 푸시 알림 수신

---

## 🎨 UI/UX 특징

### 스타일
- SafeAreaView 사용 (노치 대응)
- Flex 레이아웃
- QR 스캐너 오버레이
  - iOS: 상단 25% 위치
  - Android: 전체 화면

### 제스처
- iOS: 우측 스와이프로 뒤로 가기
- Android: 하드웨어 백 버튼

### 스플래시 스크린
- 1.5초 페이드 효과
- react-native-bootsplash 사용

---

## 🔐 보안 고려사항

### 1. 네트워크 보안
- HTTPS 사용 (`the-pureun.co.kr`)
- 혼합 콘텐츠 허용 (compatibility 모드)

### 2. 데이터 보호
- 쿠키 공유 활성화
- DOM Storage 활성화

### 3. 권한 관리
- 런타임 권한 요청
- 권한 거부 시 알림 표시

### 4. 서명
- Release 빌드 서명 키 환경 변수 관리
- Debug 키스토어 포함

---

## 📊 앱 크기 & 성능

### APK 크기
- **fmsMobile.apk**: 58MB (58,064,519 bytes)

### 최적화 설정
- Proguard: 비활성화 (`enableProguardInReleaseBuilds = false`)
- 아키텍처별 APK 분리: 비활성화
- Hermes 엔진: 설정에 따라 활성화

---

## 🧪 테스트

### 테스트 설정
```json
"jest": {
  "preset": "react-native"
}
```

### 테스트 파일
- `__tests__/App-test.tsx`

---

## 🚀 빌드 & 실행 스크립트

### 개발
```bash
yarn start              # Metro 번들러 시작
yarn android            # Android 디버그 실행
yarn ios                # iOS 디버그 실행
```

### 릴리즈
```bash
yarn release:android    # Android 릴리즈 빌드 실행
yarn bundle:android     # Android 번들 생성
```

### 기타
```bash
yarn lint               # ESLint 실행
yarn test               # Jest 테스트 실행
yarn postinstall        # iOS 권한 설정
```

---

## 🐛 알려진 이슈 & 주석

### 1. GestureRecognizer
```javascript
// GestureRecognizer가 안드로이드 가상 키보드에서 오류
// → Android에서는 GestureRecognizer 미사용
```

### 2. 인앱 업데이트
```javascript
// checkInAppUpdate(); // 주석 처리됨
```

### 3. Notifee
```javascript
await notifee.requestPermission(); // import 누락 (에러 가능성)
```

### 4. 테스트 도메인
```javascript
// 여러 테스트 도메인 주석 처리
// export const WEBVIEW_DOMAIN = "https://fmstest.innerviewit.co.kr";
// export const WEBVIEW_DOMAIN = "http://innerviewit.co.kr:3011";
// export const WEBVIEW_DOMAIN = "http://192.168.0.164:3011";
// export const WEBVIEW_DOMAIN = "http://localhost:3011";
```

---

## 📝 개선 제안

### 1. 코드 품질
- [ ] TypeScript 완전 마이그레이션 (현재 .js 파일 사용)
- [ ] notifee import 추가 또는 코드 제거
- [ ] 사용하지 않는 주석 코드 정리
- [ ] 에러 핸들링 강화

### 2. 성능
- [ ] Proguard 활성화 (APK 크기 감소)
- [ ] 아키텍처별 APK 분리 고려
- [ ] Hermes 엔진 활성화 확인

### 3. 보안
- [ ] 혼합 콘텐츠 정책 검토
- [ ] API 키 환경 변수화
- [ ] 코드 난독화

### 4. 기능
- [ ] 인앱 업데이트 활성화
- [ ] 오프라인 모드 지원
- [ ] 에러 로깅 시스템 (Sentry 등)

### 5. 테스트
- [ ] E2E 테스트 추가
- [ ] 단위 테스트 커버리지 확대
- [ ] CI/CD 파이프라인 구축

---

## 🔗 외부 의존성

### 서버 엔드포인트
- **메인**: `https://the-pureun.co.kr`
- **FCM 토큰 전송**: `POST /userApp/setToken`

### Firebase 서비스
- Firebase Cloud Messaging (FCM)
- Firebase Dynamic Links
- Google Services (Android)

---

## 📄 라이선스 & 패키지

### 주요 패키지 오버라이드
```json
"overrides": {
  "react-native-qrcode-scanner": {
    "react-native-permissions": "^3.8.4"
  }
}
```

### Patches
- `patches/` 디렉토리에 패키지 패치 포함
- `patch-package`로 관리

---

## 👥 개발 환경

### 필수 도구
- Node.js (버전: .node-version 참조)
- Yarn
- Xcode (iOS)
- Android Studio (Android)
- CocoaPods (iOS)

### 환경 변수 (Android Release)
```
MYAPP_RELEASE_STORE_FILE
MYAPP_RELEASE_KEY_PASSWORD
MYAPP_RELEASE_KEY_ALIAS
MYAPP_RELEASE_STORE_PASSWORD
```

---

## 📞 연락처 & 리소스

### 앱 정보
- **앱 이름**: FMSRn (더푸른)
- **도메인**: the-pureun.co.kr
- **회사**: FMS (Fleet Management System)

---

## 📅 버전 히스토리

### 현재 버전
- **Version**: 1.0.9
- **Version Code**: 2011
- **React Native**: 0.71.7

---

## 🎯 결론

이 프로젝트는 웹 기반 FMS 서비스를 모바일 앱으로 제공하기 위한 **하이브리드 앱**입니다. React Native WebView를 중심으로 구축되었으며, NFC, BLE, QR 코드 스캔 등 다양한 네이티브 기능을 통합하여 사용자에게 풍부한 경험을 제공합니다.

### 강점
✅ 웹과 네이티브의 장점 결합  
✅ 다양한 하드웨어 기능 지원  
✅ Firebase 통합으로 푸시 알림 지원  
✅ iOS/Android 크로스 플랫폼  

### 개선 영역
⚠️ TypeScript 마이그레이션 필요  
⚠️ 코드 최적화 및 난독화  
⚠️ 테스트 커버리지 확대  
⚠️ 에러 처리 강화  

---

**문서 작성일**: 2025-09-30  
**분석 도구**: Windsurf AI  
**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/02-web-development/innerview-project/fms-webview-app`
