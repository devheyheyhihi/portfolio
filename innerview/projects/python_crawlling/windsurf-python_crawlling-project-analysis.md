# Windsurf 프로젝트 분석 보고서

## 📋 프로젝트 개요

**프로젝트명**: Naver SmartStore 리뷰 크롤링 시스템  
**작성일**: 2025-10-01  
**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/02-web-development/innerview-project/python_crawlling`

---

## 🎯 프로젝트 목적

네이버 스마트스토어의 상품 리뷰 데이터를 자동으로 수집하여 MySQL 데이터베이스에 저장하는 웹 크롤링 시스템입니다. SVR 공식몰의 화장품 제품 리뷰를 수집하고 분석하기 위한 목적으로 개발되었습니다.

---

## 🏗️ 프로젝트 구조

```
python_crawlling/
├── naver.py                    # 메인 크롤링 스크립트
├── naver.db                    # SQLite 데이터베이스 (로컬 백업용)
├── output.json                 # 크롤링된 리뷰 데이터 샘플
├── chromedriver                # Chrome WebDriver 실행 파일
├── browsermob-proxy-2.1.4/     # 네트워크 트래픽 캡처 프록시
├── bmp.log                     # BrowserMob Proxy 로그
└── server.log                  # 서버 로그
```

---

## 🔧 기술 스택

### 핵심 라이브러리
- **Selenium Wire**: 웹 브라우저 자동화 및 네트워크 요청 인터셉트
- **Selenium**: 웹 페이지 자동화 및 로그인 처리
- **SQLAlchemy**: ORM을 통한 데이터베이스 관리
- **PyPerClip**: 클립보드를 통한 안전한 로그인 정보 입력
- **MySQL Connector**: MySQL 데이터베이스 연결

### 데이터베이스
- **MySQL**: 원격 데이터베이스 (211.169.231.242)
- **SQLite**: 로컬 백업 데이터베이스

---

## 📊 데이터 모델

### Naver 테이블 스키마

| 컬럼명 | 타입 | 설명 |
|--------|------|------|
| `id` | Float (PK) | 리뷰 고유 ID |
| `productNo` | Float | 상품 번호 |
| `productName` | String(500) | 상품명 |
| `reviewType` | String(50) | 리뷰 타입 (NORMAL, AFTER_USE) |
| `reviewScore` | Float | 리뷰 평점 (1-5) |
| `reviewResourceUrl` | String(200) | 리뷰 이미지 URL |
| `files` | String(500) | 첨부 파일 URL 목록 (쉼표 구분) |
| `reviewContent` | String(500) | 리뷰 내용 |
| `helpCount` | Float | 도움이 됨 카운트 |
| `maskedMemberId` | String(200) | 마스킹된 작성자 ID |
| `createDate` | String(50) | 작성일시 |
| `parentReviewContent` | String(500) | 원본 리뷰 내용 (재구매 리뷰의 경우) |
| `escapeHtmlParentReviewContent` | String(500) | HTML 이스케이프된 원본 리뷰 |
| `reviewDisplayStatus` | Boolean | 리뷰 표시 상태 |
| `reviewComment` | String(500) | 리뷰 댓글 |
| `bestReview` | Boolean | 베스트 리뷰 여부 |
| `bestReviewSelectDate` | String(50) | 베스트 리뷰 선정일 |
| `benefitKindType` | String(20) | 혜택 종류 |
| `benefitPaymentDate` | String(50) | 혜택 지급일 |
| `reportYn` | String(1) | 신고 여부 |
| `productOrderNo` | Float | 주문 번호 |
| `memberIdNo` | Float | 회원 ID 번호 |
| `payableBenefit` | Float | 지급 가능 혜택 |
| `productUrl` | String(200) | 상품 URL |
| `mobileProductUrl` | String(200) | 모바일 상품 URL |
| `reviewContentClassType` | String(20) | 리뷰 콘텐츠 타입 (PHOTO, TEXT) |
| `contentsStatusType` | String(100) | 콘텐츠 상태 타입 |

---

## 🔄 작동 원리

### 1. 로그인 프로세스
```python
# 네이버 스마트스토어 셀러 센터 로그인
1. 로그인 페이지 접속
2. 팝업 창으로 전환
3. PyPerClip을 사용하여 ID/PW 입력 (보안 강화)
4. 로그인 버튼 클릭
5. 15초 대기 (2FA 또는 추가 인증 대기)
```

### 2. 데이터 수집 프로세스
```python
# Selenium Wire를 통한 네트워크 요청 캡처
1. 로그인 후 리뷰 검색 페이지 자동 접근
2. API 요청 인터셉트: 
   - URL: https://sell.smartstore.naver.com/api/v3/contents/reviews/search
