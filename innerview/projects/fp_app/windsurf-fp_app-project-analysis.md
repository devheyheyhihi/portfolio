# Windsurf 프로젝트 분석 문서

## 프로젝트 개요

**프로젝트명**: 교원 프랜시스파커 앱 (Francis Parker App)  
**프로젝트 코드명**: kyowon_pri_app  
**프레임워크**: CodeIgniter 3.x  
**언어**: PHP  
**데이터베이스**: MySQL (AWS RDS)

---

## 1. 프로젝트 구조

```
fp_app/
├── application/          # 애플리케이션 코어 디렉토리
│   ├── cache/           # 캐시 파일
│   ├── config/          # 설정 파일
│   ├── controllers/     # 컨트롤러 (25개)
│   ├── core/            # 커스텀 코어 클래스
│   ├── helpers/         # 헬퍼 함수
│   ├── hooks/           # 훅
│   ├── language/        # 언어 파일
│   ├── libraries/       # 라이브러리
│   ├── logs/            # 로그 파일
│   ├── models/          # 모델 (21개)
│   ├── views/           # 뷰 파일 (33개+)
│   ├── main.php         # 메인 PHP 파일
│   └── timer.js         # 타이머 JavaScript
├── assets/              # 정적 리소스
│   ├── css/            # 스타일시트
│   ├── fonts/          # 폰트 파일 (35개)
│   ├── img/            # 이미지
│   └── js/             # JavaScript 파일
├── system/              # CodeIgniter 시스템 디렉토리
├── index.php            # 프론트 컨트롤러
├── composer.json        # Composer 의존성
└── README.md
```

---

## 2. 기술 스택

### 백엔드
- **프레임워크**: CodeIgniter 3.x
- **언어**: PHP 5.x/7.x
- **인증**: JWT (Firebase PHP-JWT ^5.2)
- **비밀번호 해싱**: BCrypt (password_hash)

### 데이터베이스
- **DBMS**: MySQL/MariaDB
- **드라이버**: MySQLi
- **인코딩**: UTF8MB4
- **연결**:
  - `default`: AWS RDS (kyowon.cu0nkqepynqr.ap-northeast-2.rds.amazonaws.com)
  - `fp`: localhost (francisparker)

### 프론트엔드
- HTML/CSS/JavaScript
- 폰트: 35개의 커스텀 폰트 파일

### 외부 서비스
- **AWS S3**: 이미지 저장 (kyowon-edusys 버킷)
- **Push 알림**: 모바일 푸시 알림 기능

---

## 3. 주요 기능 모듈

### 3.1 사용자 관리
- **컨트롤러**: `Auth.php`, `Login.php`, `Logout.php`
- **모델**: `LoginModel.php`, `ParentModel.php`, `StaffModel.php`
- **기능**:
  - 학부모/교직원 로그인
  - 비밀번호 찾기 및 변경
  - 임시 비밀번호 발급
  - 자녀 등록 및 관리

### 3.2 교직원 관리
- **컨트롤러**: `Teacher.php`, `Director.php`, `Office.php`
- **모델**: `StaffModel.php`
- **기능**:
  - 교사 메인 페이지
  - 원장 관리 기능
  - 사무실 관리

### 3.3 학부모 기능
- **컨트롤러**: `Parents.php`
- **모델**: `ParentModel.php`, `StudentModel.php`
- **기능**:
  - 자녀 정보 조회
  - 알림 확인
  - 메시지 확인
  - 학생 사진 조회 (S3)

### 3.4 앨범 관리
- **컨트롤러**: `Album.php`
- **모델**: `AlbumModel.php`
- **뷰**: `director/album/` (create, detail, edit, list)
- **기능**:
  - 앨범 생성/수정/삭제
  - 사진 업로드
  - 댓글 기능

### 3.5 급식 관리
- **컨트롤러**: `Meal.php`
- **모델**: `MealModel.php`
- **뷰**: `director/meal/` (create, detail, list, createMonth)
- **기능**:
  - 일일 급식 등록
  - 월간 급식 관리
  - 급식 조회

