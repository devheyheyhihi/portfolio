# Windsurf 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Thermos Gift Management System  
**버전**: 2023-03-01  
**목적**: Cafe24 쇼핑몰과 연동하여 선물 주문을 관리하는 웹 애플리케이션  
**개발사**: Innerview IT  
**클라이언트**: Thermos Korea  

---

## 🏗️ 기술 스택

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js 4.17.2
- **Template Engine**: Handlebars (express-hbs 2.4.0)
- **ORM**: Sequelize 6.16.1
- **Database**: MariaDB 2.5.5
- **인증**: Passport.js (Local Strategy)
- **세션 관리**: express-session 1.17.2

### 주요 라이브러리
- **스케줄링**: Bree 9.1.3, node-schedule 2.1.0
- **로깅**: Winston 3.8.2, winston-daily-rotate-file 4.7.1
- **HTTP 클라이언트**: Axios 0.26.1
- **파일 업로드**: Multer 1.4.4, multer-s3 2.10.0
- **AWS 연동**: aws-sdk 2.1171.0
- **암호화**: bcrypt 5.1.0, crypto-js 4.1.1
- **날짜 처리**: Moment.js 2.29.1
- **이메일**: Nodemailer 6.9.3

### 프로세스 관리
- **PM2**: 5.2.2 (프로덕션 환경 프로세스 관리)

### Frontend
- **UI 라이브러리**: DataTables.net 1.13.3
- **스타일**: Custom CSS (JUI, Database, Review 스타일)

---

## 📁 프로젝트 구조

```
gift/
├── app.js                    # Express 애플리케이션 메인 설정
├── bin/
│   └── www                   # 서버 시작 스크립트
├── bree.js                   # Bree 스케줄러 설정
├── bree.config.js            # Bree 환경 설정
├── ecosystem.config.js       # PM2 배포 설정
├── config/
│   ├── config.js            # 데이터베이스 설정
│   ├── global.js            # 전역 설정
│   ├── passport.js          # Passport 인증 설정
│   └── winston.js           # 로깅 설정
├── models/                   # Sequelize 모델
│   ├── index.js
│   ├── activate.js
│   ├── activateLog.js
│   ├── crm.js
│   ├── crmInterest.js
│   ├── giftOrder.js         # 선물 주문 모델
│   ├── mall.js              # 쇼핑몰 정보 모델
│   ├── order.js
│   └── product.js
├── routes/                   # Express 라우터
│   ├── index.js             # 메인 라우트 (OAuth 인증)
│   ├── gift.js              # 선물 관리 라우트
│   ├── callback.js          # OAuth 콜백
│   ├── crm.js
│   ├── delivery_info.js
│   ├── deny_gift.js
│   ├── gift_check.js
│   ├── gift_delivery.js
│   ├── gift_message.js
│   └── hook.js              # Webhook 처리
├── jobs/                     # 백그라운드 작업
│   ├── cronJob.js
│   └── shippingStatusJob.js # 배송 상태 동기화
├── utils/                    # 유틸리티 함수
│   ├── alertTalk.js         # 알림톡 발송
│   ├── auth.js              # 인증 헬퍼
│   ├── cafe24.js            # Cafe24 API 연동
│   └── schedule.js          # 스케줄 관리
├── views/                    # Handlebars 템플릿
│   ├── admin/
│   │   └── gift.hbs
│   ├── alertTalk/
│   │   ├── delivery_info.hbs
│   │   ├── gift_delivery.hbs
│   │   ├── gift_deny.hbs
│   │   └── gift_message.hbs
│   ├── layouts/
│   │   ├── layout.hbs
│   │   └── partials/
│   └── index.hbs
├── public/                   # 정적 파일
├── log/                      # 로그 파일
└── package.json
```

---

## 🔑 핵심 기능

### 1. Cafe24 OAuth 인증
- **위치**: `routes/index.js`
- **기능**:
  - Cafe24 쇼핑몰과 OAuth 2.0 인증
  - Access Token 및 Refresh Token 관리
  - 토큰 자동 갱신 (expires_at 체크)
  - HMAC 기반 요청 검증

### 2. 선물 주문 관리
- **위치**: `routes/gift.js`
- **주요 기능**:
  - 선물 주문 목록 조회 (DataTables 연동)
  - 배송지 주소 변경
  - 주문 상태 필터링 (작성완료/미작성)
  - 날짜 범위 검색
  - 주문 취소 감지 및 처리

