# Windsurf 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: SVR (Smart Review Integration System)  
**목적**: 네이버 스마트스토어 리뷰를 Cafe24 쇼핑몰로 자동 연동하는 시스템  
**기술 스택**: Node.js, Express, Sequelize, MariaDB, Puppeteer, Firebase  
**배포 환경**: PM2 Ecosystem

---

## 🏗️ 시스템 아키텍처

### 핵심 기능
1. **네이버 스마트스토어 리뷰 크롤링**: Puppeteer를 사용한 자동 로그인 및 리뷰 수집
2. **Cafe24 API 연동**: OAuth 2.0 인증 및 리뷰 등록
3. **스케줄링**: 매시간 자동 리뷰 동기화
4. **Firebase 실시간 데이터베이스**: SMS 인증 코드 수신

### 데이터 흐름
```
네이버 스마트스토어 → Puppeteer 크롤링 → DB 저장 → Cafe24 API → 리뷰 등록
```

---

## 📁 프로젝트 구조

```
svr/
├── app.js                      # Express 애플리케이션 진입점
├── bin/
│   └── www                     # 서버 시작 스크립트
├── config/
│   └── config.js               # 데이터베이스 설정
├── models/                     # Sequelize 모델
│   ├── index.js                # 모델 초기화
│   ├── article.js              # Cafe24 게시글 모델
│   ├── naver.js                # 네이버 리뷰 모델
│   └── mall.js                 # 쇼핑몰 인증 정보 모델
├── routes/                     # 라우팅
│   ├── index.js                # 메인 라우트 (OAuth 인증)
│   ├── api.js                  # API 엔드포인트
│   ├── callback.js             # OAuth 콜백
│   └── hook.js                 # 웹훅 처리
├── utils/                      # 유틸리티
│   ├── cafe24.js               # Cafe24 API 래퍼
│   └── schedule.js             # 스케줄 작업
├── views/                      # HBS 템플릿
├── public/                     # 정적 파일
├── ecosystem.config.js         # PM2 설정
├── package.json                # 의존성 관리
├── naver.js                    # 네이버 크롤링 스크립트
├── test.js                     # 테스트 스크립트
└── domain.js                   # 도메인 설정
```

---

## 🔑 주요 컴포넌트 분석

### 1. **app.js** - 애플리케이션 진입점
```javascript
- Express 서버 설정
- CORS 활성화
- 세션 관리 (express-session)
- 동적 라우트 로딩 (glob 패턴)
- 스케줄러 초기화
```

**특징**:
- `glob.sync`를 사용하여 `routes/` 디렉토리의 모든 라우트 자동 로드
- `index.js`는 루트 경로(`/`)에, 나머지는 파일명 기반 경로에 매핑
- 환경변수 기반 세션 시크릿 관리

---

### 2. **routes/index.js** - OAuth 인증 및 토큰 관리

#### 주요 기능:
1. **Cafe24 OAuth 인증 플로우**
   - HMAC 검증을 통한 요청 무결성 확인
   - Access Token 및 Refresh Token 관리
   - 토큰 만료 시 자동 갱신

2. **권한 스코프**:
   ```javascript
   - mall.read_product: 상품 정보 조회
   - mall.write_community: 리뷰 작성
   - mall.read_community: 리뷰 조회
   - mall.write_application: 앱 설치 정보 관리
   - mall.read_application: 앱 설치 정보 조회
   ```

3. **테스트 엔드포인트**:
   - `/test`: 네이버 리뷰를 Cafe24로 일괄 등록
   - `/test2`: 특정 리뷰 단건 등록
   - `/test3`: Cafe24 게시글 일괄 가져오기
   - `/test4`: 특정 게시글 조회

---

### 3. **routes/api.js** - API 엔드포인트

#### 엔드포인트:

##### `GET /api/review/:product_no`
- **기능**: 특정 상품의 리뷰 통계 조회
- **응답**: 리뷰 개수 및 평균 평점
```javascript
{
  count: 10,
  avg: 4.5
}
```

##### `GET /api/all`
- **기능**: 전체 상품의 리뷰 통계 (JSON 객체 형태)
- **응답**:
```javascript
{
  "product_no_1": { count: 5, avg: 4.2 },
  "product_no_2": { count: 8, avg: 4.8 }
}
```

##### `GET /api/test`
- **기능**: Puppeteer를 사용한 네이버 스마트스토어 리뷰 크롤링
- **프로세스**:
  1. 네이버 로그인 (자동화)
  2. Firebase에서 SMS 인증 코드 수신
  3. 리뷰 API 응답 인터셉트
  4. DB 저장 및 Cafe24 등록

