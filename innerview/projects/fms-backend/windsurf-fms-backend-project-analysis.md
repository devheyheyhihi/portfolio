# Windsurf FMS Backend 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: FMS Backend (Facility Management System Backend)  
**버전**: 2.0  
**프레임워크**: Node.js + Express.js  
**데이터베이스**: MariaDB  
**프로세스 관리**: PM2  
**저장소**: http://git.innerviewit.com/fms/fms-backend.git

## 🏗️ 프로젝트 구조

```
fms-backend/
├── app.js                      # Express 애플리케이션 메인 파일
├── bin/
│   └── www                     # 서버 시작 스크립트
├── common/                     # 공통 유틸리티 모듈
│   ├── aligoApi.js            # Aligo API 연동
│   ├── aligoSms.js            # SMS 발송 기능
│   ├── authCheck.js           # 인증 체크
│   ├── commonFunc.js          # 공통 함수
│   ├── deliveryAlertSMS.js    # 배송 알림 SMS
│   ├── deliveryAutoStatusChange.js  # 배송 상태 자동 변경
│   ├── deliveryReturnSMS.js   # 배송 반품 SMS
│   ├── express-sse.js         # Server-Sent Events
│   ├── expressDatatableResult.js  # DataTable 결과 포맷
│   ├── expressResult.js       # Express 응답 포맷
│   ├── kakaoMapApi.js         # 카카오 맵 API
│   ├── logger.js              # 로깅 시스템
│   ├── makeHash.js            # 해시 생성
│   ├── niceMobileModule.js    # 나이스 본인인증
│   ├── publicAddressSearchApi.js  # 주소 검색 API
│   └── socketIO.js            # Socket.IO 설정
├── config/                     # 설정 파일
│   ├── config.js              # DB 및 환경 설정
│   ├── passport.js            # Passport 인증 설정
│   └── winston.js             # Winston 로거 설정
├── essentials/                 # 필수 상수 및 파라미터
│   ├── Enums.js               # 열거형 상수
│   └── params.js              # 파라미터 정의
├── models/                     # Sequelize 모델 (38개)
│   ├── asset.js               # 자산 관리
│   ├── deliveryCenter.js      # 배송 센터
│   ├── deliveryIssue.js       # 배송 이슈
│   ├── distributionCenter.js  # 물류 센터
│   ├── hardware.js            # 하드웨어 관리
│   ├── member.js              # 회원 관리
│   ├── order.js               # 주문 관리
│   ├── partner.js             # 파트너 관리
│   ├── retrieveRequest.js     # 회수 요청
│   ├── returnBox.js           # 반품 박스
│   └── ...                    # 기타 모델들
├── routes/                     # 라우터 (26개 주요 라우트)
│   ├── asset/                 # 자산 관리 API
│   ├── common/                # 공통 API
│   ├── cs/                    # 고객 서비스 API
│   ├── delivery/              # 배송 API
│   ├── deliveryCenter/        # 배송 센터 API
│   ├── deliveryIssue/         # 배송 이슈 API
│   ├── distributionCenter/    # 물류 센터 API
│   ├── excel/                 # 엑셀 처리 API
│   ├── hardware/              # 하드웨어 API
│   ├── member/                # 회원 API
│   ├── notice/                # 공지사항 API
│   ├── order/                 # 주문 API
│   ├── partner/               # 파트너 API
│   ├── product/               # 상품 API
│   ├── retrieve/              # 회수 API
│   ├── retrieveApp/           # 회수 앱 API
│   ├── returnBox/             # 반품 박스 API
│   ├── userApp/               # 사용자 앱 API
│   └── ...
├── views/                      # Handlebars 템플릿 (90개)
├── public/                     # 정적 파일
├── migrations/                 # DB 마이그레이션
├── log/                        # 로그 파일
├── ecosystem.config.js         # PM2 설정
├── package.json               # 의존성 관리
└── webpack.config.js          # Webpack 설정
```

## 🔧 주요 기술 스택

### Backend
- **Node.js**: JavaScript 런타임
- **Express.js**: 웹 프레임워크
- **Sequelize**: ORM (Object-Relational Mapping)
- **MariaDB**: 관계형 데이터베이스

### 인증 & 보안
- **Passport.js**: 인증 미들웨어
- **passport-local**: 로컬 인증 전략
- **bcrypt**: 비밀번호 암호화
- **crypto-js**: 암호화 라이브러리
- **express-session**: 세션 관리
- **connect-redis**: Redis 세션 스토어

### 외부 API 연동
- **AWS SDK**: S3 파일 저장소
- **Aligo API**: SMS 발송
- **Kakao Map API**: 지도 서비스
- **NICE 본인인증**: 휴대폰 본인인증

