# Windsurf 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Annual API (연차 관리 시스템)  
**버전**: 0.0.0  
**회사**: Innerview IT  
**분석일**: 2025-09-30

이 프로젝트는 회사 내부 직원들의 연차, 휴가, 회의실 예약, 요가 예약, 경비 관리 등을 종합적으로 관리하는 백엔드 API 시스템입니다.

---

## 🏗️ 기술 스택

### 핵심 프레임워크 및 라이브러리

| 카테고리 | 기술 | 버전 | 용도 |
|---------|------|------|------|
| **런타임** | Node.js | - | 서버 실행 환경 |
| **프레임워크** | Express.js | ^4.18.2 | 웹 애플리케이션 프레임워크 |
| **데이터베이스** | PostgreSQL | - | 메인 데이터베이스 |
| **ORM** | Sequelize | ^6.31.1 | 데이터베이스 ORM |
| **인증** | Passport.js | ^0.6.0 | 사용자 인증 |
| **토큰** | JWT | ^9.0.0 | 토큰 기반 인증 |
| **암호화** | bcrypt | ^5.1.0 | 비밀번호 해싱 |
| **파일 업로드** | Multer | ^1.4.5-lts.1 | 파일 업로드 처리 |
| **스케줄링** | node-schedule | ^2.1.1 | 배치 작업 스케줄링 |
| **이메일** | nodemailer | ^6.9.3 | 이메일 발송 |
| **날짜/시간** | moment | ^2.29.4 | 날짜 처리 |
| **프로세스 관리** | PM2 | - | 프로덕션 프로세스 관리 |

---

## 📁 프로젝트 구조

```
annual_api/
├── app.js                    # 메인 애플리케이션 파일
├── package.json              # 프로젝트 의존성 관리
├── ecosystem.config.js       # PM2 배포 설정
├── .gitlab-ci.yml           # GitLab CI/CD 파이프라인
├── bin/
│   └── www                   # 서버 시작 스크립트
├── config/
│   ├── config.json          # 데이터베이스 설정
│   └── passport.js          # Passport 인증 전략
├── models/                   # Sequelize 모델
│   ├── index.js
│   ├── User.js              # 사용자 모델
│   ├── Annual.js            # 연차 모델
│   ├── Company.js           # 회사 모델
│   ├── Department.js        # 부서 모델
│   ├── Team.js              # 팀 모델
│   ├── Expenses.js          # 경비 모델
│   ├── MeetingRoom.js       # 회의실 모델
│   ├── RoomReserve.js       # 회의실 예약 모델
│   ├── Yoga.js              # 요가 모델
│   ├── YogaReserve.js       # 요가 예약 모델
│   ├── Reward.js            # 보상 연차 모델
│   ├── File.js              # 파일 모델
│   └── AccessToken.js       # 액세스 토큰 모델
├── routes/                   # API 라우트
│   ├── index.js
│   ├── auth.js              # 인증 관련 API
│   ├── user.js              # 사용자 관리 API
│   ├── annual.js            # 연차 관리 API
│   ├── department.js        # 부서 관리 API
│   ├── team.js              # 팀 관리 API
│   ├── expenses.js          # 경비 관리 API
│   ├── meetingRoom.js       # 회의실 예약 API
│   ├── yoga.js              # 요가 예약 API
│   ├── setting.js           # 설정 API
│   └── api.js               # 기타 API
├── utils/                    # 유틸리티 함수
│   ├── common.js            # 공통 유틸리티
│   ├── mail.js              # 이메일 발송
│   ├── schedule.js          # 배치 스케줄링
│   └── yoga.js              # 요가 스케줄링
├── public/
│   └── uploads/             # 업로드된 파일 저장
├── private.key              # RSA 개인키
├── public.key               # RSA 공개키
└── RSA.key                  # RSA 키

```

---

## 🔑 주요 기능

### 1. 인증 및 권한 관리 (Authentication & Authorization)

#### 인증 방식
- **JWT (JSON Web Token)**: RS256 알고리즘 사용
- **Bearer Token**: Passport.js를 통한 토큰 기반 인증
- **비밀번호 암호화**: bcrypt를 사용한 안전한 비밀번호 저장

