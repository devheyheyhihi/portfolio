# Windsurf 프로젝트 분석 - Annual (연차 관리 시스템)

## 📋 프로젝트 개요

**프로젝트명**: Annual (연차 관리 시스템)  
**버전**: 0.0.0  
**타입**: Private  
**저장소**: https://git.innerviewit.com/innerviewit/annual.git

이 프로젝트는 기업 내부의 연차, 휴가, 회의실 예약, 요가 수업 예약 등을 관리하는 종합 인사 관리 시스템입니다.

---

## 🏗️ 기술 스택

### 프론트엔드 프레임워크
- **React 18.2.0** - 메인 UI 라이브러리
- **Vite 4.3.2** - 빌드 도구 및 개발 서버
- **React Router DOM 6.11.2** - 라우팅

### 상태 관리
- **Redux Toolkit 1.9.5** - 전역 상태 관리
- **React Redux 8.0.5** - React-Redux 바인딩
- **Redux Persist 6.0.0** - 상태 영속화
- **SWR 2.1.5** - 데이터 페칭 및 캐싱

### UI 라이브러리 & 스타일링
- **Tailwind CSS** - 유틸리티 기반 CSS 프레임워크
- **DaisyUI 3.1.5** - Tailwind CSS 컴포넌트 라이브러리
- **Theme Change 2.5.0** - 다크/라이트 모드 전환
- **Heroicons React 2.0.18** - 아이콘 라이브러리

### 차트 & 시각화
- **Chart.js 4.3.0** - 차트 라이브러리
- **React ChartJS 2 5.2.0** - React용 Chart.js 래퍼

### 날짜 & 시간
- **Day.js 1.11.7** - 날짜 처리
- **Moment 2.29.4** - 날짜/시간 포맷팅
- **React Calendar 4.6.0** - 캘린더 컴포넌트
- **React Tailwindcss Datepicker 1.6.1** - 날짜 선택기

### 파일 업로드
- **FilePond 4.30.4** - 파일 업로드 라이브러리
- **React FilePond 7.1.2** - React용 FilePond
- 다양한 FilePond 플러그인:
  - 파일 인코딩
  - 파일 검증 (크기, 타입)
  - 이미지 크롭, 편집, 미리보기
  - 이미지 리사이즈, 변환

### 기타 라이브러리
- **Axios 1.4.0** - HTTP 클라이언트
- **React Notifications 1.7.4** - 알림 시스템
- **React CSV 2.2.2** - CSV 내보내기
- **React Color 2.19.3** - 컬러 피커
- **jQuery 3.7.0** - DOM 조작

### 개발 도구
- **ESLint** - 코드 린팅
- **Autoprefixer** - CSS 벤더 프리픽스 자동 추가
- **Redux DevTools** - Redux 디버깅

---

## 📁 프로젝트 구조

```
annual/
├── public/                      # 정적 파일
│   ├── favicon.ico
│   ├── logo.png
│   ├── intro.png
│   ├── default.webp
│   └── manifest.json
│
├── src/
│   ├── app/                     # 앱 초기화 및 인증
│   │   ├── auth.js             # 인증 체크
│   │   └── init.js             # 앱 초기화
│   │
│   ├── components/              # 재사용 가능한 컴포넌트
│   │
│   ├── containers/              # 레이아웃 컨테이너
│   │   └── Layout.jsx          # 메인 레이아웃
│   │
│   ├── features/                # 기능별 모듈
│   │   ├── annual/             # 연차 관리
│   │   ├── calendar/           # 캘린더
│   │   ├── charts/             # 차트
│   │   ├── common/             # 공통 기능
│   │   ├── dashboard/          # 대시보드
│   │   ├── expenses/           # 경비 관리
│   │   ├── expensesList/       # 경비 목록
│   │   ├── integration/        # 통합 기능
│   │   ├── list/               # 리스트 뷰
│   │   ├── meetingRoom/        # 회의실 예약
│   │   ├── settings/           # 설정
│   │   ├── transactions/       # 트랜잭션
│   │   ├── user/               # 사용자 관리
│   │   └── yoga/               # 요가 예약
│   │
│   ├── pages/                   # 페이지 컴포넌트
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── RegisterCompany.jsx
│   │   ├── ForgotPassword.jsx
│   │   ├── ResetPassword.jsx
│   │   └── protected/          # 인증 필요 페이지
│   │       ├── Dashboard.jsx
│   │       ├── Annual.jsx
│   │       ├── Calendar.jsx
│   │       ├── MeetingRoom.jsx
│   │       ├── Yoga.jsx
│   │       └── ...
│   │
│   ├── routes/                  # 라우팅 설정
│   │   └── index.jsx
│   │
│   ├── App.jsx                  # 메인 앱 컴포넌트
│   ├── index.jsx                # 엔트리 포인트
│   └── index.css                # 글로벌 스타일
│
├── .gitlab-ci.yml               # GitLab CI/CD 설정
├── vite.config.js               # Vite 설정
├── tailwind.config.js           # Tailwind CSS 설정
├── package.json                 # 의존성 관리
└── README.md                    # 프로젝트 문서
```