### 실시간 통신
- **Socket.IO**: 양방향 실시간 통신
- **express-sse**: Server-Sent Events

### 파일 처리
- **multer**: 파일 업로드
- **multer-s3**: S3 파일 업로드
- **excel4node**: 엑셀 생성
- **exceljs**: 엑셀 처리
- **xlsx**: 엑셀 파싱

### 유틸리티
- **moment**: 날짜/시간 처리
- **lodash**: JavaScript 유틸리티
- **axios**: HTTP 클라이언트
- **uuid**: 고유 ID 생성
- **geolib**: 지리 계산

### 로깅 & 모니터링
- **winston**: 로깅 프레임워크
- **winston-daily-rotate-file**: 일별 로그 파일 로테이션
- **morgan**: HTTP 요청 로거

### 템플릿 엔진
- **express-hbs**: Handlebars 템플릿 엔진
- **hbs**: Handlebars

### 프로세스 관리
- **PM2**: 프로세스 관리자
- **node-schedule**: 스케줄러

## 🌐 환경 설정

### Development (개발)
- **포트**: 3011
- **데이터베이스**: fms_rn_dev
- **DB 호스트**: 211.169.231.242
- **Redis**: redis://innerviewit.co.kr:6379
- **도메인**: https://fmstest.innerviewit.co.kr

### Production (운영)
- **포트**: 3015
- **데이터베이스**: fms_rn_prd
- **DB 호스트**: localhost
- **Redis**: redis://localhost:6379
- **도메인**: https://the-pureun.co.kr
- **실행 모드**: cluster_mode

### Local (로컬)
- **포트**: 3011
- **데이터베이스**: fms_rn_dev
- **DB 호스트**: 211.169.231.242
- **Redis**: redis://211.169.231.242:6379
- **도메인**: http://localhost:3011

## 📊 주요 기능 모듈

### 1. 주문 관리 (Order Management)
- 주문 접수, 처리, 추적
- 주문 상태 관리 (접수, 배송중, 완료 등)
- 엑셀 일괄 업로드

### 2. 배송 관리 (Delivery Management)
- 배송 상태 추적
- 배송 센터 관리
- 배송 이슈 처리
- 자동 상태 변경
- SMS 알림 발송

### 3. 회수 관리 (Retrieve Management)
- 회수 요청 처리
- 회수 앱 API
- 회수 완료 처리
- 회수 독촉 SMS

### 4. 자산 관리 (Asset Management)
- 자산 등록 및 추적
- 자산 상태 관리 (사용가능, 사용중, 분실 등)
- 자산 요청 처리
- 자산 히스토리

### 5. 반품 관리 (Return Management)
- 반품 요청
- 반품 박스 관리
- 반품 상태 추적

### 6. 회원 관리 (Member Management)
- 회원 가입/로그인
- 권한 관리 (관리자, 공급자, 배송자, 사용자)
- 회원 상태 관리
- Passport.js 인증

### 7. 파트너 관리 (Partner Management)
- 파트너 등록
- 파트너 계정 관리
- 파트너별 권한 설정

### 8. 하드웨어 관리 (Hardware Management)
- 하드웨어 등록 및 추적
- 비콘(Beacon), NFC 관리
- 하드웨어 로그

### 9. 고객 서비스 (CS)
- 문의 관리
- 통화 기록
- 공지사항

### 10. 물류 센터 관리
- 배송 센터
- 물류 센터
- 세탁 센터

## 🔐 인증 시스템

### Passport.js 전략
- **Local Strategy**: 이메일/비밀번호 기반 인증
- 세션 기반 인증
- Redis 세션 스토어

### 인증 흐름
1. 로그인 요청 → `/login` 또는 `/userApp/login`
2. Passport Local Strategy 검증
3. 세션 생성 및 Redis 저장
4. 인증 미들웨어를 통한 보호된 라우트 접근

### 보호된 라우트
- 로그인 페이지, 공개 페이지, 사용자 앱 로그인을 제외한 모든 라우트
- `app.all("*")` 미들웨어에서 인증 체크

## 📱 API 구조

### 라우터 자동 등록
- `routes/` 디렉토리 내 모든 `index.js` 파일 자동 스캔
- 디렉토리 구조 기반 URL 매핑
- 제외 디렉토리: `version`, `hardware`, `docs`

### API 문서
- JSDoc 기반 API 문서 생성
- `/userApp/doc`: 사용자 앱 API 문서
- `/public/doc`: 공개 API 문서

## 🔄 실시간 기능