#### 주요 엔드포인트
- `POST /auth/login` - 사용자 로그인
- `GET /auth/forgot_password` - 비밀번호 찾기
- `GET /auth/valid-token` - 토큰 유효성 검증
- `POST /auth/reset-password` - 비밀번호 재설정

#### 인증 미들웨어
```javascript
// /auth* 와 /uploads* 를 제외한 모든 경로에 인증 필요
app.all(/^(?!\/auth*)(?!\/uploads*).*$/m,
    passport.authenticate("bearer", {session: false}),
    ...
);
```

### 2. 연차 관리 시스템

#### 연차 계산 로직
- **입사 1년 미만**: 월별 연차 부여 (최대 11일)
- **입사 1년 이상**: 기본 15일 + 2년마다 1일 추가 (최대 25일)
- **보상 연차**: 별도 관리
- **사용 연차**: 실시간 차감

#### 연차 통지서 자동 발송
- 배치 작업을 통한 자동 생성
- HTML 템플릿 기반 이메일 발송
- 미사용 연차 및 휴가 사용 시기 지정 통지

### 3. 회의실 예약 시스템
- 회의실 등록 및 관리
- 예약 생성, 조회, 수정, 삭제
- 예약 시간 중복 방지

### 4. 요가 예약 시스템
- 요가 클래스 스케줄 관리
- 예약 시스템
- 자동 스케줄링 기능

### 5. 경비 관리
- 경비 신청 및 승인
- 경비 내역 조회
- 파일 첨부 기능

### 6. 조직 관리
- **회사(Company)** - **부서(Department)** - **팀(Team)** 계층 구조
- 사용자별 조직 배치
- 팀장, 부서장 권한 관리

### 7. 파일 관리
- Multer를 통한 파일 업로드
- 회사 로고, 경비 증빙 등 파일 관리
- `/public/uploads/` 디렉토리에 저장

---

## 🗄️ 데이터베이스 설계

### 주요 모델 관계

```
Company (회사)
  └── Department (부서)
        └── Team (팀)
              └── User (사용자)
                    ├── Annual (연차 신청)
                    ├── Expenses (경비)
                    ├── RoomReserve (회의실 예약)
                    ├── YogaReserve (요가 예약)
                    ├── Reward (보상 연차)
                    └── AccessToken (액세스 토큰)

MeetingRoom (회의실)
  └── RoomReserve (예약)

Yoga (요가 클래스)
  └── YogaReserve (예약)
```

### 데이터베이스 연결 정보
- **호스트**: 211.169.231.242
- **포트**: 5432
- **데이터베이스**: innerview
- **Dialect**: PostgreSQL

---

## 🚀 배포 및 실행

### 개발 환경
```bash
npm install
npm start
```

### 프로덕션 환경

#### PM2 설정 (ecosystem.config.js)
- **앱 이름**: annual
- **포트**: 3021
- **인스턴스**: 1개
- **자동 재시작**: 활성화
- **파일 감시**: 활성화 (node_modules, public, uploads 제외)

#### 배포 프로세스
```bash
# PM2를 통한 자동 배포
pm2 deploy ecosystem.config.js production setup
pm2 deploy ecosystem.config.js production
```

### CI/CD 파이프라인 (GitLab)
- **트리거**: main 브랜치 푸시
- **배포 서버**: 211.169.231.242
- **배포 경로**: /home/innerview/www/innerview/annual_api
- **자동화**: npm install → PM2 재시작 → 저장

---

## 🔐 보안 고려사항

### 1. 인증 보안
- RSA 키 쌍을 사용한 JWT 서명
- 비밀번호 bcrypt 해싱
- 토큰 만료 시간: 1일

### 2. 비밀번호 재설정
- UUID 기반 일회용 토큰
- 토큰 유효 시간: 15분
- 이메일을 통한 안전한 링크 전송

### 3. 파일 업로드 보안
- MIME 타입 검증
- 파일명 타임스탬프 기반 변경
- 업로드 경로 제한

