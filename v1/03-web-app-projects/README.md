# 03-web-app-projects

웹 애플리케이션 프로젝트들의 기능요구서 및 문서화 모음입니다.

## 📋 프로젝트 목록

### 1. CMS (Content Management System)
- **설명**: 콘텐츠 관리 시스템
- **주요 기능**: 콘텐츠 작성/편집, 미디어 관리, 사용자 권한 관리, SEO 최적화
- **기술 스택**: Node.js, React, MongoDB, AWS S3
- **문서**: [CMS_기능요구서.csv](./CMS_기능요구서.csv)

### 2. MoyeApp
- **설명**: 소셜 네트워킹 모바일 웹 앱
- **주요 기능**: 사용자 매칭, 소셜 기능, 채팅, 모임 관리, 위치 서비스
- **기술 스택**: React Native, Node.js, Socket.io, MongoDB
- **문서**: [MoyeApp_기능요구서.csv](./MoyeApp_기능요구서.csv)

### 3. FMS Backend (Fleet Management System)
- **설명**: 차량 관리 시스템 백엔드
- **주요 기능**: 차량 관리, GPS 추적, 운행 관리, 연료 관리, 정비 관리
- **기술 스택**: Node.js, Express, PostgreSQL, Redis
- **문서**: [FMS_Backend_기능요구서.csv](./FMS_Backend_기능요구서.csv)

### 4. InnerBoard
- **설명**: 사내 게시판 시스템
- **주요 기능**: 게시판 관리, 게시글/댓글 관리, 파일 관리, 알림 시스템
- **기술 스택**: React, Node.js, MySQL, Redis
- **문서**: [InnerBoard_기능요구서.csv](./InnerBoard_기능요구서.csv)

### 5. Naver Review Crawling
- **설명**: 네이버 리뷰 크롤링 시스템
- **주요 기능**: 웹 크롤링, 데이터 추출, 감정 분석, 데이터 저장 및 분석
- **기술 스택**: Python, Selenium, BeautifulSoup, MongoDB
- **문서**: [NaverReviewCrawling_기능요구서.csv](./NaverReviewCrawling_기능요구서.csv)

### 6. GrapesJS
- **설명**: 웹 페이지 빌더 시스템
- **주요 기능**: 드래그앤드롭 에디터, 컴포넌트 관리, 템플릿 시스템, 코드 편집
- **기술 스택**: GrapesJS, JavaScript, CSS, HTML
- **문서**: [GrapesJS_기능요구서.csv](./GrapesJS_기능요구서.csv)

### 7. Annual API
- **설명**: 연차 관리 시스템 API
- **주요 기능**: 연차 신청/승인, 사용자 관리, 알림 시스템, 보고서 생성
- **기술 스택**: Node.js, Express, PostgreSQL, JWT
- **문서**: [Annual_API_기능요구서.csv](./Annual_API_기능요구서.csv)

### 8. Annual (연차 관리 시스템)
- **설명**: 연차 관리 웹 애플리케이션
- **주요 기능**: 연차 신청/승인, 캘린더 관리, 사용자 대시보드, 관리자 기능
- **기술 스택**: React, Node.js, PostgreSQL, Calendar API
- **문서**: [Annual_기능요구서.csv](./Annual_기능요구서.csv)

### 9. FP App (Financial Planning)
- **설명**: 개인 재정 관리 모바일 앱
- **주요 기능**: 가계부, 예산 관리, 통계 분석, 목표 관리, 보안 기능
- **기술 스택**: React Native, Firebase, Chart.js, Biometric
- **문서**: [FP_App_기능요구서.csv](./FP_App_기능요구서.csv)

### 10. SNAC (Social Network App for Community)
- **설명**: 커뮤니티 기반 소셜 네트워크 앱
- **주요 기능**: 소셜 기능, 게시물 관리, 메시징, 그룹 기능, 실시간 알림
- **기술 스택**: Node.js, WebSocket, MongoDB, Redis, Elasticsearch
- **문서**: [SNAC_기능요구서.csv](./SNAC_기능요구서.csv)

### 11. Gift (선물 플랫폼)
- **설명**: 온라인 선물 주문 및 배송 플랫폼
- **주요 기능**: 상품 관리, 선물 기능, 결제 시스템, 배송 관리, 리뷰 시스템
- **기술 스택**: React, Node.js, Payment Gateway, Delivery API
- **문서**: [Gift_기능요구서.csv](./Gift_기능요구서.csv)

### 12. Andar Detail Editor
- **설명**: 상품 상세 페이지 편집기
- **주요 기능**: 리치 텍스트 편집, 이미지/미디어 관리, 레이아웃 편집, 실시간 미리보기
- **기술 스택**: Rich Text Editor, Canvas API, Media Processing
- **문서**: [AndarDetailEditor_기능요구서.csv](./AndarDetailEditor_기능요구서.csv)

