# Windsurf - InnerBoard API 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: InnerBoard API Server  
**버전**: 1.0.0  
**설명**: Cafe24 쇼핑몰과 연동되는 게시판 관리 API 서버

이 프로젝트는 Cafe24 쇼핑몰 플랫폼과 연동하여 게시판, 상품, 카테고리 등을 관리하는 백엔드 API 서버입니다. Node.js와 Express 프레임워크를 기반으로 구축되었으며, Cafe24 OAuth 2.0 인증을 통해 안전한 API 통신을 제공합니다.

---

## 🏗️ 기술 스택

### 백엔드 프레임워크
- **Node.js** + **Express** 4.16.1
- **Sequelize** 6.31.1 (ORM)
- **MariaDB** 3.1.2 (데이터베이스)

### 주요 라이브러리
- **axios** 1.4.0 - HTTP 클라이언트
- **jsonwebtoken** 9.0.0 - JWT 인증
- **moment** 2.29.4 - 날짜/시간 처리
- **cors** 2.8.5 - CORS 처리
- **bree** 9.1.3 - 작업 스케줄러
- **cabin** 13.2.4 - 로깅
- **morgan** 1.9.1 - HTTP 요청 로깅

### 프론트엔드 관련
- **Tailwind CSS** 3.3.2
- **DaisyUI** 3.0.0
- **Handlebars (hbs)** 4.0.4

### 개발 도구
- **PM2** - 프로세스 관리
- **ApiDoc** 1.0.3 - API 문서 자동 생성
- **GitLab CI/CD** - 배포 자동화

---

## 📁 프로젝트 구조

```
innerboard-api/
├── app.js                      # Express 애플리케이션 메인 파일
├── bin/
│   └── www                     # 서버 시작 스크립트
├── bree.js                     # 스케줄러 설정
├── bree.config.js              # 스케줄러 환경 설정
├── ecosystem.config.js         # PM2 배포 설정
├── package.json                # 프로젝트 의존성
├── apidoc.json                 # API 문서 설정
├── header.html                 # API 문서 헤더
├── .gitlab-ci.yml              # CI/CD 파이프라인
│
├── common/                     # 공통 유틸리티
│   ├── cafe24.js              # Cafe24 API 통신 모듈
│   ├── constant.js            # 상수 정의
│   ├── error.js               # 커스텀 에러 핸들러
│   └── utils.js               # 유틸리티 함수
│
├── config/                     # 설정 파일
│   └── config.js              # 데이터베이스 설정
│
├── models/                     # Sequelize 모델
│   ├── index.js               # 모델 초기화
│   ├── Article.js             # 게시글 모델
│   ├── Category.js            # 카테고리 모델
│   ├── Folder.js              # 폴더 모델
│   ├── Mall.js                # 쇼핑몰 모델
│   ├── Payment.js             # 결제 모델
│   └── ProductGroup.js        # 상품 그룹 모델
│
├── routes/                     # API 라우트
│   ├── board/                 # 게시판 관련 API
│   ├── callback/              # OAuth 콜백
│   ├── category/              # 카테고리 API
│   ├── folder/                # 폴더 API
│   ├── hook/                  # 웹훅
│   ├── index/                 # 메인 페이지
│   ├── mall/                  # 쇼핑몰 API
│   ├── payment/               # 결제 API
│   ├── product/               # 상품 API
│   └── product-group/         # 상품 그룹 API
│
├── jobs/                       # 스케줄 작업
│   ├── getPaymentInfo.js      # 결제 정보 조회
│   ├── updatePaymentStatus.js # 결제 상태 업데이트
│   └── updateTokens.js        # 토큰 갱신
│
├── views/                      # 뷰 템플릿 (Handlebars)
└── public/                     # 정적 파일
    └── apidoc/                # 생성된 API 문서
```

---

## 🔑 핵심 기능

### 1. Cafe24 OAuth 2.0 인증
- **Access Token 발급 및 관리**
- **Refresh Token 자동 갱신**
- JWT 기반 사용자 인증
- HMAC 검증 (프로덕션 환경)

### 2. 게시판 관리
- 게시판 목록 조회
- 게시물 CRUD
- 게시물과 상품 그룹 연결
- 첨부파일 관리

### 3. 상품 관리
- 상품 그룹 생성/수정/삭제
- 상품 정보 조회
- 카테고리별 상품 관리
- 상품 노출 설정 (슬라이더/그리드)

