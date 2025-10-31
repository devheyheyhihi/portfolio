# Windsurf - QT Staking 프로젝트 분석 문서

> **작성일**: 2025-10-13  
> **프로젝트**: Quantum Chain (QCC) 스테이킹 플랫폼  
> **버전**: 1.0.0

---

## 📋 목차

1. [프로젝트 개요](#프로젝트-개요)
2. [기술 스택](#기술-스택)
3. [프로젝트 구조](#프로젝트-구조)
4. [주요 기능](#주요-기능)
5. [아키텍처 분석](#아키텍처-분석)
6. [데이터베이스 설계](#데이터베이스-설계)
7. [API 엔드포인트](#api-엔드포인트)
8. [보안 구현](#보안-구현)
9. [배포 전략](#배포-전략)
10. [개선 제안사항](#개선-제안사항)

---

## 🎯 프로젝트 개요

### 프로젝트 설명
**QT Staking**은 Quantum Chain 코인(QCC)을 위한 풀스택 스테이킹 플랫폼입니다. 사용자는 자신의 QCC 토큰을 다양한 기간 동안 스테이킹하여 이자 수익을 얻을 수 있으며, 관리자는 스테이킹 현황을 모니터링하고 대량 전송 기능을 통해 효율적으로 자산을 관리할 수 있습니다.

### 핵심 가치
- ✅ **다양한 스테이킹 옵션**: 30일, 90일, 180일, 365일 기간 선택
- ✅ **차등 이자율**: 기간에 따른 5% ~ 18% APY 제공
- ✅ **실시간 모니터링**: 스테이킹 현황 및 수익 실시간 추적
- ✅ **관리자 대시보드**: 전체 스테이킹 관리 및 대량 전송 기능
- ✅ **자체 블록체인 연동**: Quantum Chain 네트워크 직접 연동

### 프로젝트 목표
1. 사용자 친화적인 스테이킹 인터페이스 제공
2. 안전한 자산 관리 및 트랜잭션 처리
3. 자동화된 만기 처리 시스템 구축
4. 관리자를 위한 효율적인 운영 도구 제공

---

## 🛠 기술 스택

### Frontend
| 기술 | 버전 | 용도 |
|------|------|------|
| **Next.js** | 14.0.0 | React 프레임워크 (SSR/SSG) |
| **React** | 18.2.0 | UI 라이브러리 |
| **TypeScript** | 5.0.0 | 타입 안정성 |
| **Tailwind CSS** | 3.3.0 | 스타일링 |
| **React Hook Form** | 7.43.0 | 폼 관리 |
| **React Query** | 5.81.2 | 서버 상태 관리 |
| **Jotai** | 2.12.5 | 클라이언트 상태 관리 |
| **Recharts** | 2.5.0 | 데이터 시각화 |
| **qrcode.react** | 3.1.0 | QR 코드 생성 |
| **PapaParse** | 5.5.3 | CSV 파싱 |

### Backend
| 기술 | 버전 | 용도 |
|------|------|------|
| **Node.js** | - | 런타임 환경 |
| **Express.js** | 4.21.2 | 웹 프레임워크 |
| **SQLite3** | 5.1.7 | 데이터베이스 |
| **bcrypt** | 6.0.0 | 비밀번호 해싱 |
| **jsonwebtoken** | 9.0.2 | JWT 인증 |
| **Helmet** | 8.1.0 | 보안 헤더 |
| **Decimal.js** | 10.5.0 | 정밀 계산 |
| **TweetNaCl** | 1.0.3 | 암호화 |

### 블록체인
- **Quantum Chain**: 자체 블록체인 네트워크
- **Ed25519**: 서명 알고리즘
- **BIP39**: 니모닉 구문 생성

---

## 📁 프로젝트 구조

```
qt_staking/
├── frontend/                          # Next.js 프론트엔드
│   ├── src/
│   │   ├── app/                       # Next.js 13+ App Router
│   │   │   ├── admin/                 # 관리자 페이지
│   │   │   ├── globals.css
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx
│   │   ├── api/                       # API 클라이언트
│   │   ├── components/                # React 컴포넌트
│   │   ├── lib/                       # 유틸리티
│   │   └── types/                     # TypeScript 타입
│   └── package.json
│
├── backend/                           # Express.js 백엔드
│   ├── src/
│   │   ├── config/                    # 설정
│   │   ├── controllers/               # 컨트롤러
│   │   ├── models/                    # 데이터 모델
│   │   ├── routes/                    # API 라우트
│   │   ├── services/                  # 비즈니스 로직
│   │   └── app.js
│   ├── scripts/                       # 자동화 스크립트
│   └── database.sqlite
│
├── DEPLOYMENT.md                      # 배포 가이드
├── README.md
└── staking_functional_requirements.csv
```

---

## 🎨 주요 기능

### 1. 사용자 기능

#### 1.1 지갑 연결
- 키 파일 업로드 (`.qcc`)
- 복구 구문 (12단어 니모닉)
- 실시간 잔액 조회

#### 1.2 스테이킹 신청
- 기간 선택: 30일, 90일, 180일, 365일
- 금액 입력 및 예상 수익 계산
- QR 코드 지원
- 블록체인 트랜잭션 전송

#### 1.3 대시보드
- 스테이킹 현황 (총액, 수익, 진행률)
- 상태별 필터링 (대기중, 진행중, 완료)
- 차트 시각화

### 2. 관리자 기능

#### 2.1 스테이킹 관리
- 전체 목록 조회
- 상태 업데이트
- 이자율 설정

#### 2.2 대량 전송 ⭐
- CSV 파일 업로드
- 실시간 진행률 표시
- 결과 다운로드

### 3. 자동화

#### 3.1 만기 처리
- Cron Job으로 자동 체크
- 상태 자동 업데이트

---

## 🏗 아키텍처 분석

### 시스템 구조

```
Frontend (Next.js)
    ↓ HTTP/REST
Backend (Express.js)
    ↓ SQL
Database (SQLite)
    ↓ API
Quantum Chain Network
```

### 데이터 흐름

**스테이킹 신청**:
1. 사용자 → Frontend: 폼 작성
2. Frontend → Blockchain: QCC 전송
3. Blockchain → Frontend: 트랜잭션 해시
4. Frontend → Backend: 스테이킹 정보 저장
5. Backend → Database: 기록 저장
6. Backend → Frontend: 성공 응답

---

## 💾 데이터베이스 설계

### 주요 테이블

#### 1. Users
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  wallet_address TEXT UNIQUE,
  created_at DATETIME
);
```

#### 2. Stakings
```sql
CREATE TABLE stakings (
  id INTEGER PRIMARY KEY,
  wallet_address TEXT,
  staked_amount REAL,
  staking_period INTEGER,
  interest_rate REAL,
  expected_reward REAL,
  start_date DATETIME,
  end_date DATETIME,
  status TEXT,
  transaction_hash TEXT
);
```

**Status**: `pending`, `active`, `completed`, `cancelled`

#### 3. InterestRates
```sql
CREATE TABLE interest_rates (
  period_days INTEGER PRIMARY KEY,
  apy_rate REAL
);
```

기본값:
- 30일: 5%
- 90일: 8%
- 180일: 12%
- 365일: 18%

---

## 🔌 API 엔드포인트

### 인증
- `POST /api/auth/login` - 관리자 로그인

### 스테이킹
- `GET /api/staking` - 목록 조회
- `GET /api/staking/wallet/:address` - 지갑별 조회
- `POST /api/staking` - 생성
- `PUT /api/staking/:id` - 업데이트
- `DELETE /api/staking/:id` - 삭제

### 이자율
- `GET /api/staking/rates` - 조회
- `PUT /api/staking/rates` - 업데이트 (관리자)

### 트랜잭션
- `POST /api/transaction/send` - QCC 전송

---

## 🔒 보안 구현

### 1. 인증 & 인가
- JWT 토큰 기반 인증
- bcrypt 비밀번호 해싱
- 세션 관리

### 2. 데이터 보호
- HTTPS 통신 권장
- 개인키 메모리에서만 처리
- 환경 변수로 민감 정보 관리

### 3. API 보안
- Helmet.js (보안 헤더)
- CORS 설정
- Rate Limiting (권장)

### 4. 입력 검증
- 폼 유효성 검사
- SQL Injection 방지
- XSS 방지

---

## 🚀 배포 전략

### 배포 환경
- **Frontend**: Vercel
- **Backend**: Render
- **Database**: SQLite (Render 내장)

### 배포 프로세스

#### 1. 백엔드 배포 (Render)
```bash
# 환경 변수 설정
NODE_ENV=production
PORT=10000
JWT_SECRET=<랜덤 문자열>
ADMIN_PASSWORD=<관리자 비밀번호>
CORS_ORIGIN=<프론트엔드 URL>
```

#### 2. 프론트엔드 배포 (Vercel)
```bash
# 환경 변수 설정
NEXT_PUBLIC_API_URL=<백엔드 API URL>
```

### 주의사항
- Render 무료 플랜: 15분 비활성 시 Sleep
- UptimeRobot으로 주기적 핑 권장
- Cold Start 시 10-30초 대기

---

## 💡 개선 제안사항

### 1. 기능 개선
- [ ] 이메일/푸시 알림 시스템
- [ ] 조기 출금 기능 (페널티 적용)
- [ ] 스테이킹 히스토리 차트
- [ ] 다국어 지원 (i18n)

### 2. 성능 최적화
- [ ] Redis 캐싱 도입
- [ ] PostgreSQL 마이그레이션
- [ ] API 응답 압축
- [ ] 이미지 최적화

### 3. 보안 강화
- [ ] 2FA 인증
- [ ] Rate Limiting
- [ ] API Key 관리
- [ ] 감사 로그

### 4. 테스트
- [ ] Unit 테스트 (Jest)
- [ ] Integration 테스트
- [ ] E2E 테스트 (Playwright)
- [ ] 부하 테스트

### 5. DevOps
- [ ] CI/CD 파이프라인
- [ ] 모니터링 (Sentry)
- [ ] 로깅 시스템
- [ ] 백업 자동화

---

## 📊 프로젝트 통계

### 코드 규모
- **Frontend**: ~50 파일
- **Backend**: ~20 파일
- **총 라인 수**: ~10,000 LOC

### 기능 요구사항 (CSV 기준)
- **프론트엔드**: 10개 기능
- **백엔드**: 10개 기능
- **데이터베이스**: 4개 테이블
- **블록체인 연동**: 4개 기능
- **보안**: 3개 요구사항

---

## 🎓 학습 포인트

### 1. 풀스택 개발
- Next.js 14 App Router
- Express.js REST API
- SQLite 데이터베이스

### 2. 블록체인 통합
- 지갑 연결 구현
- 트랜잭션 처리
- 서명 검증

### 3. 상태 관리
- Jotai (전역 상태)
- React Query (서버 상태)
- React Hook Form (폼 상태)

### 4. 보안
- JWT 인증
- 비밀번호 해싱
- CORS 설정

---

## 📞 참고 자료

### 문서
- [README.md](./README.md) - 프로젝트 소개
- [DEPLOYMENT.md](./DEPLOYMENT.md) - 배포 가이드
- [backend/CRON_SETUP.md](./backend/CRON_SETUP.md) - Cron 설정

### 주요 파일
- `frontend/src/app/page.tsx` - 메인 페이지
- `backend/src/app.js` - Express 서버
- `backend/scripts/checkExpiredStakings.js` - 만기 처리

---

## ✅ 체크리스트

### 개발 완료
- [x] 지갑 연결 기능
- [x] 스테이킹 신청
- [x] 대시보드
- [x] 관리자 페이지
- [x] 대량 전송
- [x] 만기 자동 처리
- [x] 이자율 관리

### 배포 준비
- [x] 프론트엔드 빌드
- [x] 백엔드 API
- [x] 데이터베이스 스키마
- [x] 환경 변수 설정
- [x] 배포 문서

---

**프로젝트 분석 완료일**: 2025-10-13  
**분석자**: Windsurf AI  
**문서 버전**: 1.0.0
