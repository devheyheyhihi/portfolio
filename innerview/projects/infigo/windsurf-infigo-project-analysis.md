# Windsurf 프로젝트 분석 - Infigo 시스템

## 📋 프로젝트 개요

**프로젝트명**: Infigo (Infinity Solutions)  
**기술 스택**: PHP, MySQL, JavaScript  
**프로젝트 유형**: 전시/행사 관리 및 물품 관리 시스템  
**회사**: 인피니티 (Infinity Solutions)  
**Git 저장소**: https://git.innerviewit.com/infigo/php.git

## 🎯 시스템 목적

Infigo는 전시/행사 관리, 부스 관리, 물품 재고 관리, 판촉물 관리를 통합한 업무 관리 시스템입니다. 내부 직원과 외부 회원이 모바일 앱 및 웹을 통해 행사 신청, 물품 주문, 재고 관리 등을 수행할 수 있습니다.

## 🏗️ 시스템 아키텍처

### 디렉토리 구조

```
infigo/
├── admin/              # 관리자 페이지
├── api/                # 모바일 앱용 REST API
├── asia_pdf/           # PDF 관련 파일
├── brochure/           # 판촉물 관리
├── cert/               # 인증서 파일
├── common/             # 공통 라이브러리 및 리소스
│   ├── ajax/          # AJAX 데이터 처리
│   ├── excel/         # 엑셀 처리
│   ├── html/          # 공통 HTML 컴포넌트
│   ├── js/            # JavaScript 파일
│   └── lib/           # PHP 라이브러리
├── download/           # 다운로드 파일
├── event/              # 행사 관리
├── files/              # 파일 저장소
├── images/             # 이미지 리소스
├── infi_web/           # 웹 페이지
├── login/              # 로그인 페이지
├── member/             # 회원 관리
├── notice/             # 공지사항
├── order/              # 물품 주문
├── policy/             # 정책 페이지
├── popup/              # 팝업 관리
├── product/            # 물품 관리
├── report/             # 보고서
├── sessions/           # 세션 저장소
├── sms/                # SMS 발송
├── upload/             # 업로드 파일 저장소
├── config.ini          # 설정 파일
└── index.php           # 메인 진입점
```

## 🔧 핵심 기능

### 1. 사용자 관리
- **내부 사용자 (INNER)**: 회사 직원
  - 최고관리자 (CODE_TEAM)
  - 전시팀 (CODE_TEAM_EXPO): 행사 관리 권한
  - 물류팀 (CODE_TEAM_WARE): 부스/물품 관리 권한
  - 디자인팀 (CODE_TEAM_DESIGN): 판촉물 관리 권한
  - 마케팅팀 (CODE_TEAM_MARKET): 판촉물 관리 권한

- **외부 사용자 (OUTER)**: 외부 회원
  - PM (JOBKEY_PM)
  - AD (JOBKEY_AD)
  - CAD (JOBKEY_CAD)
  - 팀 마스터 (JOBKEY_TM)
  - 전체 마스터 (JOBKEY_MASTER)

### 2. 행사 관리 (Event Management)
행사의 전체 라이프사이클을 관리합니다.

**행사 상태 코드**:
- `005001` - 신청 (APPLY)
- `005002` - 승인 (APPROVAL)
- `005003` - 설치출발 (INSTALL_START)
- `005004` - 설치완료 (INSTALL_END)
- `005005` - 철거완료 (DEMOLISH)
- `005010` - 취소 (CANCEL)

**주요 기능**:
- 행사 신청 및 승인
- 행사 일정 관리
- 행사 담당자 관리
- 행사 주소 관리
- 설치/철거 상태 추적
- 행사 비용 관리 (숙박비: 100,000원/일)

### 3. 물품 관리 (Product Management)
재고 관리 및 물품 추적 시스템

**물품 상태 코드**:
- `008001` - 정상 (OK)
- `008002` - 폐기 (DESTROY)
- `008010` - 숨김 (HIDDEN)

**재고 유형**:
- `004001` - 입고 (IN)
- `004002` - 출고 (OUT)
- `004003` - 반품폐기 (DESTROY)

**주요 기능**:
- 물품 등록 및 관리
- 재고 수량 추적
- 물품 수량 부족 알림 (기준: 10개 이하)
- 자동 재고 차감 (행사/주문 기반)
- 물품 이미지 관리

