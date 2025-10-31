# Windsurf 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: SNAC English (InnerView Project)  
**프로젝트 타입**: 영어 학습 플랫폼 웹 애플리케이션  
**개발 환경**: Node.js + Express.js  
**데이터베이스**: MariaDB  
**템플릿 엔진**: Handlebars (HBS)  
**배포 관리**: PM2 Ecosystem

---

## 🏗️ 기술 스택

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js v4.18.2
- **ORM**: Sequelize v6.32.1
- **Database**: MariaDB v3.2.0
- **Authentication**: Passport.js (Local Strategy, Remember-Me)
- **Session Management**: express-session v1.17.3

### Frontend
- **Template Engine**: express-hbs v2.4.0
- **View Engine**: Handlebars (HBS)
- **UI Framework**: 커스텀 레이아웃 시스템

### 주요 라이브러리
- **axios**: HTTP 클라이언트 (v1.4.0)
- **multer**: 파일 업로드 처리 (v1.4.5-lts.1)
- **nodemailer**: 이메일 발송 (v6.9.4)
- **md5**: 암호화 (v2.3.0)
- **cors**: CORS 처리 (v2.8.5)
- **browser-detect**: 브라우저/디바이스 감지 (v0.2.28)
- **bootpay**: 결제 시스템 통합

---

## 📁 프로젝트 구조

```
www/
├── app.js                    # 메인 애플리케이션 파일
├── package.json              # 의존성 관리
├── ecosystem.config.js       # PM2 배포 설정
├── bin/
│   └── www                   # 서버 시작 스크립트
├── config/
│   ├── config.js            # 데이터베이스 설정
│   └── passport.js          # 인증 설정
├── models/                   # Sequelize 모델 (65개 파일)
│   ├── index.js             # 모델 초기화
│   ├── USER_INFO.js         # 사용자 정보
│   ├── l_listening.js       # 리스닝 콘텐츠
│   ├── l_series.js          # 시리즈 정보
│   ├── SUBSCRIBE.js         # 구독 정보
│   └── ...                  # 기타 모델들
├── routes/                   # 라우터 (16개 파일)
│   ├── index.js             # 메인 라우트
│   ├── lesson.js            # 레슨 관련
│   ├── login.js             # 로그인/회원가입
│   ├── myPage.js            # 마이페이지
│   ├── shop.js              # 구독/결제
│   ├── ai.js                # AI 기능
│   ├── board.js             # 게시판
│   ├── brand.js             # 브랜드 페이지
│   ├── contents.js          # 콘텐츠 관리
│   ├── customer.js          # 고객센터
│   ├── epilogue.js          # 에필로그
│   ├── manage.js            # 관리자
│   ├── price.js             # 가격 정책
│   ├── quiz.js              # 퀴즈
│   ├── user.js              # 사용자 관리
│   └── api.js               # API 엔드포인트
├── views/                    # 템플릿 파일 (56개 파일)
│   ├── layouts/             # 레이아웃 파일
│   │   ├── layout.hbs       # 기본 레이아웃
│   │   └── partials/        # 재사용 컴포넌트
│   ├── Lesson/              # 레슨 관련 뷰
│   ├── Login/               # 로그인 관련 뷰
│   ├── Mypage/              # 마이페이지 뷰
│   └── Shop/                # 쇼핑/구독 뷰
├── public/                   # 정적 파일 (913개 파일)
│   ├── css/                 # 스타일시트
│   ├── js/                  # 클라이언트 JavaScript
│   └── images/              # 이미지 리소스
├── helper/                   # Handlebars 헬퍼
│   └── index.js
└── utils/                    # 유틸리티 함수
    ├── utils.js
    └── function.js
```

---

## 🔑 핵심 기능

### 1. 인증 및 사용자 관리
- **Passport.js 기반 인증 시스템**
  - Local Strategy (이메일/비밀번호)
  - Remember-Me 기능
  - 세션 기반 인증
- **사용자 타입**
  - 무료 사용자
  - 유료 구독 사용자 (is_paid_user)
  - 구 버전 유료 사용자 (is_old_paid_user)

### 2. 학습 콘텐츠 시스템
- **레슨 타입**
  - Today: 오늘의 학습
  - Courses: 코스별 학습
  - Themes: 테마별 학습
  - Shorts: 짧은 영상 학습
  - Vlogs: 브이로그 학습
  - Creators: 크리에이터별 콘텐츠
