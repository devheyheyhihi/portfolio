# SNAC 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: SNAC (영어 학습 플랫폼)  
**프로젝트 타입**: Node.js Express 웹 애플리케이션  
**데이터베이스**: MariaDB  
**뷰 엔진**: Handlebars (HBS)  
**인증**: Passport.js (Local Strategy)

---

## 🏗️ 프로젝트 구조

```
snac/
├── app.js                      # Express 애플리케이션 메인 파일
├── package.json                # 프로젝트 의존성 관리
├── ecosystem.config.js         # PM2 배포 설정
├── bin/
│   └── www                     # 서버 시작 스크립트
├── config/
│   ├── config.js              # 데이터베이스 설정
│   └── passport.js            # Passport 인증 전략 설정
├── models/                     # Sequelize 모델 (65개 파일)
│   ├── index.js               # 모델 초기화
│   ├── user_info.js           # 사용자 정보 모델
│   ├── lesson.js              # 레슨 모델
│   ├── subscribe.js           # 구독 모델
│   ├── orders.js              # 주문 모델
│   └── ...                    # 기타 모델들
├── routes/                     # 라우트 핸들러
│   ├── index.js               # 메인 라우트
│   ├── api.js                 # API 라우트
│   ├── login.js               # 로그인 라우트
│   ├── lesson.js              # 레슨 라우트
│   ├── customer.js            # 고객 라우트
│   ├── myPage.js              # 마이페이지 라우트
│   ├── common.js              # 공통 라우트
│   └── cms/                   # CMS 관리자 라우트
│       ├── index.js
│       ├── lesson.js
│       ├── user.js
│       ├── board.js
│       ├── shop.js
│       ├── quiz.js
│       ├── manage.js
│       ├── api.js
│       ├── login.js
│       └── common.js
└── views/                      # Handlebars 템플릿
    ├── index.hbs
    ├── error.hbs
    └── layouts/               # 레이아웃 파일
```

---

## 🔧 기술 스택

### Backend
- **Node.js**: JavaScript 런타임
- **Express.js**: 웹 프레임워크 (v4.16.1)
- **Sequelize**: ORM (v6.32.1)
- **MariaDB**: 관계형 데이터베이스 (v3.2.0)

### 인증 & 세션
- **Passport.js**: 인증 미들웨어 (v0.6.0)
- **Passport-local**: 로컬 인증 전략 (v1.0.0)
- **Express-session**: 세션 관리 (v1.17.3)
- **Cookie-parser**: 쿠키 파싱 (v1.4.4)

### 뷰 & 템플릿
- **Express-hbs**: Handlebars 템플릿 엔진 (v2.4.0)
- **hbshelper**: Handlebars 헬퍼 (커스텀 패키지)

### 기타
- **CORS**: Cross-Origin Resource Sharing (v2.8.5)
- **Morgan**: HTTP 요청 로거 (v1.9.1)
- **Express-flash**: 플래시 메시지 (v0.0.2)
- **Glob**: 파일 패턴 매칭 (v10.3.3)

---

## 💾 데이터베이스 구조

### 주요 테이블 (모델 기반)

#### 1. 사용자 관련
- **USER_INFO**: 사용자 정보
  - 필드: userid, email, password, username, provider, mobile, userthumb, address 등
- **user_admin**: 관리자 정보
- **user_lesson**: 사용자별 레슨 진행 상태

#### 2. 레슨 & 학습 콘텐츠
- **LESSON**: 레슨 메인 정보
  - 필드: title, desc, img, tag, level, themakey, creatorkey 등
- **l_unit**: 레슨 유닛
- **l_script**: 레슨 스크립트
- **l_category**: 레슨 카테고리
- **l_series**: 레슨 시리즈
- **l_thema**: 레슨 테마
- **l_creator**: 레슨 제작자
- **l_voca**: 어휘
- **l_card**: 카드 학습
- **l_listening**: 리스닝 콘텐츠
- **l_simple**: 간단 학습

