# Windsurf 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: SNAC-ENG CMS (Content Management System)  
**프로젝트 유형**: Node.js 기반 웹 애플리케이션  
**목적**: 영어 학습 콘텐츠 관리 시스템  
**분석 일시**: 2025-09-30

---

## 🏗️ 기술 스택

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js v4.16.1
- **Template Engine**: Handlebars (express-hbs v2.4.0)
- **ORM**: Sequelize v6.32.1
- **Database**: MariaDB v3.2.0
- **Authentication**: Passport.js (passport-local)
- **Process Manager**: PM2 (ecosystem.config.js)

### 주요 의존성
```json
{
  "axios": "^1.5.0",           // HTTP 클라이언트
  "cookie-parser": "~1.4.4",   // 쿠키 파싱
  "cors": "^2.8.5",            // CORS 처리
  "express-session": "^1.17.3", // 세션 관리
  "multer": "^1.4.5-lts.1",    // 파일 업로드
  "morgan": "~1.9.1",          // HTTP 로깅
  "striptags": "^3.2.0",       // HTML 태그 제거
  "urlencode": "^1.1.0"        // URL 인코딩
}
```

---

## 📁 프로젝트 구조

```
cms/
├── app.js                    # Express 애플리케이션 메인 파일
├── package.json              # 프로젝트 의존성 관리
├── ecosystem.config.js       # PM2 배포 설정
├── bin/
│   └── www                   # 서버 시작 스크립트
├── config/
│   ├── config.js            # 데이터베이스 설정
│   └── passport.js          # Passport 인증 설정
├── models/                   # Sequelize 데이터 모델 (65개 파일)
│   ├── index.js             # 모델 초기화
│   ├── user_*.js            # 사용자 관련 모델
│   ├── l_*.js               # 학습(Lesson) 관련 모델
│   ├── board.js             # 게시판 모델
│   ├── orders.js            # 주문 모델
│   └── ...                  # 기타 비즈니스 모델
├── routes/                   # 라우팅 파일 (12개)
│   ├── index.js             # 메인 라우트
│   ├── api.js               # API 엔드포인트 (137KB)
│   ├── lesson.js            # 레슨 관리 (56KB)
│   ├── board.js             # 게시판
│   ├── shop.js              # 쇼핑몰
│   ├── user.js              # 사용자 관리
│   ├── quiz.js              # 퀴즈
│   ├── login.js             # 로그인
│   └── ...
├── views/                    # Handlebars 템플릿 (118개 파일)
│   ├── layouts/             # 레이아웃 템플릿
│   ├── Lesson/              # 레슨 뷰 (33개)
│   ├── Board/               # 게시판 뷰 (13개)
│   ├── Quiz/                # 퀴즈 뷰 (13개)
│   ├── Shop/                # 쇼핑 뷰 (10개)
│   ├── User/                # 사용자 뷰 (7개)
│   └── ...
├── public/                   # 정적 파일 (81개)
├── helper/                   # Handlebars 헬퍼 함수
├── Common/                   # 공통 스크립트 및 리소스
└── util/                     # 유틸리티 함수
```

---

## 🔑 핵심 기능 분석

### 1. 애플리케이션 초기화 (app.js)

#### 뷰 엔진 설정
```javascript
// Handlebars 템플릿 엔진 설정
app.engine('hbs', hbs.express4({
  partialsDir: __dirname + "/views/layouts/partials",
  defaultLayout: __dirname + "/views/layouts/layout",
  layoutsDir: __dirname + "/views/layouts",
  beautify: true
}));
```

#### 미들웨어 구성
- **로깅**: Morgan (dev 모드)
- **파싱**: JSON, URL-encoded, Cookie
- **세션**: Express-session (암호화 키 사용)
- **인증**: Passport.js (세션 기반)
- **파일 업로드**: Multer

#### 동적 라우팅
```javascript
// routes 폴더의 모든 .js 파일을 자동으로 라우트로 등록
const routes = glob.sync(__dirname + '/routes/*.js');
routes.forEach(function (route) {
  var file = path.basename(route, path.extname(route));
  var router = require('./routes/' + file);
  
  if(file == 'index'){
    app.use('/', router)
  } else {
    app.use('/' + file, router)
  }
});
```

#### 인증 미들웨어
```javascript
// 개발 환경: 자동 로그인 (dev.snac, ulevel: 90)
// 프로덕션: Passport 인증 필요
app.all('*', upload.none(), function (req, res, next) {
  if (req.url.indexOf('/login') > -1 || req.url.indexOf('/uploads') > -1) {
    next();
  } else {
    // 현재는 개발 모드로 자동 인증 설정
    req.user = {idx: 1001, userid: 'dev.snac', ulevel: 90}
    res.locals.user = {idx: 1001, userid: 'dev.snac', ulevel: 90}
    next();
  }
});
```

---

### 2. 데이터베이스 설정