3. Gzip 압축 해제 및 JSON 파싱
4. 리뷰 데이터 추출 및 변환
5. 중복 체크 후 데이터베이스 저장
```

### 3. 데이터 저장 로직
```python
# 중복 방지 로직
- 기존 리뷰 ID 체크
- 새로운 리뷰만 데이터베이스에 삽입
- 첨부 파일 URL을 쉼표로 구분하여 저장
```

---

## 📈 수집 데이터 통계 (샘플 기준)

### 수집된 리뷰 분석
- **총 리뷰 수**: 15개 (샘플 데이터)
- **리뷰 타입 분포**:
  - NORMAL: 9개 (60%)
  - AFTER_USE: 6개 (40%)
- **콘텐츠 타입 분포**:
  - PHOTO: 9개 (60%)
  - TEXT: 6개 (40%)
- **평균 평점**: 4.87점 (대부분 5점)

### 주요 제품
1. **시카비트+ 피부과 크림 100ML** - 가장 많은 리뷰
2. **선시큐어 블러 SPF50+ 50ML**
3. **리프팅 앰플A 30ML**
4. **세비아클리어 젤 무쌍 폼 클렌저 400ML**

---

## 🔐 보안 고려사항

### 현재 구현된 보안 기능
1. **PyPerClip 사용**: 키보드 입력 대신 클립보드를 통한 로그인 정보 입력
2. **크로스 플랫폼 지원**: Mac (COMMAND+V), Windows (CTRL+V) 모두 지원

### ⚠️ 보안 개선 필요 사항
```python
# 하드코딩된 민감 정보 (라인 56-59, 82, 88)
username = "innerview"
password = "InnerMaria23"
host = "211.169.231.242"