### 3. 배송 상태 동기화
- **위치**: `jobs/shippingStatusJob.js`
- **기능**:
  - 배송 전/배송 보류 주문 상태 업데이트
  - Cafe24에서 주소 작성 여부 자동 감지
  - 주소 작성 완료 시 자동 보류 해제
  - 1초 간격 API 호출 (Rate Limiting 고려)

### 4. Cafe24 API 연동
- **위치**: `utils/cafe24.js`
- **주요 API**:
  - 주문 정보 조회
  - 배송지 정보 변경
  - 주문 상태 변경 (hold/unhold)
  - 취소 정보 조회

### 5. 알림톡 발송
- **위치**: `utils/alertTalk.js`
- **기능**:
  - 배송 정보 알림
  - 선물 배송 안내
  - 선물 거절 안내
  - 선물 메시지 전달

---

## 🗄️ 데이터베이스 스키마

### GiftOrder (선물 주문)
```javascript
{
  order_id: STRING (PK),           // 주문 ID
  mall_id: STRING,                 // 쇼핑몰 ID
  shop_no: STRING(5),              // 상점 번호
  buyer_name: STRING,              // 구매자 이름
  buyer_email: STRING,             // 구매자 이메일
  buyer_phone: STRING,             // 구매자 전화번호
  buyer_cellphone: STRING,         // 구매자 휴대폰
  paid: ENUM("F","T"),             // 결제 여부
  payment_date: DATE,              // 결제 일시
  cancel_date: STRING,             // 취소 일시
  shipping_type: CHAR,             // 배송 타입
  shipping_status: CHAR,           // 배송 상태
  shipping_message: STRING,        // 배송 메시지
  shipping_date: STRING,           // 배송 일자
  order_date: DATE,                // 주문 일시
  ordering_product_code: STRING,   // 상품 코드
  ordering_product_name: STRING,   // 상품명
  gift_message: STRING,            // 선물 메시지
  receiver_name: STRING,           // 수령인 이름
  receiver_phone: STRING,          // 수령인 전화번호
  receiver_cellphone: STRING,      // 수령인 휴대폰
  receiver_zipcode: STRING,        // 수령인 우편번호
  receiver_address_full: STRING,   // 수령인 전체 주소
  product_no: INTEGER,             // 상품 번호
  is_hold: ENUM("F","T"),          // 보류 여부 (기본값: T)
  is_deny: ENUM("F","T"),          // 거절 여부 (기본값: F)
  is_written: ENUM("F","T"),       // 주소 작성 여부 (기본값: F)
  payment_method: STRING,          // 결제 수단
  additional_order_info_list: STRING, // 추가 주문 정보
  product_admin: STRING,           // 상품 관리자
  member_id: STRING                // 회원 ID
}
```

### GiftMall (쇼핑몰 정보)
- mall_id: 쇼핑몰 ID
- access_token: OAuth Access Token
- refresh_token: OAuth Refresh Token
- expires_at: Access Token 만료 시간
- refresh_token_expires_at: Refresh Token 만료 시간
- client_id: OAuth Client ID
- user_id: 사용자 ID
- scopes: API 권한 범위
- issued_at: 발급 시간

---

## 🔐 환경 변수

### 개발 환경 (Development)
```javascript
NODE_ENV: 'development'
PORT: 3050
TZ: 'Asia/Seoul'
HOST: 'tkr-ec-01.kr'
MALL_ID: 'thermoskorea'
CLIENTID: 'Z7JfVtTqIqVP5X1Opi4WzH'
CLIENTSECRETKEY: 'snelODNzECgUX6M0MTCA0B'
SERVICEKEY: 'LqHs92UuQCBzdvtEYAbCEACSUJ6+wxbSUWA2lDG5FqY='
HOOKKEY: 'b2ce23f2-884c-40e2-a7aa-0bc78bda72fa'
DB_HOST: 'ec2-3-39-66-130.ap-northeast-2.compute.amazonaws.com'
DB_USER: 'thermos'
DB_PASS: 'thermos1904!'
DB_PORT: 3306
DB_NAME: 'tmdev'
LOG_DIR: './log/innerview'
```