### 4. 카테고리 관리
- Cafe24 카테고리 동기화
- 카테고리 트리 구조 관리

### 5. 결제 관리
- 결제 정보 조회
- 결제 상태 업데이트
- 구독 관리

### 6. 스케줄 작업 (Bree)
- **매일 자정 (12:00 AM)**: 토큰 갱신
- **매일 새벽 1시 (1:00 AM)**: 결제 정보 조회
- **매일 새벽 2시 (2:00 AM)**: 결제 상태 업데이트

---

## 🔐 인증 시스템

### 미들웨어 인증 흐름
```javascript
1. Authorization 헤더에서 access_token 추출
2. JWT 디코딩하여 mall_id, shop_no 확인
3. DB에서 사용자 정보 조회
4. 토큰 만료 확인
   - Access Token 만료 시 Refresh Token으로 자동 갱신
5. req.user에 사용자 정보 저장
```

### 화이트리스트 경로 (인증 불필요)
- `/hook` - 웹훅
- `/callback` - OAuth 콜백
- `/manual` - 매뉴얼
- `/api` - 공개 API
- `/dev` - 개발용

---

## 🌐 API 엔드포인트

### 게시판 (Board)
- `GET /board` - 게시판 목록 조회
- `GET /board/:board_id` - 게시물 목록 조회
- `GET /board/:board_no/:article_no` - 게시물 상세 조회
- `POST /board` - 게시물 수정

### 카테고리 (Category)
- `GET /category` - 카테고리 목록 조회

### 폴더 (Folder)
- 폴더 CRUD 작업

### 상품 (Product)
- 상품 조회 및 관리

### 상품 그룹 (Product Group)
- 상품 그룹 생성/수정/삭제
- 상품 그룹 설정 관리

### 결제 (Payment)
- 결제 정보 조회
- 구독 관리

### 쇼핑몰 (Mall)
- 쇼핑몰 정보 조회/수정

---

## 🗄️ 데이터베이스 모델

### Mall (쇼핑몰)
- 쇼핑몰 기본 정보
- OAuth 토큰 정보
- 앱 만료일

### Article (게시글)
- 게시판 번호
- 게시글 번호
- 폴더 ID
- 연결 기간

### Category (카테고리)
- Cafe24 카테고리 정보
- 카테고리 계층 구조

### Folder (폴더)
- 폴더 구조 관리

### ProductGroup (상품 그룹)
- 상품 그룹 설정
- 노출 효과 옵션
- 가격 표시 패턴

### Payment (결제)
- 결제 정보
- 구독 상태

---

## 🚀 배포 환경

### 환경별 설정

#### Localhost
- **PORT**: 3005
- **HOST**: http://localhost:3005
- **DB_HOST**: 211.169.231.242

#### Development
- **HOST**: https://boardprd.innerviewit.co.kr
- **DB_HOST**: 211.169.231.242

#### QA
- **HOST**: https://innerboardtest.innerviewit.co.kr
- **DB_HOST**: 211.169.231.242

#### Production
- **HOST**: https://api-innerboard.ivmakes.com
- **APP_HOST**: https://innerboard.ivmakes.com
- **DB_HOST**: localhost
- **Server**: AWS EC2 (3.39.208.232)

### PM2 배포 명령어
```bash
# 개발 환경
npm run pm2:dev

# 프로덕션 환경
npm run pm2

# 스케줄러 시작
npm run pm2:bree
```

### GitLab CI/CD
- **트리거**: main 브랜치 푸시
- **자동 배포**: PM2 deploy 명령어 실행
- **캐싱**: node_modules 캐싱

---

## 🔧 Cafe24 API 연동

### 사용 권한 (Scopes)
- `mall.read_category` - 카테고리 조회
- `mall.read_product` - 상품 조회
- `mall.write_community` - 게시판 쓰기
- `mall.read_community` - 게시판 읽기
- `mall.read_design` - 디자인 조회
- `mall.write_application` - 앱 설치 관리
- `mall.read_application` - 앱 정보 조회

### API 버전
- **Cafe24 API Version**: 2023-03-01

### API 호출 제한 관리
- API 호출 제한 모니터링 (x-api-call-limit 헤더)
- 10회 이상 호출 시 1초 지연

---

## 📊 에러 처리

### 커스텀 에러 코드
- `AUTH` - 인증 오류
- `CAFE24` - Cafe24 API 오류
- `SERVER` - 서버 내부 오류