#### 환경별 설정 (config/config.js)
- **Development**: SNAC_DEV 데이터베이스 (원격 서버)
- **Production**: SNAC 데이터베이스 (로컬호스트)
- **공통 설정**:
  - Timezone: Asia/Seoul (+09:00)
  - Charset: UTF8MB4
  - Timestamps: 비활성화
  - Underscored: true (snake_case)

#### 데이터 모델 구조
프로젝트는 **65개의 Sequelize 모델**을 포함하며, 주요 카테고리는:

1. **사용자 관련** (user_*)
   - `user_admin.js`: 관리자 정보
   - `user_info.js`: 사용자 기본 정보
   - `user_lesson.js`: 사용자별 레슨 진행 상황

2. **레슨/학습 관련** (l_*)
   - `l_listening.js`: 리스닝 콘텐츠
   - `l_unit.js`: 학습 유닛
   - `l_card.js`: 학습 카드
   - `l_script.js`: 스크립트
   - `l_voca.js`: 어휘
   - `l_category.js`: 카테고리
   - `l_theme.js`, `l_thema.js`: 테마
   - `l_creator.js`: 콘텐츠 제작자

3. **학습 기록** (history, h_*)
   - `history.js`: 일반 학습 기록
   - `h_unit.js`: 유닛별 학습 기록
   - `h_bookmark.js`: 북마크
   - `stt_history.js`: STT 기록

4. **커머스** (shop, orders, product)
   - `orders.js`: 주문 정보
   - `product.js`: 상품
   - `o_product.js`: 주문 상품
   - `subscribe.js`: 구독
   - `coupon.js`: 쿠폰

5. **게시판/커뮤니티**
   - `board.js`: 게시판
   - `banner.js`: 배너

6. **챌린지/평가**
   - `challengr.js`: 챌린지
   - `b_evaluate.js`: 평가

7. **통계/로그**
   - `log_user.js`: 사용자 로그
   - `log_video.js`: 비디오 로그
   - `tag_stat.js`: 태그 통계

---

### 3. 라우팅 구조

#### 주요 라우트 모듈

| 라우트 | 파일 크기 | 설명 |
|--------|----------|------|
| `/` | 250 bytes | 메인 페이지 (→ /Lesson 리다이렉트) |
| `/api` | 137 KB | RESTful API 엔드포인트 |
| `/lesson` | 56 KB | 레슨 관리 (CRUD) |
| `/board` | 19 KB | 게시판 기능 |
| `/shop` | 14 KB | 쇼핑몰 기능 |
| `/quiz` | 11 KB | 퀴즈 관리 |
| `/user` | 6 KB | 사용자 관리 |
| `/login` | 798 bytes | 로그인/로그아웃 |
| `/manage` | 932 bytes | 관리 기능 |
| `/myInfo` | 572 bytes | 내 정보 |
| `/common` | 212 bytes | 공통 기능 |

#### API 라우트 특징
- `api.js`가 가장 큰 파일 (137KB)로, 대부분의 비즈니스 로직 포함
- RESTful API 설계로 프론트엔드와 통신

---

### 4. 뷰 템플릿 구조

#### Handlebars 레이아웃 시스템
- **메인 레이아웃**: `views/layouts/layout.hbs`
- **파셜**: `views/layouts/partials/`
- **페이지별 뷰**: 각 기능별 디렉토리로 구성

#### 주요 뷰 디렉토리
- `Lesson/` (33개): 레슨 관련 화면 (가장 많은 뷰)
- `Board/` (13개): 게시판 화면
- `Quiz/` (13개): 퀴즈 화면
- `Shop/` (10개): 쇼핑몰 화면
- `User/` (7개): 사용자 관리 화면

---

## 🚀 배포 설정 (PM2)

### ecosystem.config.js 분석

#### 애플리케이션 설정
```javascript
{
  name: 'CMS',
  script: './bin/www',
  instances: 1,
  autorestart: false,
  watch: false,
  ignore_watch: ["node_modules", "public", "uploads"]
}
```

#### 환경 변수 (Development)
```javascript
{
  HOST: 'https://cms.snaceng.com',
  NODE_ENV: 'development',
  TZ: 'Asia/Seoul',
  DB_HOST: '3.36.9.39',        // AWS 원격 서버
  DB_USER: 'dbworker',
  DB_PASS: '!snac_dbWork',
  DB_PORT: '53306',
  DB_NAME: 'SNAC_DEV',
  PORT: '3001'
}
```

#### 환경 변수 (Production)
```javascript
{
  HOST: 'https://cms.snaceng.com',
  NODE_ENV: 'production',
  TZ: 'Asia/Seoul',
  DB_HOST: 'localhost',        // 로컬 데이터베이스
  DB_USER: 'dbworker',
  DB_PASS: '!snac_dbWork',
  DB_PORT: '53306',
  DB_NAME: 'SNAC',
  PORT: '3001'
}
```

#### 배포 스크립트
```bash
# Production 배포 시 자동 실행
npm install && pm2 startOrRestart ecosystem.config.js --env production && pm2 save
```