### 프로덕션 환경 (Production)
- 동일한 설정이지만 LOG_DIR이 `/var/log/innerview`로 변경

---

## 🚀 배포 설정

### PM2 Ecosystem
```javascript
{
  name: 'thermosgift',
  script: './bin/www',
  instances: 1,
  autorestart: true,
  watch: false,
  ignore_watch: ["node_modules", "public", "uploads", "log"]
}
```

### 배포 프로세스
```bash
# 프로덕션 배포
Host: tkr-ec-01.kr
User: ubuntu
Path: /home/ubuntu/tmgift
Repository: ssh://git@git.innerviewit.com:30001/thermos/gift.git
Branch: main

# 배포 후 실행
npm install
pm2 startOrRestart ecosystem.config.js --env production
pm2 save
```

---

## 📊 API 권한 범위 (Cafe24 Scopes)

### 읽기 권한
- `mall.read_category` - 상품분류 조회
- `mall.read_product` - 상품 정보 조회
- `mall.read_collection` - 브랜드/자체분류 조회
- `mall.read_supply` - 공급사 정보 조회
- `mall.read_customer` - 회원 정보 조회
- `mall.read_promotion` - 쿠폰/혜택 조회
- `mall.read_shipping` - 배송 설정 조회

### 읽기/쓰기 권한
- `mall.write_order, mall.read_order` - 주문 정보 조회/수정
- `mall.write_application, mall.read_application` - 앱 설치 관리
- `mall.read_shipping, mall.write_shipping` - 배송 설정 관리

---

## 🔄 워크플로우

### 1. 초기 인증 플로우
```
1. 사용자가 Cafe24 앱 설치
2. Cafe24에서 앱 URL로 리다이렉트 (HMAC 포함)
3. HMAC 검증
4. Access Token 발급 요청
5. Token 저장 (GiftMall 테이블)
6. /gift 페이지로 리다이렉트
```

### 2. 주문 처리 플로우
```
1. Cafe24 Webhook으로 주문 정보 수신
2. GiftOrder 테이블에 저장 (is_hold: T, is_written: F)
3. 관리자가 주소 작성 대기
4. 배송 상태 Job이 주기적으로 확인
5. 주소 작성 감지 시:
   - is_written: T로 변경
   - is_hold: F로 변경
   - Cafe24에 unhold 요청
6. 배송 진행
```

### 3. 주소 변경 플로우
```
1. 관리자가 주소 변경 요청
2. 취소 여부 확인
3. 보류 상태 해제 (필요시)
4. Cafe24 API로 주소 변경
5. GiftOrder 업데이트
6. 알림톡 발송 (선택)
```

---

## 🛠️ 주요 유틸리티 함수

### cafe24.js
- `orderInfo(order_id)` - 주문 정보 조회
- `orderInfoForBatch(order_id)` - 배치용 주문 정보 조회
- `changeOrderReceivers(request)` - 배송지 변경
- `orderState(order_id, state)` - 주문 상태 변경 (hold/unhold)
- `cancelInfo(order_id)` - 취소 정보 조회

### auth.js
- `isPermissonBool(user)` - 권한 확인
- 인증 미들웨어 함수

---

## 📝 로깅

### Winston 설정
- **일별 로그 로테이션**: winston-daily-rotate-file
- **로그 레벨**: info, error
- **로그 위치**: 
  - 개발: `./log/innerview`
  - 프로덕션: `/var/log/innerview`

### 주요 로그 포인트
- OAuth 토큰 갱신
- 주문 정보 조회
- 배송지 변경
- API 에러
- 배치 작업 실행

---

## 🔧 스케줄링

### Bree 작업
- **shippingStatusJob**: 배송 상태 동기화
- **cronJob**: 정기 작업 (구체적 내용은 cronJob.js 참조)

### 실행 주기
- 배송 상태 Job: 주기적 실행 (Bree 설정에 따름)
- API 호출 간격: 1초 (Rate Limiting 고려)

---

## 🎨 프론트엔드

### 템플릿 엔진
- **Handlebars (HBS)**
- **레이아웃**: `views/layouts/layout.hbs`
- **파셜**: `views/layouts/partials/`

### 주요 페이지
- `/` - OAuth 인증 페이지
- `/gift` - 선물 주문 관리 (DataTables)
- `/login` - 관리자 로그인
- `/permission` - 권한 설정

