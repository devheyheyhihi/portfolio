# Windsurf 프로젝트 분석 문서 - Purcell Backend

## 📋 프로젝트 개요

**프로젝트명**: Purcell Backend  
**프로젝트 타입**: Node.js Express 백엔드 API 서버  
**목적**: Purcell 쇼핑몰의 백엔드 시스템 (리뷰 관리, 주문 관리, 멤버십 관리 등)  
**개발 환경**: Node.js, Express, MariaDB, AWS S3

---

## 🏗️ 기술 스택

### 백엔드 프레임워크
- **Express.js** (v4.18.2) - 웹 애플리케이션 프레임워크
- **Node.js** - 런타임 환경

### 데이터베이스
- **MariaDB** - 메인 데이터베이스
- **Sequelize** (v6.28.1) - ORM (Object-Relational Mapping)

### 클라우드 서비스
- **AWS S3** (@aws-sdk/client-s3) - 파일 스토리지
- **AWS Lambda** (@aws-sdk/client-lambda) - 서버리스 함수
- **CloudFront** - CDN (d358qlcw9c8y5x.cloudfront.net)

### 주요 라이브러리
- **axios** (v1.3.3) - HTTP 클라이언트
- **multer** & **multer-s3** - 파일 업로드 처리
- **winston** - 로깅
- **node-schedule** - 스케줄링/배치 작업
- **moment** - 날짜/시간 처리
- **crypto-js** - 암호화
- **exceljs** - 엑셀 파일 처리
- **express-session** - 세션 관리

### 템플릿 엔진
- **Handlebars (HBS)** - 서버 사이드 렌더링

### 개발 도구
- **ESLint** - 코드 품질 관리
- **Prettier** - 코드 포맷팅
- **PM2** - 프로세스 관리 (ecosystem.config.js)

---

## 📁 프로젝트 구조

```
purcell/
├── app.js                      # Express 애플리케이션 메인 파일
├── bin/
│   └── www                     # 서버 시작 스크립트
├── config/
│   ├── config.js              # 데이터베이스 설정
│   ├── winston.js             # 로깅 설정
│   └── s3.json                # S3 설정
├── models/                     # Sequelize 모델 (23개)
│   ├── index.js               # 모델 초기화
│   ├── order.js               # 주문 모델
│   ├── product.js             # 상품 모델
│   ├── review.js              # 리뷰 모델
│   ├── membership.js          # 멤버십 모델
│   ├── influencer.js          # 인플루언서 모델
│   └── ...                    # 기타 모델들
├── routes/                     # API 라우트
│   ├── api.js                 # 메인 API 라우트
│   ├── backend.js             # 백엔드 관리 라우트
│   ├── excel.js               # 엑셀 관련 라우트
│   ├── hook.js                # 웹훅 라우트
│   ├── callback.js            # 콜백 라우트
│   ├── index.js               # 인덱스 라우트
│   └── api/
│       ├── v1/                # API v1
│       ├── v2/                # API v2
│       ├── v3/                # API v3
│       └── admin/             # 관리자 API
├── utils/                      # 유틸리티 함수
│   ├── cafe24.js              # Cafe24 API 연동
│   ├── cafe24Api.js           # Cafe24 API 헬퍼
│   ├── common.js              # 공통 유틸리티
│   ├── schedule.js            # 스케줄러
│   ├── s3manager.js           # S3 관리
│   └── s3/                    # S3 관련 유틸리티
│       ├── s3Client.js
│       ├── object.js
│       ├── util.js
│       └── files/             # 파일 타입별 처리
├── helper/
│   └── index.js               # Handlebars 헬퍼
├── public/                     # 정적 파일 (287개)
├── uploads/                    # 업로드 파일 저장소
├── log/                        # 로그 파일
├── ecosystem.config.js         # PM2 설정
├── package.json               # 의존성 관리
└── README.md                  # 프로젝트 설명
```

---

## 🗄️ 데이터베이스 모델

### 주요 모델 (23개)

1. **Order** - 주문 정보
2. **Product** - 상품 정보
3. **Review** - 리뷰
4. **ReviewReply** - 리뷰 답글
5. **ReviewAllow** - 리뷰 허용 설정
6. **Membership** - 멤버십 관리
7. **Category** - 카테고리
8. **File** - 파일 정보
9. **NomalFile** - 일반 파일
10. **StreamingFile** - 스트리밍 파일
11. **Influencer** - 인플루언서
12. **InfluencerContent** - 인플루언서 콘텐츠
13. **InfluencerInfo** - 인플루언서 정보
14. **Like** - 좋아요
15. **Mall** - 쇼핑몰 정보
16. **Push** - 푸시 알림
17. **PushAlert** - 푸시 알림 알림
18. **PushAlertLog** - 푸시 알림 로그
19. **PushLog** - 푸시 로그
20. **Alert** - 알림
21. **Inquiry** - 문의
22. **Setting** - 설정
23. **User** - 사용자 (추정)

### 데이터베이스 설정