---

## 🔐 인증 시스템

### Passport.js 전략
- **로컬 전략** (passport-local) 사용
- 세션 기반 인증
- 사용자 레벨(ulevel) 기반 권한 관리

### 세션 설정
```javascript
session({
  secret: "ThisSecretIsEqualToCustomKeyForEncryption.",
  resave: false,
  httpOnly: false,
  saveUninitialized: true,
  cookie: { secure: false }
})
```

### 개발 모드 자동 인증
- 개발 환경에서는 자동으로 `dev.snac` (레벨 90) 계정으로 로그인
- 프로덕션에서는 실제 인증 필요

---

## 📊 비즈니스 도메인 분석

### 1. 영어 학습 플랫폼
이 CMS는 **영어 학습 콘텐츠**를 관리하는 시스템으로, 다음 기능을 제공:

#### 학습 콘텐츠
- **리스닝**: 오디오/비디오 기반 학습
- **어휘**: 단어 학습 및 암기
- **스크립트**: 대본 및 자막
- **카드**: 플래시카드 형식 학습
- **유닛**: 학습 단위 구성

#### 학습 관리
- 사용자별 학습 진행도 추적
- 북마크 및 좋아요 기능
- 학습 기록 (history) 저장
- STT (Speech-to-Text) 기능

#### 커뮤니티
- 게시판 시스템
- 사용자 평가 및 리뷰

### 2. 커머스 기능
- 상품 판매 (레슨, 구독권 등)
- 주문 관리
- 쿠폰 시스템
- 구독 관리

### 3. 챌린지 시스템
- 학습 챌린지 생성 및 참여
- 평가 및 피드백

---

## 🛠️ 개발 환경

### 서버 실행
```bash
# 개발 모드
npm start

# PM2로 실행
pm2 start ecosystem.config.js

# PM2 프로덕션 모드
pm2 start ecosystem.config.js --env production
```

### 포트
- **개발/프로덕션**: 3001

### 데이터베이스
- **개발**: AWS 원격 서버 (3.36.9.39:53306)
- **프로덕션**: 로컬 서버 (localhost:53306)

---

## 📦 Git 서브모듈

프로젝트는 Git 서브모듈을 사용:
```bash
git submodule add git@github.com:SNAC-ENG/models.git
```

`models/` 디렉토리가 별도 저장소로 관리되어 여러 프로젝트에서 공유 가능

---

## 🎯 주요 특징

### 1. 모듈화된 구조
- 라우트 자동 등록 (glob 패턴)
- Sequelize 모델 자동 로드
- 재사용 가능한 헬퍼 함수

### 2. 확장성
- 65개의 데이터 모델로 복잡한 비즈니스 로직 지원
- 모듈식 라우팅으로 새 기능 추가 용이
- 서브모듈로 모델 공유

### 3. 보안
- Passport.js 인증
- 세션 기반 상태 관리
- 환경 변수로 민감 정보 관리

### 4. 개발 편의성
- PM2로 프로세스 관리
- 환경별 설정 분리
- 개발 모드 자동 인증

---

## 🔍 개선 제안

### 1. 보안
- [ ] 세션 시크릿을 환경 변수로 이동
- [ ] HTTPS 강제 (secure: true)
- [ ] httpOnly 쿠키 활성화
- [ ] 프로덕션에서 자동 인증 제거

### 2. 코드 품질
- [ ] API 라우트 분리 (137KB → 여러 파일)
- [ ] 에러 핸들링 강화
- [ ] 로깅 시스템 개선 (Winston 등)
- [ ] TypeScript 마이그레이션 고려

### 3. 성능
- [ ] 데이터베이스 인덱싱 최적화
- [ ] 캐싱 전략 (Redis)
- [ ] PM2 클러스터 모드 활성화
- [ ] 정적 파일 CDN 사용

### 4. 문서화
- [ ] API 문서 작성 (Swagger)
- [ ] 데이터 모델 ERD
- [ ] 배포 가이드
- [ ] 개발 환경 설정 가이드

---

## 📝 결론

**SNAC-ENG CMS**는 영어 학습 콘텐츠를 관리하는 종합 시스템으로:

✅ **장점**
- 체계적인 MVC 구조
- 풍부한 기능 (학습, 커머스, 커뮤니티)
- 모듈화된 설계
- PM2 기반 안정적인 배포

⚠️ **개선 필요**
- 보안 강화 (시크릿 관리, HTTPS)
- 대용량 파일 분리 (api.js)
- 문서화 보완
- 테스트 코드 추가

전반적으로 **잘 구조화된 Node.js 웹 애플리케이션**이며, 지속적인 개선을 통해 더욱 견고한 시스템으로 발전할 수 있습니다.

---

**분석 작성자**: Windsurf AI  
**분석 도구**: Windsurf Cascade  
**문서 버전**: 1.0  
**최종 수정일**: 2025-09-30
