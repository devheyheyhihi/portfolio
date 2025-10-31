# Windsurf Lovebug Map - 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Lovebug Map (러브버그 맵)  
**설명**: SNS 데이터 기반 실시간 러브버그 출몰 지도 서비스  
**버전**: 1.0.0  
**작성일**: 2025-10-02

## 🎯 프로젝트 목적

러브버그(붉은등우단털파리)의 실시간 출몰 정보를 SNS 데이터를 크롤링하여 수집하고, 이를 지도 위에 시각화하여 사용자에게 제공하는 웹 애플리케이션입니다.

## 🏗️ 프로젝트 구조

```
lovebug_map/
├── backend/                    # 백엔드 (FastAPI)
│   ├── app/
│   │   ├── api/               # API 라우트
│   │   │   └── routes.py      # REST API 엔드포인트
│   │   ├── crawlers/          # 크롤러 모듈
│   │   │   └── twitter_crawler.py
│   │   ├── models/            # 데이터 모델
│   │   │   └── lovebug_data.py
│   │   ├── utils/             # 유틸리티
│   │   │   ├── location_extractor.py
│   │   │   ├── text_analyzer.py
│   │   │   └── websocket_manager.py
│   │   └── main.py            # 메인 애플리케이션
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── render.yaml            # Render 배포 설정
│   └── start.sh
│
└── frontend/                   # 프론트엔드 (React + TypeScript)
    ├── public/
    ├── src/
    │   ├── api/               # API 클라이언트
    │   │   └── lovebugApi.ts
    │   ├── components/        # 재사용 컴포넌트
    │   │   ├── ErrorBoundary.tsx
    │   │   ├── Header.tsx
    │   │   ├── Layout.tsx
    │   │   ├── LoadingSpinner.tsx
    │   │   ├── MapView.tsx
    │   │   ├── NotificationToast.tsx
    │   │   └── Sidebar.tsx
    │   ├── contexts/          # React Context
    │   │   └── WebSocketContext.tsx
    │   ├── pages/             # 페이지 컴포넌트
    │   │   ├── DashboardPage.tsx
    │   │   ├── HomePage.tsx
    │   │   ├── HotspotsPage.tsx
    │   │   ├── MapPage.tsx
    │   │   ├── ReportPage.tsx
    │   │   ├── ReportsPage.tsx
    │   │   └── StatisticsPage.tsx
    │   ├── styles/            # 스타일링
    │   │   ├── GlobalStyles.ts
    │   │   └── theme.ts
    │   ├── types/             # TypeScript 타입 정의
    │   │   └── index.ts
    │   ├── utils/             # 유틸리티 함수
    │   │   ├── api.ts
    │   │   ├── constants.ts
    │   │   ├── formatters.ts
    │   │   └── websocket.ts
    │   ├── App.tsx
    │   └── index.tsx
    ├── package.json
    └── tsconfig.json
```

## 🛠️ 기술 스택

### Backend
- **Framework**: FastAPI 0.104.1
- **Server**: Uvicorn 0.24.0
- **Database**: MongoDB (Motor 3.3.2, PyMongo 4.6.0)
- **Web Scraping**: 
  - Selenium 4.15.2
  - BeautifulSoup4 4.12.2
  - Tweepy 4.14.0
- **NLP/ML**:
  - KoNLPy 0.6.0 (한국어 자연어 처리)
  - scikit-learn 1.3.2
  - pandas 2.1.3
  - numpy 1.24.3
- **AI**: OpenAI 1.3.8
- **Task Scheduling**: APScheduler 3.10.4
- **Real-time Communication**: WebSockets 12.0
- **Authentication**: python-jose, passlib
- **Caching**: Redis 5.0.1

### Frontend
- **Framework**: React 18.2.0 + TypeScript 4.9.5
- **Routing**: React Router DOM 6.8.0
- **State Management**: @tanstack/react-query 4.24.0
- **Styling**: styled-components 5.3.6
- **Map**: Leaflet 1.9.4
- **Charts**: Recharts 3.0.2
- **HTTP Client**: Axios 1.3.0

### Infrastructure
- **Deployment**: Render (Backend), Vercel (Frontend 추정)
- **Containerization**: Docker
- **Environment**: Python 3.x

## 📊 주요 기능