---

## 🎯 주요 기능

### 1. 인증 시스템
- 로그인 / 로그아웃
- 회원가입 (개인 / 회사)
- 비밀번호 찾기 / 재설정
- 토큰 기반 인증

### 2. 연차 관리
연차 타입:
- **연차** (allday) - 전일 휴가
- **월차** (month) - 월차
- **오전 반차** (am) - 오전 반차
- **오후 반차** (pm) - 오후 반차
- **경조사** (event) - 경조사 휴가
- **병가** (sick) - 병가
- **기타** (etc) - 기타

연차 상태:
- **신청** (pending) - 휴가 신청
- **1차 승인** (approved) - 승인됨
- **반려** (rejected) - 반려됨
- **완료** (checked) - 처리 완료
- **취소** (cancelled) - 취소됨

### 3. 캘린더
- 연차 일정 표시
- 회의실 예약 일정
- 요가 수업 일정
- 이벤트 색상 구분 (파랑, 초록, 보라, 주황, 핑크)

### 4. 회의실 예약
- 회의실 예약 시스템
- 예약 현황 확인
- 예약 관리

### 5. 요가 수업 예약
- 요가 수업 예약
- 예약 목록 확인

### 6. 대시보드
- 연차 사용 현황 통계
- 차트 및 시각화
- 주요 지표 표시

### 7. 경비 관리
- 경비 신청
- 경비 목록 조회
- 경비 승인/반려

### 8. 사용자 관리
- 사용자 목록
- 부서(Part) 관리
- 팀(Team) 관리
- 프로필 설정

### 9. 기타 기능
- 다크/라이트 모드 전환
- 알림 시스템
- CSV 내보내기
- 파일 업로드 (이미지 크롭, 리사이즈)

---

## 🔧 설정 파일

### Vite Config (vite.config.js)
프로젝트에서 사용하는 전역 상수들을 정의:
- `MODAL_BODY_TYPES` - 모달 타입 정의
- `ANNUAL_TYPE` - 연차 타입 정의
- `ANNUAL_STATUS` - 연차 상태 정의
- `RIGHT_DRAWER_TYPES` - 우측 드로어 타입
- `CALENDAR_EVENT_STYLE` - 캘린더 이벤트 스타일

### Tailwind Config
- 다크 모드 지원 (`data-theme="dark"`)
- DaisyUI 플러그인 사용
- Typography 플러그인 사용
- 라이트/다크 테마

---

## 🚀 실행 방법

### 개발 서버 실행
```bash
npm run dev
# 또는
yarn dev
```

### 프로덕션 빌드
```bash
npm run build
# 또는
yarn build
```

### 빌드 미리보기
```bash
npm run preview
# 또는
yarn preview
```

### 린팅
```bash
npm run lint
# 또는
yarn lint
```

---

## 🔄 CI/CD

### GitLab CI/CD 파이프라인
- **트리거**: main 브랜치에 커밋 시 자동 배포
- **캐싱**: `node_modules` 캐싱으로 빌드 속도 향상
- **빌드**: `npm run build`로 프로덕션 빌드
- **배포**: SCP를 통해 서버(`211.169.231.242`)의 `/var/www/annual`로 배포

배포 스크립트:
```yaml
script:
  - npm install
  - npm run build
  - scp -r dist/* innerview@211.169.231.242:/var/www/annual
```

---

## 🎨 UI/UX 특징

### 디자인 시스템
- **Tailwind CSS** 기반 유틸리티 클래스 사용
- **DaisyUI** 컴포넌트로 일관된 디자인
- **다크 모드** 지원으로 사용자 경험 향상
- **반응형 디자인** 지원

### 컴포넌트 구조
- **Lazy Loading**: React.lazy()를 사용한 코드 스플리팅
- **모듈화**: 기능별로 분리된 features 디렉토리
- **재사용성**: 공통 컴포넌트 분리