### 4. API 보안
- CORS 활성화
- Bearer 토큰 인증
- 관리자 권한 분리 (is_admin, team_leader, part_leader)

---

## 📧 이메일 시스템

### 기능
- 연차 통지서 발송
- 비밀번호 재설정 링크 발송
- HTML 템플릿 지원

### 주요 수신자
- doyoo@innerviewit.com
- ktk@innerviewit.com

---

## ⏰ 배치 작업 (Scheduling)

### 1. 연차 배치 (schedule.js)
- 연차 자동 계산
- 통지서 자동 발송
- 정기적인 연차 현황 업데이트

### 2. 요가 스케줄 (yoga.js)
- 요가 클래스 자동 스케줄링
- 예약 관리 자동화

---

## 🔧 환경 변수

### 개발 환경
```javascript
NODE_ENV: 'development'
PORT: 3021
TZ: 'Asia/Seoul'
secret: 'InnerviewSecret'
DB_HOST: '211.169.231.242'
DB_USER: 'innerview'
DB_PASS: '$inner20'
DB_PORT: 5432
DB_NAME: 'innerview'
```

### 프로덕션 환경
- 개발 환경과 동일한 설정 사용
- NODE_ENV만 'production'으로 변경

---

## 📝 API 라우팅 구조

### 동적 라우트 로딩
```javascript
// app.js에서 routes 폴더의 모든 .js 파일을 자동으로 로드
const routes = glob.sync(__dirname + '/routes/*.js');
routes.forEach(function (route) {
    var file = path.basename(route, '.js');
    if(file == 'index'){
        app.use('/', router)
    }else{
        app.use('/'+file, router)
    }
});
```

### 라우트 목록
- `/` - index.js
- `/auth` - 인증 관련
- `/user` - 사용자 관리
- `/annual` - 연차 관리
- `/department` - 부서 관리
- `/team` - 팀 관리
- `/expenses` - 경비 관리
- `/meetingRoom` - 회의실 예약
- `/yoga` - 요가 예약
- `/setting` - 설정
- `/api` - 기타 API

---

## 🎯 개선 제안사항

### 1. 보안 강화
- [ ] 환경 변수를 `.env` 파일로 분리
- [ ] 데이터베이스 자격 증명 암호화
- [ ] API Rate Limiting 추가
- [ ] HTTPS 강제 적용

### 2. 코드 품질
- [ ] ESLint 설정 추가
- [ ] 단위 테스트 작성
- [ ] API 문서화 (Swagger)
- [ ] 에러 핸들링 표준화

### 3. 성능 최적화
- [ ] 데이터베이스 쿼리 최적화
- [ ] 캐싱 전략 도입 (Redis)
- [ ] 로깅 시스템 개선 (Winston)
- [ ] PM2 클러스터 모드 활용

### 4. 기능 개선
- [ ] 연차 승인 워크플로우
- [ ] 실시간 알림 시스템
- [ ] 모바일 앱 지원
- [ ] 다국어 지원

---

## 📞 연락처 및 리소스

### Git 저장소
- **URL**: ssh://git@git.innerviewit.com:30001/innerviewit/annual_api.git
- **브랜치**: main

### 서버 정보
- **호스트**: 211.169.231.242
- **사용자**: innerview
- **배포 경로**: /home/innerview/www/innerview/annual_api

---

## 📄 라이선스

Private - Innerview IT 내부 프로젝트

---

## 🔄 버전 히스토리

- **v1.0** - 초기 릴리스
  - 연차 관리 시스템
  - 회의실 예약 시스템
  - 요가 예약 시스템
  - 경비 관리 시스템
  - 사용자 인증 및 권한 관리

---

## 📚 참고 문서

### RSA 키 생성
```bash
# 개인키 생성
openssl genrsa -out private.key 2048

# 공개키 생성
openssl rsa -in private.key -out public.key -pubout

# PKCS#1 형식으로 변환
openssl rsa -pubin -in public.key -RSAPublicKey_out
```

---

**문서 작성일**: 2025-09-30  
**작성자**: Windsurf AI  
**프로젝트 상태**: 운영 중