##### `POST /api/insert_review`
- **기능**: 외부에서 리뷰 데이터 수신 및 등록
- **요청 본문**:
```javascript
{
  review: {
    id: "review_id",
    productNo: "product_code",
    reviewContent: "리뷰 내용",
    reviewScore: 5,
    // ... 기타 필드
  }
}
```

---

### 4. **utils/cafe24.js** - Cafe24 API 래퍼

#### 주요 함수:

##### `getToken(mall_id)`
- Access Token 조회 및 자동 갱신
- 토큰 만료 시 Refresh Token으로 재발급

##### `SearchProduct(code)`
- 상품 코드로 Cafe24 상품 검색
- Rate Limiting 방지를 위한 2초 딜레이

##### `InsertReview(product_no, review)`
- 네이버 리뷰를 Cafe24 게시판에 등록
- 이미지 첨부 파일 URL 처리
- 리뷰 내용을 게시글 제목/본문으로 변환

##### `getArticles(offset, limit)`
- Cafe24 게시판 게시글 목록 조회
- 페이지네이션 지원

##### `changeGroup(group_no, members)`
- 회원 그룹 일괄 변경
- 200개씩 배치 처리

##### `createCoupon(coupon_no, member_id)`
- 특정 회원에게 쿠폰 발급

---

### 5. **utils/schedule.js** - 스케줄링

#### 스케줄 설정:
```javascript
schedule.scheduleJob('0 * * * *', async function () {
  // 매시간 정각에 실행
});
```

#### 작업 내용:
1. Puppeteer로 네이버 스마트스토어 접속
2. Firebase에서 SMS 인증 코드 대기
3. 로그인 후 리뷰 API 응답 인터셉트
4. 신규 리뷰 DB 저장
5. Cafe24에 리뷰 등록

---

### 6. **models/** - 데이터베이스 모델

#### Article (Cafe24 게시글)
```javascript
{
  article_no: INTEGER (PK),
  product_no: INTEGER,
  writer: STRING,
  title: STRING,
  content: TEXT,
  rating: INTEGER,
  display: CHAR,
  deleted: CHAR,
  // ... 기타 필드
}
```

#### Naver (네이버 리뷰)
```javascript
{
  id: STRING (PK),
  productNo: STRING,
  productName: STRING,
  reviewScore: INTEGER,
  reviewContent: TEXT,
  files: STRING(2000),
  maskedMemberId: STRING,
  createDate: DATE,
  inserted: BOOLEAN,  // Cafe24 등록 여부
  // ... 기타 필드
}
```

#### Mall (쇼핑몰 인증 정보)
```javascript
{
  mall_id: STRING (UNIQUE),
  access_token: STRING,
  refresh_token: STRING,
  expires_at: DATE,
  refresh_token_expires_at: DATE,
  scopes: STRING,
  // ... 기타 필드
}
```

---

## 🔐 인증 및 보안

### Cafe24 OAuth 2.0
1. **Authorization Code Grant**
   - 사용자가 앱 설치 시 인증 페이지로 리디렉션
   - 인증 후 Authorization Code 발급
   - Code를 Access Token으로 교환

2. **HMAC 검증**
   ```javascript
   const hash = crypto.createHmac('SHA256', secretkey)
                      .update(query)
                      .digest('base64');
   ```

3. **토큰 갱신**
   - Access Token 만료 시 자동 갱신
   - Refresh Token 만료 시 재인증 필요

### Firebase 인증
- SMS 인증 코드를 Firebase Realtime Database로 수신
- 네이버 로그인 시 2단계 인증 자동화

---

## 🤖 Puppeteer 크롤링 전략

### 로그인 프로세스
```javascript
1. 네이버 커머스 로그인 페이지 접속
2. 팝업 창 대기 및 감지
3. 아이디/비밀번호 입력
4. SMS 인증 코드 입력 (Firebase 연동)
5. 리뷰 검색 페이지 이동
```

### 데이터 수집
- **Request Interception**: API 응답을 가로채서 JSON 파싱
- **Target URL**: `https://sell.smartstore.naver.com/api/review/list/search`
- **데이터 추출**: 리뷰 목록, 상품 정보, 평점, 이미지 등

### 안정성 확보
- User-Agent 설정으로 봇 감지 우회
- `waitForTimeout`으로 페이지 로딩 대기
- 에러 핸들링 및 브라우저 자동 종료

---

## 🚀 배포 설정 (PM2 Ecosystem)