- **개발 환경**: 2개의 데이터베이스 연결 (test, real)
  - Test DB: `pctest` (테스트용)
  - Real DB: `pcdev` (실제 데이터)
- **프로덕션 환경**: 단일 데이터베이스
- **호스트**: purcelldev.co.kr
- **포트**: 3306
- **타임존**: Asia/Seoul (UTC+9)
- **문자셋**: utf8mb4

---

## 🔌 API 엔드포인트

### 리뷰 관련 API
- `GET /api/reviews` - 전체 리뷰 조회
- `GET /api/reviews/best` - 베스트 리뷰 조회
- `GET /api/reviews/photo` - 포토 리뷰 조회
- `GET /api/review/detail/:id` - 리뷰 상세 조회
- `GET /api/review/like/:id` - 리뷰 좋아요
- `GET /api/product/reviews/:id` - 상품별 리뷰 조회
- `GET /api/product/count/reviews/:id` - 상품별 리뷰 수 조회
- `GET /api/product/review/best/:id` - 상품별 베스트 리뷰
- `GET /api/product/review/photo/:id` - 상품별 포토 리뷰
- `GET /api/review/wrote` - 작성한 리뷰 조회
- `GET /api/review/able` - 작성 가능한 리뷰 조회
- `GET /api/review/point` - 리뷰 포인트 조회

### 주문 관련 API
- `GET /api/orderInfo/:order_id` - 주문 정보 조회
- `GET /api/orderBatch` - 주문 배치 처리

### 멤버십 관련 API
- `GET /api/membership/:id` - 멤버십 정보 조회
- `POST /api/membership/request` - 멤버십 신청

### 네이버페이 리뷰
- `GET /api/nReviews` - 네이버페이 리뷰 조회

### API 버전
- **v1**: `/api/v1/*`
- **v2**: `/api/v2/*` (현재 메인 사용)
- **v3**: `/api/v3/*`
- **Admin**: `/api/admin/*`

---

## ⚙️ 주요 기능

### 1. 리뷰 관리 시스템
- 리뷰 작성, 조회, 수정, 삭제
- 베스트 리뷰 선정
- 포토 리뷰 필터링
- 리뷰 좋아요 기능
- 리뷰 답글 기능
- 리뷰 작성 시 포인트 지급
- 텍스트 필터링 (욕설, 개인정보 등)
- 리뷰 노출 규칙 설정 (이름, 옵션, 날짜 등)

### 2. 주문 관리
- Cafe24 주문 데이터 동기화
- 주문 생성/업데이트
- 배송 상태 추적
- 취소/반품/교환 처리
- 주문 배치 처리

### 3. 멤버십 시스템
- VIP CLUB 등급 관리
- 멤버십 신청 조건 검증
  - 최근 6개월 구매액 150,000원 이상
  - 최근 6개월 주문 2건 이상
  - 최근 6개월 리뷰 2건 이상
- 자동 등급 업/다운
- 멤버십 연장 시스템
- 알림톡 발송

### 4. 파일 관리
- AWS S3 연동
- 이미지 업로드 (JPEG, JPG, PNG)
- 비디오 업로드 (MP4)
- 스트리밍 파일 처리
- CloudFront CDN 연동
- 파일 타입별 처리
  - 인플루언서 콘텐츠 파일
  - 인플루언서 정보 파일
  - 리뷰 파일
  - 일반 파일
  - 스트리밍 파일

### 5. Cafe24 연동
- 주문 데이터 동기화
- 상품 데이터 동기화
- 카테고리 데이터 동기화
- 회원 등급 변경
- 포인트 적립
- 개인정보 조회

### 6. 배치 작업 (Scheduler)
- **매일 00:00** - 푸시 알림 통계 초기화
- **매일 00:30** - 주문 데이터 동기화
- **매일 01:00** - 취소 데이터 동기화
- **매일 01:30** - 배송 상태 업데이트
- **매일 02:00** - 카테고리 데이터 동기화
- **매일 02:15** - 상품 데이터 동기화
- **매일 06:00** - 멤버십 등급 관리
  - 신청 가능 상태 업데이트
  - 등급 업 처리
  - 등급 다운 처리
  - 멤버십 연장
- **매일 09:30** - 리뷰 작성 알림톡 발송
- **매일 10:00** - 멤버십 신청 가능 알림
- **매일 10:30** - 멤버십 강등 예정 알림
- **매일 11:00** - 멤버십 등업 완료 알림

### 7. 알림톡 시스템
- 리뷰 작성 요청 알림
- 멤버십 관련 알림
- 배송 완료 알림

### 8. 인플루언서 관리
- 인플루언서 정보 관리
- 인플루언서 콘텐츠 관리
- 스트리밍 파일 관리

---

## 🔐 환경 변수

### 애플리케이션 설정
- `NODE_ENV`: 환경 (development/production)
- `PORT`: 서버 포트 (3011)
- `TZ`: 타임존 (Asia/Seoul)
- `HOST`: 호스트 도메인
- `secret`: 세션 시크릿