- **학습 기록 관리**
  - 학습 히스토리 추적
  - 출석 체크 시스템
  - 복습 기능

### 3. 구독 및 결제
- **Bootpay 결제 시스템 통합**
- **구독 관리**
  - 카드 자동결제 (card_rebill)
  - 구독 상태 관리
  - 결제 내역 조회

### 4. AI 기능
- AI 기반 학습 지원 (별도 라우트)

### 5. 커뮤니티 기능
- 게시판 시스템
- 챌린지 시스템
- 북마크 기능

---

## 🔐 보안 및 인증

### 미들웨어 체인
```javascript
1. Flash 메시지
2. Logger (Morgan)
3. JSON/URL-encoded 파싱
4. Cookie Parser
5. Static 파일 서빙
6. Session 관리
7. Passport 초기화
8. Remember-Me 인증
9. 커스텀 인증 미들웨어
```

### 인증 로직
- 로그인하지 않은 사용자 → `/login`으로 리다이렉트
- 로그인한 사용자가 `/` 또는 `/login` 접근 → `/Lesson/Today`로 리다이렉트
- 구독 상태 확인 및 `res.locals`에 주입

---

## 🌐 라우팅 시스템

### 동적 라우트 로딩
```javascript
// glob 패턴을 사용한 자동 라우트 등록
const routes = glob.sync(__dirname + '/routes/*.js');
routes.forEach(function (route) {
  var file = path.basename(route, '.js');
  var router = require('./routes/' + file);
  
  if(file == 'index'){
    app.use('/', router)
  } else {
    app.use('/' + file, router)
  }
});
```

### 주요 엔드포인트
- `/` - 메인 페이지 (로그인 시 `/Lesson/Today`로 리다이렉트)
- `/login` - 로그인/회원가입
- `/lesson/*` - 학습 관련 페이지
- `/myPage/*` - 마이페이지
- `/shop/*` - 구독/결제
- `/ai/*` - AI 기능
- `/board/*` - 게시판
- `/api/*` - REST API

---

## 💾 데이터베이스 구조

### 주요 테이블 (모델 기준)
- **USER_INFO**: 사용자 정보
- **SUBSCRIBE**: 구독 정보
- **l_listening**: 리스닝 콘텐츠
- **l_series**: 시리즈 정보
- **l_thema**: 테마 정보
- **l_creator**: 크리에이터 정보
- **l_unit**: 학습 유닛
- **history**: 학습 히스토리
- **h_bookmark**: 북마크
- **challengr**: 챌린지
- **coupon**: 쿠폰
- **board**: 게시판
- **banner**: 배너

### 데이터베이스 설정
- **Dialect**: MariaDB
- **Charset**: utf8mb4
- **Timezone**: Asia/Seoul (+09:00)
- **Timestamps**: 비활성화 (커스텀 관리)

---

## 🚀 배포 환경

### PM2 Ecosystem 설정

#### Development 환경
- **DB Host**: 3.36.9.39 (AWS)
- **DB Port**: 53306
- **DB Name**: SNAC_DEV
- **Host**: https://www.snaceng.com/

#### Production 환경
- **DB Host**: localhost
- **DB Port**: 53306
- **DB Name**: SNAC
- **Host**: https://www.snaceng.com/

### 배포 프로세스
```bash
npm install
pm2 startOrRestart ecosystem.config.js --env production
pm2 save
```

---

## 🎨 프론트엔드 구조

### 템플릿 시스템
- **Layout**: `renew3` (기본 레이아웃)
- **Partials**: 재사용 가능한 컴포넌트
- **Handlebars Helpers**: 커스텀 헬퍼 함수

### 디바이스 감지
- iOS 감지 (`is_ios`)
- 모바일 디바이스 감지 (`ismobiledevice`)
- OS 정보 (`os`)

### 전역 변수 (res.locals)
```javascript
- js_ver: JavaScript 버전
- css_ver: CSS 버전
- sep1, sep2: 구분자
- os: 운영체제
- channel_key, channel_secret: 채널 정보
- is_login: 로그인 상태
- user: 사용자 정보
- is_paid_user: 유료 사용자 여부
- is_old_paid_user: 구 버전 유료 사용자 여부
```

---

## 📊 주요 비즈니스 로직