#### 3. 학습 기록
- **history**: 학습 이력
- **h_unit**: 유닛별 학습 기록
- **h_bookmark**: 북마크
- **s_history**: 학습 히스토리
- **t_history**: 테스트 히스토리
- **log_user**: 사용자 로그
- **log_video**: 비디오 시청 로그
- **stt_history**: STT(음성인식) 기록

#### 4. 구독 & 결제
- **subscribe**: 구독 정보
- **orders**: 주문 정보
- **product**: 상품 정보
- **o_product**: 주문 상품
- **coupon**: 쿠폰

#### 5. 학습 계획 & 챌린지
- **myc_plan**: 나의 학습 계획
- **myc_plan_day**: 일별 학습 계획
- **myc_user_schedule**: 사용자 스케줄
- **challengr**: 챌린지
- **c_detail**: 챌린지 상세
- **c_history**: 챌린지 이력

#### 6. 기타
- **board**: 게시판
- **banner**: 배너
- **push**: 푸시 알림
- **setting**: 설정
- **today**: 오늘의 학습
- **tag_stat**: 태그 통계

---

## 🔐 인증 시스템

### Passport.js 로컬 전략
- **로그인 방식**: username/password 기반
- **세션 관리**: Express-session 사용
- **Remember Me**: 토큰 기반 자동 로그인 (코드에 구현되어 있으나 주석 처리됨)

### 인증 흐름
1. 사용자가 username/password 입력
2. Passport Local Strategy로 USER_INFO 테이블 조회
3. 비밀번호 검증 (평문 비교 - **보안 개선 필요**)
4. 세션에 user_id와 status 저장
5. 이후 요청에서 세션 기반 인증

---

## 🚀 배포 설정 (PM2)

### 환경 변수
**Development**
- DB_HOST: localhost
- DB_USER: dbworker
- DB_PASS: !snac_dbWork
- DB_PORT: 3306
- DB_NAME: SNAC
- TZ: Asia/Seoul

**Production**
- DB_PORT: 53306 (다른 포트 사용)
- 기타 동일

### PM2 설정
- **Script**: ./bin/www
- **Instances**: 1
- **Autorestart**: false
- **Watch**: false
- **Ignore Watch**: node_modules, public, uploads

---

## 📂 라우팅 구조

### 자동 라우팅
`app.js`에서 glob 패턴을 사용하여 `/routes/*.js` 파일을 자동으로 로드:
- `index.js` → `/`
- `api.js` → `/api`
- `login.js` → `/login`
- `lesson.js` → `/lesson`
- `customer.js` → `/customer`
- `myPage.js` → `/myPage`
- `common.js` → `/common`

### CMS 관리자 라우트
- `/cms/` - CMS 메인
- `/cms/lesson` - 레슨 관리
- `/cms/user` - 사용자 관리
- `/cms/board` - 게시판 관리
- `/cms/shop` - 쇼핑 관리
- `/cms/quiz` - 퀴즈 관리
- `/cms/manage` - 시스템 관리

---

## 🎨 뷰 템플릿 구조

### Handlebars 설정
- **Views 디렉토리**: `views/`
- **Layouts 디렉토리**: `views/layouts/`
- **Partials 디렉토리**: `views/layouts/partials/`
- **Default Layout**: `views/layouts/layout`
- **Beautify**: true (HTML 포맷팅 활성화)

---

## 🔍 주요 기능 분석

### 1. 영어 학습 플랫폼
- 다양한 레슨 타입 지원 (리스닝, 어휘, 카드, 간단 학습 등)
- 테마별, 카테고리별, 시리즈별 학습 콘텐츠 구성
- 레벨별 학습 콘텐츠 제공

### 2. 학습 관리
- 학습 진행 상태 추적
- 북마크 기능
- 학습 히스토리 기록
- 비디오 시청 로그
- STT(음성인식) 학습 지원

### 3. 개인화 학습
- 나만의 학습 계획 수립
- 일별 학습 스케줄
- 챌린지 참여
- 학습 통계 및 분석

