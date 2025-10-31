# Windsurf 프로젝트 분석 - Innerboard

## 📋 프로젝트 개요

**프로젝트명**: Innerboard  
**타입**: React 기반 웹 애플리케이션  
**설명**: 사내 리액트 보일러플레이트 기반으로 구축된 보드 관리 시스템  
**빌드 도구**: Webpack 5  
**패키지 매니저**: Yarn  

---

## 🏗️ 기술 스택

### Core Technologies
- **React**: 18.2.0
- **React Router DOM**: 6.14.1
- **Redux Toolkit**: 1.9.5
- **React Query (TanStack)**: 4.29.19
- **Styled Components**: 5.3.9

### 주요 라이브러리
- **상태 관리**: Redux Toolkit, Redux Thunk
- **데이터 페칭**: React Query, Axios
- **UI 컴포넌트**:
  - react-beautiful-dnd (드래그 앤 드롭)
  - react-datepicker (날짜 선택)
  - react-select (셀렉트 박스)
  - react-color (컬러 피커)
  - react-toastify (토스트 알림)
- **유틸리티**: 
  - lodash
  - moment, date-fns
  - query-string
  - react-intl (다국어 지원)

### 빌드 & 개발 도구
- **Webpack**: 5.76.0
- **Babel**: 7.20.7
- **Sass**: 1.62.0
- **Prettier**: 2.8.8

---

## 📁 프로젝트 구조

```
📦 innerboard
├─ 📁 public/              # 정적 파일 (favicon, fonts 등)
├─ 📁 dist/                # 빌드 결과물
├─ 📁 src/
│  ├─ 📁 apis/            # API 서비스 레이어
│  │  ├─ index.js        # Axios 인스턴스 및 인터셉터 설정
│  │  ├─ board.js        # 보드 관련 API
│  │  ├─ category.js     # 카테고리 API
│  │  ├─ folder.js       # 폴더 API
│  │  ├─ mall.js         # 쇼핑몰 API
│  │  ├─ payment.js      # 결제 API
│  │  ├─ product.js      # 상품 API
│  │  ├─ productGroup.js # 상품 그룹 API
│  │  ├─ download.js     # 다운로드 API
│  │  └─ script.js       # 스크립트 API
│  │
│  ├─ 📁 assets/          # 정적 리소스
│  │  ├─ images/         # 이미지 파일
│  │  └─ stylesheets/    # SCSS 파일
│  │
│  ├─ 📁 components/      # React 컴포넌트
│  │  ├─ common/         # 공통 컴포넌트
│  │  │  ├─ Checkbox.js
│  │  │  ├─ ColorPicker.js
│  │  │  ├─ CustomSelect.js
│  │  │  ├─ DatePickerInput.js
│  │  │  ├─ Input.js
│  │  │  ├─ Label.js
│  │  │  └─ Loading.js
│  │  │
│  │  ├─ modal/          # 모달 컴포넌트
│  │  │  ├─ Modal.js
│  │  │  ├─ AddEventModal.js
│  │  │  ├─ AddProductGroupModal.js
│  │  │  ├─ CopyGroupModal.js
│  │  │  ├─ ExistProductGroupModal.js
│  │  │  ├─ ExpireTrialModal.js
│  │  │  ├─ PreviewModal.js
│  │  │  ├─ ProductConnectModal.js
│  │  │  ├─ ScriptInstallModal.js
│  │  │  ├─ UserPlanInfoModal.js
│  │  │  └─ tutorial/    # 튜토리얼 모달
│  │  │
│  │  ├─ BoardManage/    # 보드 관리 컴포넌트
│  │  ├─ PrdGroup/       # 상품 그룹 컴포넌트
│  │  └─ [기타 컴포넌트들]
│  │
│  ├─ 📁 constants/       # 상수 정의
│  │
│  ├─ 📁 lang/            # 다국어 지원 파일
│  │
│  ├─ 📁 redux/           # Redux 상태 관리
│  │  ├─ store.js        # Redux Store 설정
│  │  ├─ ModalSlice.js
│  │  ├─ TipModalSlice.js
│  │  ├─ ToastSlice.js
│  │  ├─ ConfirmSlice.js
│  │  ├─ DatePickerModalSlice.js
│  │  ├─ AddEventModalSlice.js
│  │  ├─ AddProductGroupModalSlice.js
│  │  ├─ ArticleSlice.js
│  │  ├─ BoardSlice.js
│  │  ├─ ProductGroupSlice.js
│  │  └─ GlobalLoadingSlice.js
│  │
│  ├─ 📁 routes/          # 라우트 페이지
│  │  ├─ index.js        # 라우터 설정
│  │  ├─ Root.js         # 루트 레이아웃
│  │  ├─ Login.js        # 로그인 페이지
│  │  ├─ main/           # 메인 페이지
│  │  ├─ BoardManage.js  # 보드 관리 페이지
│  │  ├─ Preview.js      # 미리보기 페이지
│  │  ├─ Tutorial.js     # 튜토리얼 페이지 (Lazy Load)
│  │  └─ Error.js        # 에러 페이지
│  │
│  ├─ 📁 styles/          # 글로벌 스타일
│  │  ├─ GlobalStyle.js
│  │  ├─ theme.js
│  │  └─ _datepicker.scss
│  │
│  ├─ 📁 utils/           # 유틸리티 함수
│  │
│  ├─ 📄 App.js           # 메인 App 컴포넌트
│  └─ 📄 index.js         # 엔트리 포인트
│
├─ ⚙️ .babelrc            # Babel 설정
├─ ⚙️ .env                # 환경 변수
├─ ⚙️ .env.dev            # 개발 환경 변수
├─ ⚙️ .gitignore          # Git ignore 설정
├─ ⚙️ .gitlab-ci.yml      # GitLab CI/CD 설정
├─ ⚙️ .prettierrc         # Prettier 설정
├─ ⚙️ webpack.common.js   # Webpack 공통 설정
├─ ⚙️ webpack.dev.js      # Webpack 개발 설정
├─ ⚙️ webpack.prod.js     # Webpack 프로덕션 설정
├─ 📄 package.json        # 프로젝트 의존성
└─ 📄 README.md           # 프로젝트 문서
```