### 4. 부스 관리 (Booth Management)
전시 부스의 상태 및 배정 관리

**부스 상태 코드**:
- `002001` - 정상 (OK)
- `002002` - 수리중 (REPAIR)
- `002003` - 폐기 (DESTROY)
- `002010` - 숨김 (HIDDEN)

**용처 구분**:
- `009001` - 단독 사용
- `009002` - 팀 공용
- `009003` - 회사 공용

### 5. 물품 주문 시스템 (Order System)
외부 회원의 물품 신청 및 관리

**주문 상태 코드**:
- `007001` - 신청 (APPLY)
- `007002` - 확인 (APPROVAL)
- `007003` - 출고 (RELEASE)
- `007004` - 완료 (DONE)
- `007010` - 취소 (CANCEL)
- `007011` - 분실 (MISSED)

### 6. 판촉물 관리 (Brochure Management)
마케팅 자료 및 판촉물 관리

### 7. 명함첩 시스템
- `010001` - 물품신청 수령자
- `010002` - 행사 주소
- `010003` - 행사 담당자

## 🔌 API 시스템

### API 엔드포인트 목록

| 엔드포인트 | 설명 |
|-----------|------|
| `/api/login.php` | 로그인 인증 (내부/외부 사용자) |
| `/api/event_list.php` | 행사 목록 조회 |
| `/api/event_view.php` | 행사 상세 조회 |
| `/api/event_save.php` | 행사 저장 |
| `/api/event_process.php` | 행사 상태 변경 |
| `/api/my_event_list.php` | 내 행사 목록 |
| `/api/product_list.php` | 물품 목록 조회 |
| `/api/order_save.php` | 물품 주문 |
| `/api/order_view.php` | 주문 상세 조회 |
| `/api/my_order_list.php` | 내 주문 목록 |
| `/api/booth_list.php` | 부스 목록 조회 |
| `/api/category_list.php` | 카테고리 목록 |
| `/api/area_list.php` | 지역 목록 |
| `/api/shiptype_list.php` | 배송 유형 목록 |
| `/api/notice_list.php` | 공지사항 목록 |
| `/api/notice_view.php` | 공지사항 상세 |
| `/api/popup_list.php` | 팝업 목록 |
| `/api/namecard_list.php` | 명함첩 목록 |
| `/api/applymember_list.php` | 신청 회원 목록 |
| `/api/push.php` | 푸시 알림 발송 |
| `/api/push_i.php` | iOS 푸시 |
| `/api/push_a.php` | Android 푸시 |

### API 로깅
- API 로그 사용: `Y`
- 로그 저장 경로: `/upload/apilog`

## 🗄️ 데이터베이스 구조

### 주요 테이블 (추정)

#### 사용자 관련
- `tb_admin` - 내부 관리자
- `tb_admin_grade` - 관리자 등급
- `tb_admin_log` - 관리자 로그
- `tb_company` - 회사 정보
- `tb_company_member` - 외부 회원
- `tb_company_member_log` - 회원 로그
- `tb_company_team` - 회사 팀
- `tb_company_job` - 직무

#### 행사 관련
- `tb_event` - 행사 정보
- `tb_event_booth` - 행사별 부스 배정
- `tb_event_product` - 행사별 물품 배정

#### 물품 관련
- `tb_product` - 물품 정보
- `tb_booth` - 부스 정보
- `tb_order` - 주문 정보
- `tb_order_stock` - 주문별 재고

#### 기타
- `tb_common_code` - 공통 코드
- `tb_notice` - 공지사항
- `tb_popup` - 팝업
- `tb_namecard` - 명함첩

## ⚙️ 시스템 설정 (config.ini)

### 데이터베이스 설정
```ini
[db]
db_hostname = "211.169.231.242"
db_portnum  = "3306"
db_userid   = "infigo"
db_password = "infigo!@#"
db_dbname   = "infigo"
```

### 호스트 설정
```ini
[host]
host_http  = "http://localhost"
host_https = "https://www.infigo.co.kr"
https_force = "N"
```

### SMS 설정
```ini
[sms]
sms_use      = "N"
sms_id       = "infigo_sms"
sms_apikey   = "6f0e6123994ee703a6d237715ad8fe92"
sms_callback = "02-2265-4776"
```