---

## 🔐 인증 흐름

1. 사용자가 로그인 페이지 접속
2. 로그인 성공 시 토큰 발급
3. `checkAuth()` 함수로 토큰 검증
4. 토큰이 유효하면 `/app/dashboard`로 리다이렉트
5. 토큰이 없으면 `/login`으로 리다이렉트

```jsx
<Route path="*" element={<Navigate to={token ? "/app/dashboard" : "/login"} replace/>}/>
```

---

## 📊 라우팅 구조

### 공개 라우트
- `/login` - 로그인
- `/register` - 회원가입
- `/register-company` - 회사 등록
- `/forgot-password` - 비밀번호 찾기
- `/reset-password/:token` - 비밀번호 재설정

### 보호된 라우트 (`/app/*`)
- `/app/dashboard` - 대시보드
- `/app/welcome` - 환영 페이지
- `/app/annual` - 연차 관리
- `/app/calendar` - 캘린더
- `/app/meetingRoom` - 회의실 예약
- `/app/yoga` - 요가 예약
- `/app/expenses` - 경비 신청
- `/app/expensesList` - 경비 목록
- `/app/list` - 리스트
- `/app/charts` - 차트
- `/app/transactions` - 트랜잭션
- `/app/settings-profile` - 프로필 설정
- `/app/settings-user` - 사용자 관리
- `/app/settings-part` - 부서 관리
- `/app/settings-team` - 팀 관리
- `/app/integration` - 통합
- `/app/404` - 404 페이지
- `/app/blank` - 빈 페이지

---

## 🛠️ 개발 가이드

### 새로운 기능 추가하기

1. **Feature 생성**: `src/features/[기능명]/` 디렉토리 생성
2. **페이지 생성**: `src/pages/protected/[페이지명].jsx` 생성
3. **라우트 추가**: `src/routes/index.jsx`에 라우트 추가
4. **컴포넌트 개발**: 필요한 컴포넌트 개발
5. **상태 관리**: Redux slice 생성 (필요시)

### 코드 스타일
- ESLint 규칙 준수
- React Hooks 사용
- 함수형 컴포넌트 사용
- Lazy loading 적용

---

## 📦 주요 의존성 버전

| 패키지 | 버전 | 용도 |
|--------|------|------|
| React | 18.2.0 | UI 라이브러리 |
| Vite | 4.3.2 | 빌드 도구 |
| Redux Toolkit | 1.9.5 | 상태 관리 |
| React Router | 6.11.2 | 라우팅 |
| Tailwind CSS | - | 스타일링 |
| DaisyUI | 3.1.5 | UI 컴포넌트 |
| Axios | 1.4.0 | HTTP 클라이언트 |
| Chart.js | 4.3.0 | 차트 |
| Day.js | 1.11.7 | 날짜 처리 |

---

## 🌟 프로젝트 특징

### 장점
1. **모던 스택**: React 18, Vite, Tailwind CSS 등 최신 기술 사용
2. **모듈화**: 기능별로 잘 분리된 구조
3. **타입 안정성**: Redux Toolkit 사용으로 예측 가능한 상태 관리
4. **성능 최적화**: Lazy loading, SWR 캐싱
5. **UI/UX**: DaisyUI로 일관된 디자인, 다크 모드 지원
6. **자동화**: GitLab CI/CD로 자동 배포

### 개선 가능한 부분
1. **TypeScript 도입**: 타입 안정성 강화
2. **테스트 코드**: 단위 테스트, 통합 테스트 추가
3. **문서화**: API 문서, 컴포넌트 문서 보강
4. **성능 모니터링**: 성능 메트릭 수집 및 분석
5. **에러 처리**: 전역 에러 바운더리 추가

---

## 📝 추가 정보

### 환경 변수
- `.env.production` - 프로덕션 환경 변수

### 브라우저 지원
- 모던 브라우저 (Chrome, Firefox, Safari, Edge 최신 버전)

### 라이선스
- Private 프로젝트

---

## 👥 팀 정보

**조직**: InnerviewIT  
**프로젝트**: Annual (연차 관리 시스템)  
**저장소**: GitLab (git.innerviewit.com)

---

## 📞 문의

프로젝트 관련 문의사항이 있으시면 InnerviewIT 팀에 문의해주세요.

---

**문서 작성일**: 2025-09-30  
**작성자**: Windsurf AI Assistant  
**버전**: 1.0.0