### 1. 데이터 수집 (Crawling)
- **Twitter Crawler**: 트위터에서 러브버그 관련 트윗 수집
  - 키워드: 러브버그, 붉은등우단털파리, 빨간벌레 등
  - 10분마다 자동 크롤링 (APScheduler)
  - Twitter API 또는 웹 스크래핑 방식 지원

### 2. 데이터 분석
- **텍스트 분석** (`text_analyzer.py`):
  - 감정 분석 (sentiment: -1.0 ~ 1.0)
  - 강도 분석 (intensity: low/medium/high)
  - 신뢰도 계산 (confidence: 0.0 ~ 1.0)
  - 관련성 점수 (relevance: 0.0 ~ 1.0)
  - 키워드 추출

- **위치 추출** (`location_extractor.py`):
  - 텍스트에서 위치명 추출 (역, 구, 동 등)
  - 서울 주요 지역 좌표 매핑 (64개 위치)
  - 주소 → 좌표 변환
  - 구/군, 시/도 추출

### 3. 심각도 판단
4단계 심각도 레벨:
- **LOW**: 적음
- **MEDIUM**: 보통
- **HIGH**: 많음
- **CRITICAL**: 매우 많음

키워드 기반 자동 분류:
- CRITICAL: 지옥, 떼거리, 엄청, 미친, 완전
- HIGH: 많아, 진짜, 심해, 대박
- MEDIUM: 좀, 꽤, 조금
- LOW: 기타

### 4. REST API 엔드포인트

#### 보고서 관련
- `GET /api/v1/reports` - 러브버그 보고서 목록 조회
  - 필터: severity, platform, hours
  - 페이지네이션: limit, offset
- `GET /api/v1/reports/{report_id}` - 특정 보고서 조회
- `GET /api/v1/search` - 보고서 검색
  - 키워드, 위치, 반경, 심각도, 플랫폼, 시간 필터

#### 통계 관련
- `GET /api/v1/stats` - 통계 정보 조회
  - 전체 보고서 수
  - 시간대별 보고서 수
  - 지역별 보고서 수
  - 심각도별 분포
  - 인기 키워드 TOP 10
  - 평균 감정 점수

#### 핫스팟 관련
- `GET /api/v1/hotspots` - 러브버그 핫스팟 조회
  - 위치별 보고서 집계
  - 평균 심각도 계산
  - 최소 2개 이상 보고서 필요

#### 지역 관련
- `GET /api/v1/districts` - 지역별 현황 조회

#### 시스템
- `GET /` - 헬스체크
- `GET /health` - 상세 헬스체크 (MongoDB, Scheduler 상태)
- `WS /ws` - WebSocket 연결 (실시간 업데이트)

### 5. 실시간 통신
- WebSocket을 통한 실시간 데이터 업데이트
- 새로운 러브버그 보고서 발생 시 클라이언트에 즉시 전송
- 연결 유지 및 재연결 관리

### 6. 프론트엔드 페이지
- **HomePage**: 메인 페이지
- **MapPage**: 지도 뷰 (Leaflet)
- **StatisticsPage**: 통계 대시보드
- **DashboardPage**: 종합 대시보드
- **ReportPage**: 보고서 작성
- **ReportsPage**: 보고서 목록
- **HotspotsPage**: 핫스팟 분석

## 💾 데이터 모델

### LovebugReport
```python
{
    "id": str,                    # MongoDB ObjectId
    "tweet_id": str,              # 트위터 고유 ID
    "platform": Platform,         # twitter/instagram/naver_blog/kakao_talk
    "content": str,               # 원본 텍스트
    "location": {
        "latitude": float,
        "longitude": float,
        "address": str,
        "district": str,          # 구/군
        "city": str               # 시/도
    },
    "severity": SeverityLevel,    # low/medium/high/critical
    "confidence": float,          # 0.0-1.0
    "sentiment": float,           # -1.0 ~ 1.0
    "keywords": List[str],
    "image_urls": List[str],
    "author": str,
    "created_at": datetime,
    "updated_at": datetime
}
```

### LovebugStats
```python
{
    "total_reports": int,
    "reports_by_hour": Dict[int, int],
    "reports_by_district": Dict[str, int],
    "severity_distribution": Dict[SeverityLevel, int],
    "top_keywords": List[Dict],
    "average_sentiment": float,
    "last_updated": datetime
}
```