### 3.6 투약 관리
- **컨트롤러**: `Medicine.php`
- **모델**: `MedicineModel.php`
- **뷰**: `director/medicine/` (detail, list)
- **기능**:
  - 투약 의뢰서 관리
  - 투약 기록 조회

### 3.7 귀가 관리
- **컨트롤러**: `Gohome.php`
- **모델**: `GoHomeModel.php`
- **뷰**: `director/gohome/` (detail, list)
- **기능**:
  - 귀가 방법 관리
  - 귀가 시간 기록

### 3.8 공지사항
- **컨트롤러**: `Notice.php`
- **모델**: `NoticeModel.php`
- **기능**:
  - 공지사항 작성/조회
  - 학부모/교직원 공지 구분

### 3.9 계획안
- **컨트롤러**: `Plan.php`
- **모델**: `PlanModel.php`
- **기능**:
  - 교육 계획안 작성
  - 계획안 조회

### 3.10 알림장
- **컨트롤러**: `Report.php`
- **모델**: `ReportModel.php`
- **기능**:
  - 일일 알림장 작성
  - 알림장 조회

### 3.11 동화책
- **컨트롤러**: `Storybook.php`
- **모델**: `StorybookModel.php`, `BookModel.php`
- **기능**:
  - 동화책 목록 관리
  - 독서 기록

### 3.12 푸시 알림
- **컨트롤러**: `Push.php`
- **모델**: `PushModel.php`, `PushTestModel.php`
- **기능**:
  - 모바일 푸시 알림 발송
  - 읽지 않은 알림 카운트
  - 디바이스 토큰 관리

### 3.13 파일 관리
- **컨트롤러**: `Uploads.php`, `Download.php`, `UploadTest.php`
- **기능**:
  - 파일 업로드
  - 파일 다운로드
  - S3 연동

### 3.14 API
- **컨트롤러**: `Api.php`
- **모델**: `ApiModel.php`
- **기능**:
  - RESTful API 엔드포인트
  - JWT 토큰 인증
  - CORS 설정
  - 모바일 앱 연동

### 3.15 배치 작업
- **컨트롤러**: `Batch.php`
- **기능**:
  - 정기 배치 작업
  - 데이터 정리

---

## 4. 데이터베이스 구조

### 주요 테이블 (추정)
- `kw_parent`: 학부모 정보
- `kw_staff`: 교직원 정보
- `kw_student`: 학생 정보
- `kw_class`: 반 정보
- `kw_album`: 앨범
- `kw_meal`: 급식
- `kw_medicine`: 투약
- `kw_gohome`: 귀가
- `kw_notice`: 공지사항
- `kw_plan`: 계획안
- `kw_report`: 알림장
- `kw_storybook`: 동화책
- `kw_push`: 푸시 알림
- `kw_message`: 메시지

---

## 5. 보안 기능

### 인증 및 권한
- JWT 토큰 기반 인증 (API)
- 세션 기반 인증 (웹)
- BCrypt 비밀번호 해싱

### 비밀번호 정책
- 길이: 6-16자
- 복잡도: 영문 대/소문자, 숫자, 특수문자 중 최소 2가지 조합
- 제약사항:
  - ID와 동일한 비밀번호 불가
  - 공백 포함 불가
  - 이전 비밀번호와 동일 불가

### CSRF 보호
- CodeIgniter CSRF 토큰 사용
- API 엔드포인트는 CSRF 보호 비활성화

### CORS 설정
- API에서 모든 오리진 허용 (`Access-Control-Allow-Origin: *`)
- GET, POST, OPTIONS, PUT, DELETE 메서드 허용

---

## 6. 환경 설정

### 애플리케이션 환경
- **현재 환경**: Development
- **에러 리포팅**: 모든 에러 표시 (개발 환경)
- **기본 컨트롤러**: `welcome`
- **404 핸들러**: `my404`