### Socket.IO
- 실시간 배송 상태 업데이트
- 관리자 알림
- 하드웨어 상태 모니터링

### Server-Sent Events (SSE)
- 단방향 실시간 데이터 스트리밍

## 📧 알림 시스템

### SMS 발송
- **Aligo API** 연동
- 배송 알림 자동 발송
- 회수 요청/완료 알림
- 회수 독촉 SMS

### SMS 템플릿 카테고리
- 배송알림 자동발송
- 배송 완료
- 회수 요청
- 회수 완료
- 회수 독촉

## 📦 파일 저장

### AWS S3
- 이미지 업로드
- 문서 파일 저장
- 엑셀 파일 저장
- Multer-S3 연동

## 📈 로깅 시스템

### Winston Logger
- 일별 로그 파일 로테이션
- 에러 로그 별도 관리
- 로그 레벨: error, warn, info, debug

### 로그 디렉토리
- Development: `./log/innerview/renewal`
- Production: `/var/log/innerview`

## 🗃️ 데이터베이스 모델

### 주요 모델 (38개)
1. **Member**: 회원
2. **Partner**: 파트너
3. **Order**: 주문
4. **DeliveryCenter**: 배송 센터
5. **DeliveryIssue**: 배송 이슈
6. **DeliverySituation**: 배송 상황
7. **DistributionCenter**: 물류 센터
8. **Asset**: 자산
9. **AssetCategory**: 자산 카테고리
10. **AssetHistory**: 자산 히스토리
11. **AssetRequest**: 자산 요청
12. **Hardware**: 하드웨어
13. **HardwareLog**: 하드웨어 로그
14. **Beacon**: 비콘
15. **NFC**: NFC
16. **ReturnBox**: 반품 박스
17. **ReturnBoxLog**: 반품 박스 로그
18. **RetrieveRequest**: 회수 요청
19. **CsInquiry**: 고객 문의
20. **CsCall**: 고객 통화
21. **Notice**: 공지사항
22. **SmsLog**: SMS 로그
23. **SmsTemplate**: SMS 템플릿
24. **FcmToken**: FCM 토큰
25. **File**: 파일

## 🔢 주요 Enum 상수

### 주문 상태 (orderStatus)
- `orderReceived`: 주문접수
- `arrived`: 허브도착
- `collected`: 물류집하
- `delivering`: 배송 중
- `delivered`: 배송완료
- `holdDelivery`: 배송대기
- `unableDelivery`: 배송불가
- `requestRetrieve`: 회수요청
- `retrieved`: 회수완료
- `pushRetrieve`: 회수독촉
- `requestReturn`: 반품요청
- `returned`: 반품 완료

### 자산 상태 (assetStatus)
- `usable`: 사용가능
- `requested`: 사용요청
- `willBeUsed`: 사용예정
- `using`: 사용중
- `lost`: 분실
- `retrieved`: 회수완료

### 회원 유형 (memberType)
- `master`: 관리자
- `supplier`: 공급자
- `shipper`: 배송자
- `user`: 사용자

### 회원 상태 (memberStatus)
- `normal`: 정상
- `hold`: 대기
- `reject`: 거부
- `delete`: 삭제
- `block`: 차단
- `sleep`: 휴면
- `withDrawl`: 탈퇴

## 🚀 실행 방법

### 개발 환경
```bash
# 의존성 설치
npm install

# 개발 서버 실행
npm start

# PM2로 개발 환경 실행
pm2 start ecosystem.config.js --env develop
```

### 운영 환경
```bash
# PM2로 운영 환경 실행
pm2 start ecosystem.config.js --env production

# PM2 클러스터 모드로 실행
pm2 startOrRestart ecosystem.config.js --env production
```

### 배포
```bash
# PM2 배포
pm2 deploy production
```

## 🔧 스크립트

- `npm start`: 서버 시작
- `npm run prod:start`: PM2 런타임으로 프로덕션 시작
- `./run.sh`: 개발 서버 실행 (Unix)
- `./run_dev.sh`: 개발 서버 실행 (개발 모드)
- `exec.bat`: 윈도우 실행 스크립트
- `createOrUpdateApiDoc.sh`: API 문서 생성

## 📝 주요 특징

### 1. 동적 라우터 등록
- 파일 시스템 기반 자동 라우터 등록
- 디렉토리 구조가 URL 구조를 결정

### 2. 멀티 환경 지원
- Development, Production, Local 환경 분리
- 환경별 독립적인 설정

### 3. 세션 관리
- Redis 기반 세션 스토어
- 30일 세션 유지

### 4. CORS 설정
- 허용된 오리진 화이트리스트
- 크리덴셜 포함 요청 지원