### 에러 응답 형식
```json
{
  "code": "ERROR_CODE",
  "message": "에러 메시지",
  "status_code": "상태 코드 (선택)",
  "data": "추가 데이터 (선택)"
}
```

---

## 📝 API 문서

### 자동 생성
```bash
npm run apidoc
```

API 문서는 `/public/apidoc/` 디렉토리에 생성되며, 웹 브라우저에서 확인할 수 있습니다.

### ApiDoc 주석 형식
각 라우트 컨트롤러에 ApiDoc 주석이 포함되어 있어 자동으로 문서가 생성됩니다.

---

## 🔄 스케줄 작업 상세

### 1. updateTokens (매일 자정)
- 모든 쇼핑몰의 Refresh Token 갱신
- 만료 예정 토큰 자동 업데이트

### 2. getPaymentInfo (매일 새벽 1시)
- 결제 정보 조회
- 구독 상태 확인

### 3. updatePaymentStatus (매일 새벽 2시)
- 결제 상태 업데이트
- 만료된 구독 처리

---

## 🛠️ 개발 가이드

### 설치
```bash
# 의존성 설치
npm install

# 또는
yarn install
```

### 실행
```bash
# 개발 모드
npm start

# PM2로 실행
npm run pm2:dev
```

### API 문서 생성
```bash
npm run apidoc
```

### 환경 변수
주요 환경 변수는 `ecosystem.config.js`와 `bree.config.js`에 정의되어 있습니다:
- `NODE_ENV` - 환경 (localhost/development/production)
- `PORT` - 서버 포트
- `HOST` - API 서버 주소
- `APP_HOST` - 프론트엔드 주소
- `CLIENTID` - Cafe24 클라이언트 ID
- `CLIENTSECRETKEY` - Cafe24 클라이언트 시크릿
- `DB_*` - 데이터베이스 설정

---

## 🔒 보안

### 인증
- JWT 기반 토큰 인증
- Cafe24 OAuth 2.0
- HMAC 검증 (프로덕션)

### CORS
- 모든 오리진 허용 (`origin: '*'`)
- 허용 메소드: GET, POST, PUT, PATCH, DELETE, OPTIONS
- Credentials 지원

### 데이터 제한
- Request Body 크기 제한: 100MB

---

## 📈 모니터링 및 로깅

### 로깅
- **Morgan**: HTTP 요청 로깅
- **Cabin**: 스케줄 작업 로깅

### 에러 핸들링
- 전역 에러 핸들러
- 커스텀 에러 클래스
- 상세한 에러 메시지

---

## 🎯 주요 특징

1. **자동 토큰 관리**: Access Token 만료 시 자동으로 Refresh Token으로 갱신
2. **동적 라우팅**: glob 패턴을 사용한 자동 라우트 등록
3. **스케줄 작업**: Bree를 활용한 정기 작업 자동화
4. **API 문서 자동화**: ApiDoc을 통한 문서 자동 생성
5. **멀티 환경 지원**: localhost, development, QA, production 환경 분리
6. **PM2 배포**: 무중단 배포 및 프로세스 관리

---

## 📞 연락처 및 링크

### 프로덕션 URL
- **API**: https://api-innerboard.ivmakes.com
- **앱**: https://innerboard.ivmakes.com

### 개발 URL
- **API**: https://boardprd.innerviewit.co.kr
- **앱**: https://boardprd.innerviewit.co.kr

### QA URL
- **API/앱**: https://innerboardtest.innerviewit.co.kr

---

## 📅 버전 정보

- **현재 버전**: 1.0.0
- **Node.js**: 권장 버전 14.x 이상
- **Cafe24 API**: 2023-03-01

---

## 🚨 주의사항

1. **환경 변수**: 민감한 정보(API Key, DB 비밀번호)는 환경 변수로 관리
2. **토큰 관리**: Refresh Token 만료 전 갱신 필요
3. **API 호출 제한**: Cafe24 API 호출 제한 준수
4. **CORS 설정**: 프로덕션에서는 특정 오리진만 허용 권장
5. **데이터베이스**: MariaDB 3306 포트 사용

---

## 🔮 향후 개선 사항

1. TypeScript 마이그레이션
2. 단위 테스트 추가
3. API 버전 관리
4. 캐싱 전략 구현
5. 로깅 시스템 고도화
6. 에러 모니터링 도구 연동 (Sentry 등)
7. API Rate Limiting 구현
8. 데이터베이스 마이그레이션 도구 도입

---

*이 문서는 2025년 9월 30일 기준으로 작성되었습니다.*