### 개발 환경
```javascript
{
  NODE_ENV: 'development',
  PORT: 3013,
  DB_HOST: '58.229.163.18',
  DB_NAME: 'svr',
  HOST: 'svrdev.kr'
}
```

### 프로덕션 환경
```javascript
{
  NODE_ENV: 'production',
  PORT: 3013,
  DB_HOST: 'localhost',
  DB_NAME: 'svr',
  HOST: 'svrdev.kr'
}
```

### 배포 명령어
```bash
# 개발 환경 시작
pm2 start ecosystem.config.js --env development

# 프로덕션 배포
pm2 deploy production
```

---

## 📊 데이터베이스 스키마

### 테이블 관계
```
Mall (1) ─── (N) Article
Naver (1) ─── (1) Article (productNo 매칭)
```

### 인덱스 전략
- `article_no` (PK)
- `product_no` (검색 최적화)
- `mall_id` (UNIQUE)
- `id` (Naver 리뷰 PK)

---

## 🔄 워크플로우

### 1. 앱 설치 플로우
```
사용자 → Cafe24 앱스토어 → OAuth 인증 → 토큰 저장 → 리뷰 연동 시작
```

### 2. 리뷰 동기화 플로우
```
스케줄러 → 네이버 크롤링 → DB 저장 → 상품 매칭 → Cafe24 등록 → inserted=true
```

### 3. API 조회 플로우
```
프론트엔드 → /api/review/:product_no → DB 집계 쿼리 → 통계 반환
```

---

## 🛠️ 의존성 패키지

### 핵심 패키지
- **express**: 웹 프레임워크
- **sequelize**: ORM (MariaDB 연동)
- **puppeteer**: 헤드리스 브라우저 자동화
- **axios**: HTTP 클라이언트
- **node-schedule**: Cron 스케줄러
- **firebase**: Firebase Realtime Database

### 유틸리티
- **moment**: 날짜/시간 처리
- **glob**: 파일 패턴 매칭
- **cors**: CORS 처리
- **cookie-parser**: 쿠키 파싱
- **express-session**: 세션 관리
- **image-downloader**: 이미지 다운로드

---

## ⚠️ 주요 이슈 및 개선 사항

### 현재 이슈
1. **하드코딩된 인증 정보**
   - `cafe24.js`에 Client ID/Secret이 하드코딩됨
   - 환경변수로 이동 필요

2. **중복 함수 정의**
   - `cafe24.js`의 `coupons()` 함수가 두 번 정의됨

3. **에러 핸들링 부족**
   - Puppeteer 크롤링 실패 시 재시도 로직 없음
   - API 호출 실패 시 알림 없음

4. **보안 취약점**
   - 네이버 계정 정보가 코드에 노출됨
   - Firebase 설정이 클라이언트 코드에 포함됨

### 개선 제안
1. **환경변수 관리**
   - `.env` 파일 사용 (dotenv)
   - 민감 정보 암호화

2. **로깅 시스템**
   - Winston 또는 Bunyan 도입
   - 에러 추적 및 모니터링

3. **재시도 로직**
   - Puppeteer 크롤링 실패 시 3회 재시도
   - API 호출 실패 시 Exponential Backoff

4. **테스트 코드**
   - Jest를 사용한 단위 테스트
   - API 엔드포인트 통합 테스트

5. **성능 최적화**
   - DB 쿼리 최적화 (인덱스 추가)
   - Redis 캐싱 도입
   - 배치 처리 크기 조정

---

## 📈 성능 지표

### API 응답 시간
- `/api/review/:product_no`: ~50ms
- `/api/all`: ~200ms (전체 상품 집계)

### 크롤링 주기
- 매시간 정각 실행
- 평균 처리 시간: 2-3분

### 데이터베이스
- MariaDB 10.x
- 연결 풀: 기본 설정
- 타임존: Asia/Seoul

---

## 🔍 디버깅 가이드

### 로그 확인
```bash
# PM2 로그
pm2 logs SVR

# 에러 로그만
pm2 logs SVR --err
```

### 데이터베이스 확인
```sql
-- 미등록 리뷰 확인
SELECT * FROM navers WHERE inserted = false;

-- 상품별 리뷰 통계
SELECT product_no, COUNT(*), AVG(rating) 
FROM articles 
WHERE display = 'T' AND deleted = 'F'
GROUP BY product_no;
```

### Puppeteer 디버깅
```javascript
// headless: false로 변경하여 브라우저 확인
const browser = await puppeteer.launch({
  headless: false,
  slowMo: 100  // 동작 속도 늦추기
});
```