### 4. 구독 & 결제
- 구독 기반 서비스
- 상품 구매 시스템
- 쿠폰 시스템

### 5. CMS 관리자 기능
- 레슨 콘텐츠 관리
- 사용자 관리
- 게시판 관리
- 쇼핑몰 관리
- 퀴즈 관리

---

## ⚠️ 보안 고려사항

### 현재 발견된 보안 이슈
1. **비밀번호 평문 비교**
   - `config/passport.js` 76번 줄: `user.password != password`
   - **권장**: bcrypt 등을 사용한 해시 비교

2. **환경 변수 관리**
   - `ecosystem.config.js`에 DB 자격증명이 하드코딩됨
   - **권장**: `.env` 파일 사용 및 `.gitignore`에 추가

3. **세션 보안**
   - `secret`이 환경변수로 관리되나 `cookie.secure: false`
   - **권장**: HTTPS 환경에서 `secure: true` 설정

---

## 📦 의존성 관리

### 주요 의존성
```json
{
  "express": "~4.16.1",
  "sequelize": "^6.32.1",
  "mariadb": "^3.2.0",
  "passport": "^0.6.0",
  "passport-local": "^1.0.0",
  "express-session": "^1.17.3",
  "express-hbs": "^2.4.0",
  "cors": "^2.8.5",
  "glob": "^10.3.3"
}
```

---

## 🚦 시작하기

### 설치
```bash
npm install
```

### 개발 모드 실행
```bash
npm start
```

### PM2로 실행
```bash
pm2 start ecosystem.config.js
```

### PM2로 프로덕션 실행
```bash
pm2 start ecosystem.config.js --env production
```

---

## 📊 데이터베이스 설정

### MariaDB 연결 정보
- **호스트**: localhost
- **포트**: 3306 (개발), 53306 (프로덕션)
- **데이터베이스**: SNAC
- **사용자**: dbworker
- **타임존**: Asia/Seoul (+09:00)
- **문자셋**: utf8mb4

### Sequelize 설정
- **Timestamps**: false (자동 타임스탬프 비활성화)
- **Underscored**: true (스네이크 케이스 사용)
- **Date Strings**: true
- **Type Cast**: true

---

## 🎯 개선 제안

### 1. 보안 강화
- [ ] 비밀번호 해싱 (bcrypt 도입)
- [ ] 환경변수 관리 개선 (.env 파일 사용)
- [ ] HTTPS 적용 및 secure cookie 설정
- [ ] SQL Injection 방지 검증
- [ ] XSS 방지 검증

### 2. 코드 품질
- [ ] ESLint 설정 추가
- [ ] 코드 포맷터 (Prettier) 도입
- [ ] 에러 핸들링 개선
- [ ] 로깅 시스템 개선 (Winston 등)

### 3. 테스트
- [ ] 유닛 테스트 추가 (Jest, Mocha)
- [ ] 통합 테스트 추가
- [ ] API 테스트 추가

### 4. 문서화
- [ ] API 문서화 (Swagger)
- [ ] 코드 주석 추가
- [ ] 개발자 가이드 작성

### 5. 성능 최적화
- [ ] 데이터베이스 쿼리 최적화
- [ ] 캐싱 전략 수립 (Redis)
- [ ] CDN 활용
- [ ] 이미지 최적화

### 6. 모니터링
- [ ] APM 도구 도입 (New Relic, DataDog)
- [ ] 에러 트래킹 (Sentry)
- [ ] 로그 수집 및 분석

---

## 📝 참고사항

- 프로젝트는 영어 학습 플랫폼 "SNAC"으로 추정됨
- 65개의 데이터베이스 모델로 구성된 대규모 애플리케이션
- CMS 관리자 기능이 별도로 구현되어 있음
- 구독 기반 비즈니스 모델 지원
- 학습 진행 상태 추적 및 통계 기능 포함

---

**문서 작성일**: 2025-10-01  
**분석 도구**: Windsurf AI  
**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/02-web-development/innerview-project/snac`