### 데이터베이스 설정
```php
// 기본 DB (AWS RDS)
hostname: kyowon.cu0nkqepynqr.ap-northeast-2.rds.amazonaws.com
database: kyowon
username: infinity_ksys_db

// 로컬 DB
hostname: localhost
database: francisparker
username: francisparker
```

---

## 7. 사용자 역할

### 1. 학부모 (Parents)
- 자녀 정보 조회
- 앨범/급식/투약/귀가 정보 확인
- 공지사항 확인
- 알림장 확인
- 메시지 수신

### 2. 교사 (Teacher)
- 학생 관리
- 일일 기록 작성
- 알림장 작성

### 3. 원장 (Director)
- 전체 관리 기능
- 앨범/급식/투약/귀가 관리
- 공지사항 작성
- 통계 조회

### 4. 사무실 (Office)
- 행정 업무
- 문서 관리

---

## 8. 외부 의존성

### Composer 패키지
```json
{
  "require": {
    "firebase/php-jwt": "^5.2"
  }
}
```

### AWS 서비스
- **S3**: 이미지 저장 (버킷: kyowon-edusys)
  - 경로 패턴: `_STUPIC_/{학생번호}.png`
- **RDS**: MySQL 데이터베이스

---

## 9. API 엔드포인트

### 인증
- JWT 토큰 기반
- 헤더: `token: {JWT_TOKEN}`
- Secret Key: `this-is-the-secret`

### 주요 엔드포인트 (추정)
- `POST /api/push`: 푸시 알림 발송
- `POST /api/pushtest`: 푸시 테스트
- 기타 CRUD 엔드포인트

---

## 10. 개발 환경

### 요구사항
- PHP 5.6+ (권장: PHP 7.x)
- MySQL 5.6+
- Apache/Nginx 웹 서버
- Composer

### 로컬 개발 설정
1. 데이터베이스 설정 (`application/config/database.php`)
2. 베이스 URL 설정 (`application/config/config.php`)
3. Composer 의존성 설치: `composer install`
4. 웹 서버 실행

---

## 11. 배포 정보

### 프로덕션 URL
- `https://newapp.francisparker.kr/`

### 환경 변수
- `CI_ENV`: 환경 설정 (development/testing/production)

---

## 12. 주요 특징

### 1. MVC 아키텍처
- CodeIgniter의 MVC 패턴 준수
- 명확한 관심사 분리

### 2. 모듈화된 구조
- 기능별 컨트롤러 분리
- 재사용 가능한 모델
- 뷰 템플릿 시스템

### 3. 모바일 앱 연동
- RESTful API 제공
- JWT 인증
- 푸시 알림 지원

### 4. 클라우드 통합
- AWS S3 이미지 저장
- AWS RDS 데이터베이스

### 5. 다중 사용자 지원
- 학부모, 교사, 원장, 사무실 역할 구분
- 역할 기반 접근 제어

---

## 13. 개선 제안

### 보안
1. JWT Secret Key를 환경 변수로 분리
2. 데이터베이스 자격 증명을 환경 변수로 관리
3. CORS 설정을 특정 도메인으로 제한
4. HTTPS 강제 적용

### 코드 품질
1. PHP 버전 업그레이드 (PHP 8.x)
2. CodeIgniter 4.x로 마이그레이션 고려
3. 에러 핸들링 개선
4. 로깅 시스템 강화

### 성능
1. 데이터베이스 쿼리 최적화
2. 캐싱 전략 구현
3. 이미지 최적화 및 CDN 사용

### 유지보수
1. API 문서화 (Swagger/OpenAPI)
2. 단위 테스트 작성
3. CI/CD 파이프라인 구축
4. 버전 관리 전략 수립

---

## 14. 문서 정보

- **작성일**: 2025-09-30
- **분석 도구**: Windsurf AI
- **프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/02-web-development/innerview-project/fp_app`

---

## 15. 참고 자료

- [CodeIgniter 3 문서](https://codeigniter.com/userguide3/)
- [Firebase PHP-JWT](https://github.com/firebase/php-jwt)
- [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/)