### 데이터베이스
- `DB_HOST`: 데이터베이스 호스트
- `DB_USER`: 데이터베이스 사용자
- `DB_PASS`: 데이터베이스 비밀번호
- `DB_PORT`: 데이터베이스 포트
- `DB_NAME`: 데이터베이스 이름
- `REAL_DB_USER`: 실제 DB 사용자
- `REAL_DB_NAME`: 실제 DB 이름

### AWS S3
- `S3REGION`: S3 리전 (ap-northeast-2)
- `S3ACCESSKEY`: S3 액세스 키
- `S3SECRETACCESSKEY`: S3 시크릿 키

### Cafe24 API
- `CLIENTID`: Cafe24 클라이언트 ID
- `CLIENTSECRETKEY`: Cafe24 시크릿 키
- `SERVICEKEY`: 서비스 키
- `HOOKKEY`: 웹훅 키

### 기타
- `MALL_ID`: 쇼핑몰 ID (purcell)
- `USER_ID`: 사용자 ID
- `isDebug`: 디버그 모드
- `LOG_DIR`: 로그 디렉토리
- `APP_VERSION`: 앱 버전

---

## 🚀 배포 설정

### PM2 설정 (ecosystem.config.js)
- **앱 이름**: purcelldev
- **인스턴스**: 1
- **메모리 제한**: 4GB
- **자동 재시작**: 활성화

### 배포 환경

#### Staging
- **서버**: 43.200.20.218
- **사용자**: ubuntu
- **브랜치**: develop
- **경로**: /var/www/purcell

#### Production
- **서버**: purcelldev.co.kr
- **사용자**: root
- **브랜치**: master
- **경로**: /var/www/pcdev/purcell

### CI/CD
- **GitLab CI** (.gitlab-ci.yml)
- **Jenkins** (Jenkinsfile)
- Git 저장소: git.innerviewit.com

---

## 📊 로깅

### Winston 로깅 시스템
- **로그 디렉토리**: 
  - 개발: `./log/innerview`
  - 프로덕션: `/var/log/innerview`
- **로그 레벨**: info, error, debug
- **로그 로테이션**: winston-daily-rotate-file

---

## 🔧 개발 가이드

### 설치
```bash
npm install
# 또는
yarn install
```

### 실행
```bash
# 개발 모드
npm start

# PM2로 실행
pm2 start ecosystem.config.js --env development

# 프로덕션 모드
pm2 start ecosystem.config.js --env production
```

### 코드 품질
```bash
# ESLint 실행
npm run lint

# Prettier 실행
npm run format
```

---

## 🔍 주요 특징

### 1. 멀티 데이터베이스 지원
- 개발 환경에서 테스트 DB와 실제 DB 동시 연결
- 프로덕션 환경에서는 단일 DB 사용

### 2. API 버전 관리
- v1, v2, v3 API 버전 분리
- 관리자 전용 API 분리

### 3. 파일 업로드 최적화
- S3 직접 업로드
- CloudFront CDN 연동
- 파일 타입별 처리 로직

### 4. 자동화된 배치 작업
- 데이터 동기화
- 멤버십 관리
- 알림 발송

### 5. 보안
- 세션 관리
- 암호화 (crypto-js)
- 파일 확장자 검증
- 텍스트 필터링

### 6. 성능 최적화
- 페이지네이션
- 데이터베이스 인덱싱
- 캐싱 (추정)

---

## 📝 개선 제안

### 1. 보안
- [ ] 환경 변수 파일에서 민감 정보 제거 (ecosystem.config.js)
- [ ] API 인증/인가 시스템 강화
- [ ] HTTPS 강제 적용
- [ ] Rate Limiting 추가

### 2. 코드 품질
- [ ] TypeScript 마이그레이션 고려
- [ ] 단위 테스트 추가
- [ ] API 문서화 (Swagger/OpenAPI)
- [ ] 에러 핸들링 표준화

### 3. 성능
- [ ] Redis 캐싱 도입
- [ ] 데이터베이스 쿼리 최적화
- [ ] 이미지 최적화 (리사이징, WebP 변환)
- [ ] CDN 활용 확대

### 4. 모니터링
- [ ] APM 도구 도입 (New Relic, DataDog 등)
- [ ] 에러 트래킹 (Sentry)
- [ ] 로그 분석 시스템

### 5. 아키텍처
- [ ] 마이크로서비스 아키텍처 고려
- [ ] 메시지 큐 도입 (RabbitMQ, Kafka)
- [ ] 서버리스 함수 활용 확대

---

## 📞 연락처 및 리소스

- **Git 저장소**: ssh://git@git.innerviewit.com:30001/purcell/purcell.git
- **도메인**: purcelldev.co.kr
- **S3 버킷**: purcell-storage
- **CloudFront**: d358qlcw9c8y5x.cloudfront.net

---

## 📅 버전 정보

- **앱 버전**: 2023-06-01
- **버전**: 2.0

---

## 📄 라이센스

프로젝트 라이센스 정보는 별도 문서 참조

---

**문서 작성일**: 2025-10-01  
**작성자**: Windsurf AI  
**최종 업데이트**: 2025-10-01