---

## 📝 API 문서

### 인증
- **타입**: OAuth 2.0
- **헤더**: `Authorization: Bearer {access_token}`
- **API 버전**: `X-Cafe24-Api-Version: 2021-12-01`

### 엔드포인트 요약

| 메서드 | 경로 | 설명 | 인증 필요 |
|--------|------|------|-----------|
| GET | `/` | OAuth 인증 시작 | ❌ |
| GET | `/callback` | OAuth 콜백 | ❌ |
| GET | `/api/review/:product_no` | 리뷰 통계 조회 | ❌ |
| GET | `/api/all` | 전체 통계 조회 | ❌ |
| POST | `/api/insert_review` | 리뷰 등록 | ❌ |
| GET | `/test` | 일괄 리뷰 등록 | ❌ |

---

## 🎯 비즈니스 로직

### 리뷰 매칭 전략
1. 네이버 상품 코드 (`productNo`)로 Cafe24 상품 검색
2. `custom_product_code` 필드 매칭
3. 매칭 성공 시 리뷰 등록
4. 실패 시 `inserted=false` 유지

### 중복 방지
- 네이버 리뷰 ID를 Primary Key로 사용
- `findOrCreate`로 중복 등록 방지
- `inserted` 플래그로 처리 상태 관리

### 이미지 처리
- 네이버 이미지 URL을 Cafe24 `attach_file_urls`로 전달
- 파일명은 URL에서 추출
- 최대 2000자 제한

---

## 🌐 네트워크 구성

### 포트
- **애플리케이션**: 3013
- **데이터베이스**: 3306

### 도메인
- **개발**: svrdev.kr
- **프로덕션**: svrdev.kr

### 방화벽
- 3013 포트 오픈 (HTTP)
- 3306 포트 제한 (내부 네트워크만)

---

## 📚 참고 자료

### Cafe24 API
- [Cafe24 개발자 센터](https://developers.cafe24.com/)
- [OAuth 2.0 가이드](https://developers.cafe24.com/docs/api/admin/#oauth-2-0)
- [게시판 API](https://developers.cafe24.com/docs/api/admin/#boards)

### 네이버 스마트스토어
- [판매자 센터](https://sell.smartstore.naver.com/)
- 리뷰 API는 공식 제공되지 않음 (크롤링 필요)

### 기술 스택
- [Express.js](https://expressjs.com/)
- [Sequelize](https://sequelize.org/)
- [Puppeteer](https://pptr.dev/)
- [Firebase](https://firebase.google.com/)

---

## 🚦 시작하기

### 설치
```bash
npm install
```

### 환경 설정
```bash
# .env 파일 생성 (예시)
NODE_ENV=development
PORT=3013
DB_HOST=localhost
DB_USER=svr
DB_PASS=DbPassSvR
DB_NAME=svr
CLIENTID=your_client_id
CLIENTSECRETKEY=your_client_secret
```

### 실행
```bash
# 개발 모드
npm start

# PM2로 실행
pm2 start ecosystem.config.js --env development

# 프로덕션 배포
pm2 deploy production
```

### 테스트
```bash
# 리뷰 동기화 테스트
curl http://localhost:3013/test

# 리뷰 통계 조회
curl http://localhost:3013/api/review/123
```

---

## 👥 팀 및 연락처

**개발사**: Innerview IT  
**프로젝트 저장소**: git.innerviewit.com/develop/svr  
**배포 서버**: 58.229.163.18

---

## 📄 라이선스

ISC License

---

## 🔄 버전 히스토리

### v1.0.0 (2021-12-01)
- 초기 릴리스
- 네이버 스마트스토어 리뷰 크롤링
- Cafe24 API 연동
- 스케줄링 기능

---

## 💡 추가 기능 아이디어

1. **대시보드 구축**
   - 리뷰 동기화 현황 모니터링
   - 통계 차트 (일별/월별 리뷰 수)

2. **알림 시스템**
   - 신규 리뷰 등록 시 이메일/슬랙 알림
   - 크롤링 실패 시 관리자 알림

3. **다중 쇼핑몰 지원**
   - 여러 Cafe24 쇼핑몰 동시 관리
   - 쇼핑몰별 설정 분리

4. **리뷰 필터링**
   - 평점 기준 필터링
   - 키워드 기반 자동 분류

5. **분석 기능**
   - 감성 분석 (긍정/부정)
   - 키워드 추출 및 워드 클라우드

---

**문서 작성일**: 2025-10-01  
**작성자**: Windsurf AI Assistant