# 네이버 로그인 정보
pyperclip.copy('innerviewit')
pyperclip.copy('$inner23$')
```

**권장 개선 방안**:
- 환경 변수 (.env 파일) 사용
- AWS Secrets Manager 또는 Azure Key Vault 활용
- 설정 파일을 .gitignore에 추가

---

## 🚀 실행 방법

### 사전 요구사항
```bash
# 필수 라이브러리 설치
pip install selenium-wire
pip install selenium
pip install sqlalchemy
pip install mysql-connector-python
pip install pyperclip
```

### 실행 명령
```bash
cd /Users/sinseonghyeon/Documents/GitHub/02-web-development/innerview-project/python_crawlling
python naver.py
```

### 실행 흐름
1. Chrome 브라우저 자동 실행
2. 네이버 스마트스토어 로그인 (수동 2FA 필요 시 15초 대기)
3. 리뷰 데이터 자동 수집
4. MySQL 데이터베이스에 저장
5. 60초 대기 후 브라우저 종료

---

## 📝 주요 기능

### ✅ 구현된 기능
- [x] 네이버 스마트스토어 자동 로그인
- [x] 리뷰 API 요청 인터셉트
- [x] Gzip 압축 데이터 자동 해제
- [x] 중복 리뷰 필터링
- [x] MySQL 데이터베이스 자동 저장
- [x] 리뷰 첨부 이미지 URL 수집
- [x] 크로스 플랫폼 지원 (Mac/Windows)

### 🔄 개선 가능한 기능
- [ ] 페이지네이션 처리 (현재 1페이지만 수집)
- [ ] 스케줄링 기능 (주기적 자동 실행)
- [ ] 에러 핸들링 및 재시도 로직
- [ ] 로깅 시스템 개선
- [ ] 수집 진행률 표시
- [ ] 데이터 검증 및 정제
- [ ] 이미지 다운로드 기능
- [ ] 리뷰 감성 분석 기능

---

## 🐛 알려진 이슈

1. **하드코딩된 대기 시간**
   - `time.sleep(15)`: 로그인 후 고정 대기
   - `time.sleep(60)`: 종료 전 고정 대기
   - 개선: 동적 대기 조건 구현 필요

2. **단일 페이지 수집**
   - 현재 첫 페이지 리뷰만 수집
   - 페이지네이션 로직 필요

3. **에러 처리 부족**
   - 네트워크 오류, 로그인 실패 등에 대한 예외 처리 미흡

4. **보안 취약점**
   - 민감 정보 하드코딩
   - Git 저장소에 노출 위험

---

## 📊 데이터 활용 방안

### 분석 가능한 인사이트
1. **제품별 리뷰 분석**
   - 평점 분포
   - 리뷰 수 추이
   - 베스트 리뷰 선정 패턴

2. **고객 행동 분석**
   - 재구매 리뷰 비율
   - 포토 리뷰 vs 텍스트 리뷰
   - 리뷰 작성 시간대 분석

3. **마케팅 인사이트**
   - 인기 제품 파악
   - 고객 만족도 트렌드
   - 개선 필요 사항 도출

---

## 🔮 향후 개발 계획

### Phase 1: 안정성 개선
- [ ] 환경 변수 분리
- [ ] 에러 핸들링 강화
- [ ] 로깅 시스템 구축

### Phase 2: 기능 확장
- [ ] 전체 페이지 크롤링
- [ ] 스케줄러 구현 (APScheduler)
- [ ] 이미지 다운로드 기능

### Phase 3: 데이터 분석
- [ ] 리뷰 감성 분석 (NLP)
- [ ] 대시보드 구축 (Streamlit/Dash)
- [ ] 리포트 자동 생성

### Phase 4: 확장성
- [ ] 다중 스토어 지원
- [ ] 분산 크롤링 (Celery)
- [ ] API 서버 구축 (FastAPI)

---

## 💡 기술적 특징

### 장점
1. **Selenium Wire 활용**: 일반 Selenium으로는 불가능한 API 요청 캡처
2. **ORM 사용**: SQLAlchemy를 통한 안전한 데이터베이스 작업
3. **중복 방지**: 효율적인 데이터 수집
4. **크로스 플랫폼**: Mac/Windows 모두 지원

### 개선 필요 사항
1. **성능**: 대량 데이터 수집 시 최적화 필요
2. **안정성**: 네트워크 오류 처리 강화
3. **유지보수성**: 설정 파일 분리, 모듈화

---

## 📚 참고 자료

### 사용된 주요 라이브러리 문서
- [Selenium Wire](https://github.com/wkeeling/selenium-wire)
- [SQLAlchemy](https://www.sqlalchemy.org/)
- [Selenium](https://selenium-python.readthedocs.io/)

### 관련 기술
- BrowserMob Proxy: 네트워크 트래픽 캡처
- Chrome DevTools Protocol: 브라우저 자동화

---

## 👥 프로젝트 정보

**개발자**: innerview  
**데이터베이스**: MySQL (innerview@211.169.231.242)  
**네이버 계정**: innerviewit  
**프로젝트 타입**: 웹 크롤링 / 데이터 수집

---

## ⚖️ 법적 고려사항

**주의**: 이 프로젝트는 네이버 스마트스토어의 리뷰 데이터를 수집합니다.
- 네이버 이용약관 준수 필요
- 개인정보 보호법 준수
- 로봇 배제 표준(robots.txt) 확인
- 상업적 이용 시 법적 검토 필요

---

## 📞 문의 및 지원

프로젝트 관련 문의사항이나 개선 제안은 프로젝트 관리자에게 연락 바랍니다.

---

**문서 버전**: 1.0  
**최종 수정일**: 2025-10-01  
**작성 도구**: Windsurf AI