### 스타일
- `css_review`: 리뷰 스타일
- `css_database`: 데이터베이스 스타일
- `css_jui`: jQuery UI 스타일

---

## 🔒 보안

### 인증
- **Passport Local Strategy**: 로컬 인증
- **Session 기반**: express-session
- **Remember Me**: passport-remember-me

### 데이터 보호
- **bcrypt**: 비밀번호 해싱
- **HMAC SHA256**: Cafe24 요청 검증
- **HTTPS**: 프로덕션 환경 필수

### CORS
- `cors` 미들웨어 활성화
- 모든 origin 허용 (프로덕션에서는 제한 권장)

---

## 📦 의존성 관리

### 주요 의존성
```json
{
  "express": "^4.17.2",
  "sequelize": "^6.16.1",
  "mariadb": "^2.5.5",
  "passport": "^0.4.1",
  "bree": "^9.1.3",
  "winston": "^3.8.2",
  "axios": "^0.26.1",
  "moment": "^2.29.1",
  "pm2": "^5.2.2"
}
```

### 개발 의존성
```json
{
  "sequelize-cli": "^6.4.1"
}
```

---

## 🚨 에러 처리

### 주요 에러 케이스
1. **OAuth 토큰 만료**: 자동 갱신 로직
2. **API Rate Limiting**: 1초 간격 sleep
3. **주문 취소**: is_deny 플래그 설정
4. **네트워크 에러**: Winston 로깅 및 재시도
5. **데이터베이스 에러**: Sequelize 에러 핸들링

### 에러 응답
- 클라이언트: `<script>alert(...);history.go(-1);</script>`
- API: JSON 형식 에러 메시지
- 로그: Winston을 통한 상세 로깅

---

## 🔍 모니터링

### PM2 모니터링
```bash
pm2 list                    # 프로세스 목록
pm2 logs thermosgift        # 로그 확인
pm2 monit                   # 실시간 모니터링
pm2 restart thermosgift     # 재시작
```

### 로그 확인
```bash
# 개발 환경
tail -f ./log/innerview/*.log

# 프로덕션 환경
tail -f /var/log/innerview/*.log
```

---

## 📈 개선 제안

### 보안
1. CORS 설정을 특정 도메인으로 제한
2. 환경 변수를 .env 파일로 분리
3. API 키를 코드에서 완전히 분리
4. Rate Limiting 미들웨어 추가

### 성능
1. 데이터베이스 인덱스 최적화
2. API 응답 캐싱
3. 배치 작업 병렬 처리
4. Connection Pool 설정 최적화

### 코드 품질
1. TypeScript 도입 검토
2. ESLint/Prettier 설정
3. 단위 테스트 추가
4. API 문서화 (Swagger)

### 기능
1. 실시간 알림 (WebSocket)
2. 주문 통계 대시보드
3. 엑셀 내보내기 기능
4. 일괄 주소 변경 기능

---

## 📞 연락처 및 참고

### 개발사
- **회사**: Innerview IT
- **Git Repository**: git.innerviewit.com/thermos/gift

### 외부 서비스
- **Cafe24 API**: https://developers.cafe24.com/
- **호스트**: tkr-ec-01.kr
- **데이터베이스**: ec2-3-39-66-130.ap-northeast-2.compute.amazonaws.com

---

## 📅 버전 히스토리

### 2023-03-01
- 현재 버전
- Cafe24 OAuth 연동
- 선물 주문 관리 시스템
- 배송 상태 자동 동기화
- 알림톡 발송 기능

---

## 🎯 프로젝트 목표

이 시스템은 Thermos Korea의 Cafe24 쇼핑몰에서 발생하는 선물 주문을 효율적으로 관리하기 위해 개발되었습니다. 주요 목표는:

1. **자동화**: 배송 상태 자동 동기화로 수작업 최소화
2. **효율성**: 주소 작성 여부 자동 감지 및 보류 해제
3. **안정성**: OAuth 토큰 자동 갱신 및 에러 핸들링
4. **확장성**: 모듈화된 구조로 기능 추가 용이
5. **모니터링**: Winston 로깅 및 PM2 프로세스 관리

---

*문서 작성일: 2025-09-30*  
*작성자: Windsurf AI Assistant*