---

## 🔧 주요 기능

### 1. **라우팅 구조**
- **Root** (`/`): 메인 레이아웃
  - **Index**: 메인 페이지
  - **BoardManage** (`/board-manage`): 보드 관리
- **Preview** (`/preview`): 미리보기
- **Tutorial** (`/tutorial`): 튜토리얼 (Lazy Loading)
- **Login** (`/login`): 로그인

### 2. **상태 관리 (Redux Slices)**
- **Modal**: 모달 상태 관리
- **TipModal**: 팁 모달 관리
- **Toast**: 토스트 알림 관리
- **Confirm**: 확인 다이얼로그 관리
- **DatePicker**: 날짜 선택 모달 관리
- **AddEventModal**: 이벤트 추가 모달
- **AddProductGroupModal**: 상품 그룹 추가 모달
- **Article**: 아티클 상태
- **Board**: 보드 상태
- **ProductGroup**: 상품 그룹 상태
- **GlobalLoading**: 전역 로딩 상태

### 3. **API 통신**
- **Base URL**: `https://boardprd.innerviewit.co.kr`
- **인증**: Cookie 기반 (`ivInnerboard_auth`)
- **인터셉터**:
  - Request: Authorization 헤더 자동 추가
  - Response: 에러 핸들링 (4xx: Confirm 모달, 5xx: Toast)

### 4. **UI/UX 기능**
- 드래그 앤 드롭 (react-beautiful-dnd)
- 날짜 선택기 (react-datepicker)
- 컬러 피커 (react-color)
- 토스트 알림 (react-toastify)
- 커스텀 셀렉트 박스
- 로딩 인디케이터
- 모달 시스템
- 튜토리얼 시스템

### 5. **데이터 페칭**
- React Query를 통한 서버 상태 관리
- `refetchOnWindowFocus: false` 설정
- React Query DevTools 포함

---

## 🎨 스타일링

### 아키텍처
- **Styled Components**: 컴포넌트 레벨 스타일링
- **SCSS**: 글로벌 스타일 및 테마
- **Theme Provider**: Light 테마 적용
- **GlobalStyle**: 전역 스타일 리셋 및 기본 스타일

### 스타일 구조
```scss
src/
  styles/
    GlobalStyle.js      # 전역 스타일 (styled-components)
    theme.js            # 테마 설정
    _datepicker.scss    # DatePicker 커스텀 스타일
```

---

## 🚀 실행 방법