### SMTP 메일 설정
```ini
[smtp]
smtp_use    = "N"
smtp_host   = "mail.infinitysolutions.co.kr"
smtp_port   = "587"
smtp_name   = "인피니티"
smtp_email  = "infigo@infinitysolutions.co.kr"
smtp_id     = "infigo@infinitysolutions.co.kr"
smtp_pw     = "infigo1004"
smtp_secure = "N"
```

### 푸시 알림 설정
```ini
[push]
push_product = "N"  # N: 샌드박스, Y: 프로덕션
```

## 🔐 보안 기능

### 비밀번호 암호화
- AES 암호화 사용
- MD5 해시를 키로 사용
- SQL: `HEX(AES_ENCRYPT(password, MD5(password)))`

### 세션 관리
- 세션 기반 인증
- 로그인 여부 체크: `isLogin()`
- 회원 정보 조회: `getMemberInfo()`

### 접근 제어
- 팀별 권한 분리
- 페이지별 권한 체크
- 삭제된 사용자 로그인 차단
- 비정규직 로그인 차단

## 📱 모바일 앱 지원

### 디바이스 토큰 관리
- iOS/Android 푸시 알림
- 디바이스 타입 및 토큰 저장
- 마지막 로그인 시간 추적

### 푸시 알림 유형
- 행사 상태 변경 알림
- 주문 상태 변경 알림
- 공지사항 알림

### APNS 인증서
- 개발용: `apns-dev.pem`

## 🚀 CI/CD 파이프라인

### GitLab CI 설정 (.gitlab-ci.yml)
```yaml
deployServer:
  stage: deploy
  tags:
    - share
  script:
    - lftp를 통한 FTP 배포
    - common, event, order, popup, product 디렉토리 동기화
  only:
    - main
```

### 배포 대상
- FTP 서버: 222.231.29.202
- 계정: infigo
- 자동 배포 디렉토리:
  - `/common`
  - `/event`
  - `/order`
  - `/popup`
  - `/product`

## 📂 파일 업로드 경로

### 이미지 업로드
- 회사 로고: `/upload/image/logo`
- 물품 이미지: `/upload/image/product`
- 부스 이미지: `/upload/image/booth`
- 행사 이미지: `/upload/image/event`
- 판촉물 이미지: `/upload/image/brochure/product`
- 팝업 이미지 (웹): `/upload/image/popup/web`
- 팝업 이미지 (앱): `/upload/image/popup/app`

### 첨부파일 업로드
- 물품 첨부: `/upload/attach/product`
- 부스 첨부: `/upload/attach/booth`
- 행사 첨부: `/upload/attach/event`
- 판촉물 첨부: `/upload/attach/brochure`

### 기타
- 회사 엑셀: `/upload/companyexcel`
- API 로그: `/upload/apilog`

## 🎨 UI/UX 기능

### 테마 시스템
사용자별 테마 선택 가능:
- 기본 (그린)
- 빨강 (red)
- 주황 (orange)
- 겨자 (mustard)
- 청록 (cyan)
- 하늘 (skyblue)
- 파랑 (blue)
- 보라 (purple)

### 페이지 뷰 옵션
- 40개씩 보기
- 80개씩 보기
- 160개씩 보기
- 320개씩 보기

### 주소 검색
- 다음 우편번호 API 연동
- HTTP/HTTPS 자동 전환

## 🔔 알림 시스템

### SMS 알림
- 사용 여부: 설정 가능
- 발신번호: 02-2265-4776

### 이메일 알림
- SMTP 발송
- 회신 이메일: infigo@infinitysolutions.co.kr

### 푸시 알림
- iOS (APNS)
- Android (FCM)
- 수신 동의 플래그:
  - SMS: 0x01
  - 이메일: 0x02
  - 푸시: 0x04

## 📊 로깅 시스템

### 로그 유형
- `003001` - 시스템 로그
- `003002` - 사용자 액션 로그

### 로그 기록 내용
- 로그인/로그아웃
- 행사 신청/승인/취소
- 물품 주문/출고/완료
- 푸시 알림 확인
- IP 주소
- 타임스탬프

## 🔄 라우팅 시스템

### 메인 진입점 (index.php)
로그인 후 사용자 유형에 따라 자동 리다이렉트:

**내부 사용자**:
- 최고관리자 → `/notice/notice_list.php`
- 물류팀 → `/product/product_list.php`
- 전시팀 → `/event/event_list.php`
- 디자인팀 → 개발 중

**외부 사용자**:
- 기본 → `/event/event_list.php`

## 🛠️ 개발 환경

### 필수 라이브러리
- PHP (버전 명시 안됨, 추정 7.x+)
- MySQL/MariaDB
- jQuery
- AJAX
- Excel 처리 라이브러리
- PDF 처리 라이브러리

### 공통 라이브러리 파일
- `_header.php` - 공통 헤더 (웹용)
- `_header_api.php` - 공통 헤더 (API용)
- `defines.php` - 상수 정의
- `functions.php` - 공통 함수
- `functions_sql.php` - SQL 관련 함수
- `sessions.php` - 세션 관리
- `infinity.php` - 비즈니스 로직
- `infinity_event.php` - 행사 관련 로직
- `infinity_order.php` - 주문 관련 로직
- `msg_txt.php` - 메시지 텍스트
- `errorlevel.php` - 에러 레벨 설정
- `db.php` - 데이터베이스 클래스

## 📈 비즈니스 규칙

### 재고 관리
- 물품 수량 부족 알림 기준: 10개
- 자동 재고 차감:
  - 수동 차감 (011001)
  - 행사 기반 차감 (011002)
  - 주문 기반 차감 (011003)

### 행사 관리
- 숙박비: 100,000원/일
- 행사 상태는 순차적으로 진행
- 취소 시 재고 복구 필요

### 비밀번호 정책
- 최소 길이: 4자 (개발 환경)
- 프로덕션 권장: 10자

## 🔍 주요 특징

1. **멀티 플랫폼 지원**: 웹 + 모바일 앱 (iOS/Android)
2. **역할 기반 접근 제어**: 내부/외부 사용자, 팀별 권한
3. **실시간 상태 추적**: 행사, 주문, 재고 상태 실시간 업데이트
4. **통합 알림 시스템**: SMS, 이메일, 푸시 알림
5. **재고 자동화**: 행사/주문 기반 자동 재고 차감
6. **로깅 시스템**: 모든 사용자 액션 추적
7. **CI/CD 자동화**: GitLab CI를 통한 자동 배포
8. **테마 커스터마이징**: 사용자별 UI 테마 선택

## 🚨 보안 고려사항

### 현재 보안 이슈
1. **설정 파일 노출**: `config.ini`에 DB 비밀번호 평문 저장
2. **HTTPS 미적용**: 강제 HTTPS 설정이 'N'
3. **SMS/SMTP 미사용**: 알림 기능 비활성화 상태
4. **에러 표시**: 프로덕션에서 에러 표시 설정 확인 필요

### 권장 개선사항
1. 환경 변수로 민감 정보 관리
2. HTTPS 강제 적용
3. 비밀번호 정책 강화 (최소 10자)
4. SQL Injection 방어 강화
5. XSS 방어 강화
6. CSRF 토큰 적용

## 📝 개발 가이드

### 새 페이지 추가 시
1. 공통 헤더 포함: `include_once $_SERVER["DOCUMENT_ROOT"]."/common/lib/_header.php";`
2. 로그인 체크: `isLogin()`
3. 권한 체크: 팀 코드 확인
4. 공통 HTML 컴포넌트 사용

### 새 API 추가 시
1. API 헤더 포함: `include_once $_SERVER["DOCUMENT_ROOT"]."/common/lib/_header_api.php";`
2. API 로그 작성: `writeApiLog();`
3. 파라미터 검증: `checkSubmitValue()`
4. JSON 응답: `JSONApi()`

### 데이터베이스 쿼리
1. Db 클래스 사용
2. Quote 메서드로 SQL Injection 방어
3. 트랜잭션 처리 필요 시 명시

## 📞 연락처 및 지원

- 회사명: 인피니티 (Infinity Solutions)
- 전화번호: 02-2265-4776
- 이메일: infigo@infinitysolutions.co.kr
- 웹사이트: https://www.infigo.co.kr

---

**문서 작성일**: 2025-09-30  
**분석 도구**: Windsurf AI  
**프로젝트 상태**: 운영 중