### 5. 파일 업로드
- 500MB까지 파일 업로드 지원
- S3 직접 업로드

### 6. 스케줄링
- node-schedule을 통한 정기 작업
- 3시간마다 실행되는 워커

### 7. 보안
- bcrypt 비밀번호 해싱
- HTTPS 지원
- HTTP-only 쿠키
- CSRF 보호

## 🔍 코드 품질

### 에러 처리
- 중앙 집중식 에러 핸들러
- Winston을 통한 에러 로깅
- 사용자 친화적 에러 페이지

### 검증
- express-validator를 통한 입력 검증
- 커스텀 검증 결과 포맷터

### 코드 구조
- MVC 패턴
- 모듈화된 공통 기능
- 재사용 가능한 유틸리티

## 📊 성능 최적화

### PM2 클러스터 모드
- 멀티 코어 활용
- 무중단 재시작
- 자동 재시작

### Redis 캐싱
- 세션 캐싱
- 데이터 캐싱

### Webpack 번들링
- 프론트엔드 자산 최적화
- 코드 스플리팅

## 🔐 보안 고려사항

### 환경 변수
- 민감한 정보는 환경 변수로 관리
- DB 비밀번호, API 키 등

### 인증
- Passport.js 기반 인증
- 세션 기반 상태 관리

### 데이터 암호화
- bcrypt 비밀번호 해싱
- crypto-js 데이터 암호화

## 📱 모바일 앱 지원

### 사용자 앱 (User App)
- `/userApp/*` 라우트
- 별도 인증 플로우
- NICE 본인인증 연동

### 회수 앱 (Retrieve App)
- `/retrieveApp/*` 라우트
- 배송기사용 앱
- 실시간 위치 추적

## 🌐 외부 서비스 연동

1. **AWS S3**: 파일 저장소
2. **Aligo**: SMS 발송
3. **Kakao Map**: 지도 및 주소 검색
4. **NICE**: 본인인증
5. **Redis**: 세션 및 캐시

## 📈 모니터링

### PM2 모니터링
- CPU, 메모리 사용량
- 프로세스 상태
- 에러 로그

### Winston 로깅
- 애플리케이션 로그
- 에러 추적
- 일별 로그 파일

## 🎯 개선 가능 영역

### TODO 주석에서 발견된 개선 사항
1. `app.all` → `app.use`로 변경
2. 관리자 웹 서비스 라우터 구조 개선 (userApp처럼 공통 URL 추가)

### 권장 개선 사항
1. **TypeScript 도입**: 타입 안정성 향상
2. **API 버전 관리**: `/api/v1` 형태의 버전 관리
3. **테스트 코드**: 단위 테스트 및 통합 테스트 추가
4. **API 문서 자동화**: Swagger/OpenAPI 도입
5. **환경 변수 관리**: dotenv 사용
6. **에러 코드 표준화**: 일관된 에러 응답 포맷
7. **로깅 레벨 세분화**: 환경별 로그 레벨 조정
8. **성능 모니터링**: APM 도구 도입 (New Relic, DataDog 등)

## 📚 의존성 요약

### 주요 프로덕션 의존성
- express: ~4.16.1
- sequelize: ^6.21.3
- mariadb: ^3.0.1
- passport: ^0.6.0
- socket.io: ^4.5.1
- redis: ^4.2.0
- aws-sdk: ^2.1354.0
- moment: ^2.29.4
- winston: ^3.8.1

### 개발 의존성
- webpack: ^5.82.1
- jsdoc: ^4.0.0

## 🏁 결론

FMS Backend는 물류 및 배송 관리를 위한 종합적인 백엔드 시스템입니다. 주문, 배송, 회수, 자산 관리 등 다양한 기능을 제공하며, 실시간 통신, SMS 알림, 외부 API 연동 등 현대적인 웹 애플리케이션의 요구사항을 충족합니다.

### 주요 강점
- ✅ 모듈화된 구조
- ✅ 자동 라우터 등록
- ✅ 멀티 환경 지원
- ✅ 실시간 통신
- ✅ 외부 서비스 연동
- ✅ 포괄적인 로깅
- ✅ PM2 프로세스 관리

### 기술적 특징
- Node.js + Express.js 기반
- Sequelize ORM
- Passport.js 인증
- Socket.IO 실시간 통신
- Redis 세션 관리
- AWS S3 파일 저장
- Winston 로깅

이 시스템은 확장 가능하고 유지보수가 용이한 구조로 설계되어 있으며, 지속적인 개선을 통해 더욱 발전할 수 있는 기반을 갖추고 있습니다.