### 개발 모드
```bash
yarn dev
```
- 포트: 3000
- Hot Reload 활성화
- Webpack Dev Server 사용

### 프로덕션 빌드
```bash
yarn build
```
- 빌드 결과물: `dist/` 디렉토리
- 최적화된 정적 파일 생성

---

## 🔐 환경 변수

### `.env` 파일
```env
BASE_URL=https://boardprd.innerviewit.co.kr
```

### Webpack 설정
- `webpack.DefinePlugin`을 통해 환경 변수 주입
- `process.env.BASE_URL`로 접근

---

## 📦 주요 의존성

### Production Dependencies
```json
{
  "@reduxjs/toolkit": "^1.9.5",
  "@tanstack/react-query": "^4.29.19",
  "axios": "^1.4.0",
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "react-router-dom": "^6.14.1",
  "react-redux": "^8.1.1",
  "styled-components": "^5.3.9",
  "react-beautiful-dnd": "^13.1.1",
  "react-datepicker": "^4.15.0",
  "react-toastify": "^9.1.3"
}
```

### Dev Dependencies
```json
{
  "@babel/core": "^7.20.7",
  "webpack": "^5.76.0",
  "webpack-dev-server": "^4.11.1",
  "sass-loader": "^13.2.0",
  "prettier": "^2.8.8"
}
```

---

## 🔄 CI/CD

- **GitLab CI/CD**: `.gitlab-ci.yml` 설정 파일 존재
- 자동화된 빌드 및 배포 파이프라인

---

## 🎯 핵심 특징

### 1. **모듈화된 구조**
- 명확한 폴더 구조 (apis, components, redux, routes)
- 재사용 가능한 공통 컴포넌트
- 도메인별 API 분리

### 2. **강력한 상태 관리**
- Redux Toolkit을 통한 효율적인 상태 관리
- React Query를 통한 서버 상태 캐싱
- 모달, 토스트 등 UI 상태의 중앙 관리

### 3. **개발자 경험**
- Hot Reload 지원
- React Query DevTools
- Prettier를 통한 코드 포맷팅
- 명확한 에러 핸들링

### 4. **사용자 경험**
- 직관적인 드래그 앤 드롭
- 반응형 모달 시스템
- 토스트 알림을 통한 피드백
- 튜토리얼 시스템

### 5. **성능 최적화**
- Lazy Loading (Tutorial 페이지)
- Code Splitting
- React Query 캐싱
- Webpack 최적화

---

## 📝 개발 가이드라인

### 컴포넌트 작성
- Styled Components 사용
- PropTypes를 통한 타입 체크
- 공통 컴포넌트는 `components/common/`에 배치

### API 호출
- `src/apis/` 디렉토리에 도메인별 API 함수 작성
- Axios 인스턴스 사용
- 에러는 자동으로 인터셉터에서 처리

### 상태 관리
- Redux Toolkit의 Slice 패턴 사용
- 비동기 로직은 React Query 또는 Redux Thunk 사용
- UI 상태는 Redux, 서버 상태는 React Query

### 스타일링
- 컴포넌트 스타일: Styled Components
- 글로벌 스타일: SCSS
- 테마 변수 활용

---

## 🐛 에러 핸들링

### API 에러
- **4xx 에러**: Confirm 모달로 표시
- **5xx 에러**: Toast 메시지로 표시
- `skipAlert` 옵션으로 자동 알림 비활성화 가능

### 라우팅 에러
- ErrorPage 컴포넌트로 처리
- 404 및 기타 라우팅 에러 대응

---

## 🔮 향후 개선 사항

1. **TypeScript 도입**: 타입 안정성 향상
2. **테스트 코드**: Jest, React Testing Library
3. **성능 모니터링**: Lighthouse, Web Vitals
4. **접근성**: ARIA 속성, 키보드 네비게이션
5. **다국어 지원 확대**: react-intl 활용

---

## 👥 개발자 정보

- **Author**: junhyun
- **License**: ISC

---

## 📚 참고 문서

- [React 공식 문서](https://react.dev/)
- [Redux Toolkit 문서](https://redux-toolkit.js.org/)
- [React Query 문서](https://tanstack.com/query/latest)
- [Styled Components 문서](https://styled-components.com/)
- [Webpack 문서](https://webpack.js.org/)

---

**문서 작성일**: 2025-09-30  
**분석 도구**: Windsurf AI