### 1. 출석 체크 시스템
```sql
SELECT count(distinct (regdate))
FROM U_UNIT
WHERE userkey = ? 
  AND substring(regdate, 1, 7) = 'YYYY-MM'
```

### 2. 구독 상태 확인
```javascript
// 최신 구독 정보 조회
const subscription = await models.SUBSCRIBE.findOne({
  where: { userkey: req.user.idx },
  order: [['idx', 'DESC']]
});

// 구 버전 유료 사용자 확인
const oldSubscription = await models.SUBSCRIBE.findOne({
  where: {
    userkey: req.user.idx,
    status: 10,
    payment_method: { [Op.not]: 'card_rebill' }
  }
});
```

### 3. 사용자명 표시 로직
```javascript
let my_username = user.username;
if(my_username === "") {
  my_username = user.email;
  if(my_username.indexOf("@") > 0){
    my_username = my_username.substring(0, my_username.indexOf("@"));
  }
}
```

---

## 🔧 유틸리티 함수

### utils/utils.js
- `jserror()`: JavaScript 에러 처리
- `script()`: 스크립트 생성
- `enc()`: 암호화
- `dec()`: 복호화

### utils/function.js
- `refer()`: 리퍼러 처리
- `uri()`: URI 처리
- `get_today_rss()`: 오늘의 RSS 가져오기

---

## 📝 개발 가이드

### 시작하기
```bash
# 의존성 설치
npm install

# 개발 서버 실행
npm start

# PM2로 실행 (개발 환경)
pm2 start ecosystem.config.js

# PM2로 실행 (프로덕션 환경)
pm2 start ecosystem.config.js --env production
```

### 환경 변수
```
NODE_ENV=development|production
TZ=Asia/Seoul
DB_HOST=데이터베이스 호스트
DB_USER=데이터베이스 사용자
DB_PASS=데이터베이스 비밀번호
DB_PORT=데이터베이스 포트
DB_NAME=데이터베이스 이름
BOOTPAY_APP_ID=부트페이 앱 ID
BOOTPAY_PRIVATE_KEY=부트페이 비밀키
```

### 새 라우트 추가
1. `/routes` 디렉토리에 `{name}.js` 파일 생성
2. Express Router 작성
3. 자동으로 `/{name}` 경로에 등록됨

### 새 모델 추가
1. `/models` 디렉토리에 `{model_name}.js` 파일 생성
2. Sequelize 모델 정의
3. 자동으로 `models` 객체에 등록됨

---

## 🐛 알려진 이슈 및 개선 사항

### 보안
- ⚠️ **세션 시크릿이 하드코딩됨**: 환경 변수로 이동 필요
- ⚠️ **결제 키가 코드에 노출됨**: 환경 변수로 이동 필요
- ⚠️ **채널 키/시크릿이 하드코딩됨**: 환경 변수로 이동 필요

### 성능
- 📌 주석 처리된 테스트 코드 정리 필요 (app.js 58-68줄)
- 📌 불필요한 console.log 제거 필요

### 코드 품질
- 📌 에러 핸들링 개선 필요
- 📌 TypeScript 마이그레이션 고려
- 📌 API 문서화 필요

---

## 📞 연락처 및 리소스

- **호스트**: https://www.snaceng.com/
- **데이터베이스**: MariaDB (AWS 3.36.9.39:53306)
- **Git Submodule**: models (git@github.com:SNAC-ENG/models.git)

---

## 📅 문서 정보

- **작성일**: 2025-10-01
- **작성자**: Windsurf AI
- **버전**: 1.0.0
- **마지막 업데이트**: 2025-10-01

---

## 🎯 결론

이 프로젝트는 **영어 학습 플랫폼**으로, Express.js 기반의 전통적인 MVC 패턴을 따르는 웹 애플리케이션입니다. 

### 주요 특징
✅ 체계적인 폴더 구조  
✅ Sequelize ORM을 통한 데이터베이스 관리  
✅ Passport.js 기반 인증 시스템  
✅ 동적 라우트 로딩 시스템  
✅ PM2를 통한 프로세스 관리  
✅ Handlebars 템플릿 엔진  
✅ 구독 기반 비즈니스 모델  

### 개선 권장 사항
🔧 환경 변수 관리 강화  
🔧 에러 핸들링 개선  
🔧 API 문서화  
🔧 테스트 코드 작성  
🔧 TypeScript 마이그레이션 검토  