### HotSpot
```python
{
    "location": Location,
    "report_count": int,
    "average_severity": float,
    "radius": float,              # km
    "last_activity": datetime
}
```

## 🔄 데이터 흐름

1. **수집 단계**:
   - APScheduler가 10분마다 TwitterCrawler 실행
   - 러브버그 관련 키워드로 트윗 검색
   - 트윗 데이터 수집 (텍스트, 작성자, 시간, 이미지)

2. **분석 단계**:
   - TextAnalyzer: 감정, 강도, 신뢰도, 관련성 분석
   - LocationExtractor: 위치 정보 추출 및 좌표 변환
   - 심각도 자동 판단

3. **저장 단계**:
   - MongoDB에 LovebugReport 저장
   - tweet_id 기준 upsert (중복 방지)

4. **전송 단계**:
   - WebSocket을 통해 연결된 클라이언트에 실시간 전송
   - REST API를 통한 조회 가능

5. **시각화 단계**:
   - React 프론트엔드에서 데이터 수신
   - Leaflet 지도에 마커 표시
   - Recharts로 통계 차트 생성

## 🌐 배포 환경

### Backend
- **Platform**: Render
- **Environment Variables**:
  - `PORT`: 8000
  - `MONGODB_URL`: MongoDB 연결 문자열
  - `ALLOWED_ORIGINS`: CORS 허용 도메인
  - `ENVIRONMENT`: production
  - `LOG_LEVEL`: INFO
  - `TWITTER_BEARER_TOKEN`: Twitter API 토큰 (선택)

### Frontend
- **Platform**: Vercel (추정)
- **URL**: https://lovebug-map.vercel.app (추정)

### Backend URL
- https://lovebug-map-backend.onrender.com

## 🔐 보안 및 인증

- CORS 설정으로 허용된 도메인만 접근 가능
- python-jose를 통한 JWT 토큰 지원
- passlib를 통한 비밀번호 해싱
- 환경 변수를 통한 민감 정보 관리

## 📈 성능 최적화

### Backend
- Motor (비동기 MongoDB 드라이버) 사용
- Redis 캐싱 지원
- APScheduler를 통한 효율적인 작업 스케줄링
- MongoDB aggregation pipeline 활용

### Frontend
- React Query를 통한 데이터 캐싱
  - staleTime: 5분
  - cacheTime: 10분
- 코드 스플리팅 (React Router)
- styled-components를 통한 CSS-in-JS

## 🧪 테스트

- Frontend: Jest 테스트 환경 설정 (`react-scripts test`)
- Backend: 테스트 데이터 추가 스크립트 (`add_test_data.py`)

## 📝 주요 특징

1. **실시간성**: WebSocket을 통한 실시간 데이터 업데이트
2. **자동화**: 10분마다 자동 크롤링 및 분석
3. **지능형 분석**: NLP 기반 텍스트 분석 및 위치 추출
4. **확장성**: 
   - 다중 플랫폼 지원 (Twitter, Instagram, Naver, Kakao)
   - MongoDB를 통한 대용량 데이터 처리
   - Redis 캐싱 지원
5. **사용자 친화적**: 
   - 직관적인 지도 인터페이스
   - 다양한 필터링 옵션
   - 통계 및 핫스팟 분석

## 🚀 실행 방법

### Backend
```bash
cd backend
pip install -r requirements.txt
python -m app.main
```

### Frontend
```bash
cd frontend
npm install
npm start
```

## 🔮 향후 개선 사항

1. **데이터 소스 확장**:
   - Instagram, Naver Blog, Kakao Talk 크롤러 구현
   - 공공 데이터 API 연동

2. **분석 고도화**:
   - OpenAI API를 활용한 고급 텍스트 분석
   - 이미지 인식을 통한 러브버그 자동 감지
   - 예측 모델 구축 (scikit-learn)

3. **기능 추가**:
   - 사용자 제보 기능
   - 알림 설정 (특정 지역 심각도 상승 시)
   - 히스토리 데이터 분석
   - 날씨 데이터 연동

4. **성능 개선**:
   - CDN 적용
   - 데이터베이스 인덱싱 최적화
   - 캐싱 전략 고도화

## 📞 연락처 및 배포 URL

- **Backend API**: https://lovebug-map-backend.onrender.com
- **Frontend**: https://lovebug-map.vercel.app

---

**문서 작성**: Windsurf AI  
**최종 업데이트**: 2025-10-02