### 13. Retrieve Web App
- **설명**: 데이터 검색 및 조회 웹 애플리케이션
- **주요 기능**: 통합 검색, 데이터 조회, 결과 표시, 내보내기, 성능 최적화
- **기술 스택**: Elasticsearch, React, Node.js, Redis
- **문서**: [RetrieveWebApp_기능요구서.csv](./RetrieveWebApp_기능요구서.csv)

## 📊 프로젝트 현황 요약

### 개발 상태별 분류
- **완료**: CMS, InnerBoard, Annual API, Annual, Gift, Andar Detail Editor, Retrieve Web App
- **진행중**: MoyeApp, FMS Backend, FP App, SNAC
- **계획**: Naver Review Crawling, GrapesJS

### 기술 스택별 분류
- **Backend**: Node.js/Express (10개), Python (1개)
- **Frontend**: React/React Native (9개), GrapesJS (1개), Rich Text Editor (1개)
- **Database**: MongoDB (5개), PostgreSQL (4개), MySQL (1개), Elasticsearch (3개)
- **Real-time**: WebSocket/Socket.io (4개)

### 도메인별 분류
- **소셜/커뮤니티**: MoyeApp, SNAC
- **비즈니스 관리**: CMS, InnerBoard, Annual, Annual API, FMS Backend
- **이커머스**: Gift, Andar Detail Editor
- **개인 도구**: FP App
- **데이터 처리**: Naver Review Crawling, Retrieve Web App
- **개발 도구**: GrapesJS

## 🛠 공통 기능 요소

### 인증 및 보안
- JWT 토큰 기반 인증
- OAuth 소셜 로그인
- 2단계 인증 (2FA)
- 데이터 암호화
- API 보안

### 데이터 관리
- 데이터베이스 설계 및 최적화
- 백업 및 복구 시스템
- 데이터 마이그레이션
- 캐싱 시스템 (Redis)

### 성능 최적화
- 로드 밸런싱
- CDN 활용
- 이미지/동영상 최적화
- 쿼리 최적화

### 모니터링 및 분석
- 시스템 로그 관리
- 에러 추적 (Sentry)
- 성능 모니터링
- 사용자 행동 분석

### 알림 시스템
- 푸시 알림 (FCM/APNS)
- 이메일 알림
- 실시간 알림
- 알림 설정 관리

## 📈 개발 우선순위

### 높은 우선순위
1. 사용자 인증 및 관리
2. 핵심 비즈니스 로직
3. 데이터 보안
4. API 설계 및 구현

### 중간 우선순위
1. 사용자 경험 개선
2. 성능 최적화
3. 알림 시스템
4. 관리자 기능

### 낮은 우선순위
1. 고급 분석 기능
2. AI/ML 기반 추천
3. 확장 기능
4. 써드파티 연동

## 🔧 개발 환경 및 도구

### 개발 도구
- **IDE**: VS Code, WebStorm
- **버전 관리**: Git, GitHub
- **API 문서화**: Swagger/OpenAPI
- **테스트**: Jest, Mocha, Supertest

### 배포 및 인프라
- **컨테이너화**: Docker
- **오케스트레이션**: Kubernetes
- **CI/CD**: GitHub Actions
- **클라우드**: AWS, Firebase

### 모니터링
- **로그**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **에러 추적**: Sentry
- **성능**: New Relic
- **분석**: Google Analytics

## 📝 문서 구조

각 프로젝트의 기능요구서는 다음과 같은 구조로 작성되었습니다:

```csv
카테고리,기능명,우선순위,상태,담당자,예상공수(일),설명,기술스택,의존성
```

### 필드 설명
- **카테고리**: 기능의 분류 (예: 사용자관리, API, 보안 등)
- **기능명**: 구체적인 기능 이름
- **우선순위**: 높음/중간/낮음
- **상태**: 완료/진행중/계획
- **담당자**: 개발 담당 팀
- **예상공수**: 개발 예상 소요 일수
- **설명**: 기능에 대한 간단한 설명
- **기술스택**: 사용할 기술 및 도구
- **의존성**: 선행되어야 할 기능

## 🚀 향후 계획

### 단기 목표 (3개월)
- 진행중인 프로젝트들의 핵심 기능 완료
- API 문서화 및 테스트 코드 작성
- 보안 강화 및 성능 최적화

### 중기 목표 (6개월)
- 모든 프로젝트의 MVP 버전 완료
- 사용자 피드백 수집 및 개선
- 모니터링 시스템 구축

### 장기 목표 (1년)
- AI/ML 기반 기능 추가
- 마이크로서비스 아키텍처 전환
- 글로벌 서비스 확장

## 📞 연락처

프로젝트 관련 문의사항이 있으시면 다음으로 연락해 주세요:

- **프로젝트 매니저**: [PM 연락처]
- **기술 리드**: [Tech Lead 연락처]
- **개발팀**: [Dev Team 연락처]

---

*마지막 업데이트: 2024년 12월* 