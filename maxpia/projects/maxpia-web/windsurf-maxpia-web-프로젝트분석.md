# Windsurf - Maxpia Web 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Maxpia Web (맥스피아아이시티 웹사이트)  
**버전**: 0.1.0  
**설명**: 블록체인 기반 금융 솔루션 및 노드 운영 서비스를 제공하는 기업 웹사이트

---

## 🛠 기술 스택

### Frontend
- **Framework**: Next.js 15.3.5 (App Router)
- **React**: 19.0.0
- **TypeScript**: 5.x
- **스타일링**: Tailwind CSS 3.4.0
- **폰트**: Pretendard Variable

### Backend & Database
- **ORM**: Prisma 6.11.1
- **Database**: MySQL
- **API**: Next.js API Routes

### 주요 라이브러리
- **@tiptap/react**: 2.25.0 - 리치 텍스트 에디터
- **@tiptap/starter-kit**: 2.25.0
- **@tiptap/extension-image**: 2.25.0
- **date-fns**: 4.1.0 - 날짜 포맷팅

### 개발 도구
- **ESLint**: 9.x
- **PostCSS**: 8.4.32
- **Autoprefixer**: 10.4.16
- **Turbopack**: Next.js dev 모드에서 사용

---

## 📁 프로젝트 구조

```
maxpia-web/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── about/             # 회사 소개 페이지
│   │   ├── admin/             # 관리자 페이지
│   │   ├── api/               # API Routes
│   │   │   ├── email/
│   │   │   ├── inquiries/     # 문의 API
│   │   │   ├── notices/       # 공지사항 API
│   │   │   └── upload/        # 파일 업로드 API
│   │   ├── contact/           # 문의하기 페이지
│   │   ├── notice/            # 공지사항 페이지
│   │   │   └── [id]/         # 공지사항 상세
│   │   ├── qcc/              # QCC 관련 페이지
│   │   ├── solutions/         # 솔루션 소개 페이지
│   │   ├── actions.ts         # Server Actions
│   │   ├── globals.css        # 전역 스타일
│   │   ├── layout.tsx         # 루트 레이아웃
│   │   └── page.tsx           # 홈페이지
│   ├── components/            # React 컴포넌트
│   │   ├── ContactModal.tsx
│   │   ├── ContactSection.tsx
│   │   ├── CreateNoticeModal.tsx
│   │   ├── EditNoticeModal.tsx
│   │   ├── FaqSection.tsx
│   │   ├── FeaturesSection.tsx
│   │   ├── Footer.tsx
│   │   ├── Header.tsx
│   │   ├── HeroSection.tsx
│   │   ├── NoticeBoard.tsx
│   │   ├── PasswordCheckModal.tsx
│   │   ├── ReadOnlyNoticeBoard.tsx
│   │   ├── SkillStackSection.tsx
│   │   ├── Tiptap.css
│   │   └── TiptapEditor.tsx
│   ├── generated/             # Prisma 생성 파일
│   └── lib/                   # 유틸리티 함수
├── prisma/
│   ├── migrations/            # DB 마이그레이션
│   └── schema.prisma          # Prisma 스키마
├── public/                    # 정적 파일 (이미지, 아이콘 등)
├── next.config.js
├── tailwind.config.js
├── tsconfig.json
└── package.json
```

---

## 🗄 데이터베이스 스키마

### Notice (공지사항)
```prisma
model Notice {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?  @db.Text
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### Inquiry (문의)
```prisma
model Inquiry {
  id        Int      @id @default(autoincrement())
  name      String
  company   String
  title     String
  email     String
  message   String   @db.Text
  status    String   @default("pending") // "pending", "completed"
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

---

## 🎨 주요 페이지 및 기능

### 1. 홈페이지 (`/`)
- **컴포넌트 구성**:
  - Header: 네비게이션 메뉴
  - HeroSection: 메인 배너
  - SkillStackSection: 기술 스택 소개
  - FeaturesSection: 주요 기능/서비스
  - ContactSection: 문의 섹션
  - Footer: 푸터

### 2. 회사 소개 (`/about`)
- **탭 구성**:
  - 회사연혁: 2016년부터 현재까지의 주요 이력
  - 조직구성: 조직도 이미지
  - 설립목적: 회사 설립 목적 및 비전
- **핵심가치**: 기술 혁신, 투명성, 전문성, 고객중심, 글로벌 지향

### 3. 솔루션 (`/solutions`)
- **가상서버 (Cloud VPS) 서비스**:
  - 웹 호스팅 (Linux, Windows, NFT)
  - 클라우드 호스팅 (탄력적 확장, 하이브리드, 커뮤니티)
  - 서버 호스팅 (VPS, 공유, 매니지드, 셀프, 스트리밍)
  - 데이터베이스 호스팅 (On-Premises, Managed, DBaaS)
- **블록체인 노드 운영 솔루션**
- **시스템 통합 운영 솔루션**
- **대표 참여 코인**: Bitcoin, Ethereum, Filecoin, Aleo

### 4. 관리자 페이지 (`/admin`)
- **인증**: 비밀번호 기반 (aortmvldkict)
- **문의 관리**:
  - 문의 목록 조회
  - 상태 관리 (대기 중/완료)
  - 문의 상세 정보 확인
- **공지사항 관리**:
  - 공지사항 작성 (TipTap 에디터)
  - 공지사항 수정
  - 공지사항 삭제
  - 페이지네이션 (10개씩)

### 5. 문의하기 (`/contact`)
- 문의 폼 제출
- 이메일 발송 기능

### 6. 공지사항 (`/notice`)
- 공지사항 목록
- 공지사항 상세 페이지 (`/notice/[id]`)

### 7. QCC (`/qcc`)
- 퀀텀체인(QCC) 관련 정보

---

## 🔧 주요 컴포넌트

### TiptapEditor
- 리치 텍스트 에디터
- 이미지 업로드 지원
- 텍스트 포맷팅 (Bold, Italic, Strike, Code 등)
- 리스트, 인용구, 코드 블록 지원

### NoticeBoard
- 공지사항 목록 표시
- 페이지네이션
- 수정/삭제 기능 (관리자 모드)

### ContactSection
- 문의 폼
- 모달 기반 UI

### Header
- 반응형 네비게이션
- 투명 모드 지원

---

## 🚀 실행 스크립트

```json
{
  "dev": "next dev --turbopack",      // 개발 서버 (Turbopack 사용)
  "build": "next build",              // 프로덕션 빌드
  "start": "next start -H 0.0.0.0",   // 프로덕션 서버 (모든 IP 허용)
  "lint": "next lint"                 // ESLint 실행
}
```

---

## 🎯 주요 특징

### 1. 블록체인 전문 기업 웹사이트
- 2016년부터 축적된 블록체인 노드 운영 경험
- Bitcoin, Ethereum, Filecoin, Aleo 등 주요 코인 노드 운영
- 퀀텀체인(QCC) 자체 개발

### 2. 다양한 호스팅 솔루션
- 웹/클라우드/서버/DB 호스팅 서비스
- NFT 특화 호스팅
- 블록체인 기반 노드 운영 솔루션

### 3. 관리자 시스템
- 문의 관리 시스템
- 공지사항 CMS (Content Management System)
- TipTap 기반 리치 텍스트 에디터

### 4. 현대적인 UI/UX
- Tailwind CSS 기반 반응형 디자인
- Pretendard 폰트 사용
- 그라디언트 및 모던한 색상 팔레트 (#04AAAB 메인 컬러)

### 5. SEO 최적화
- Next.js Metadata API 활용
- 한국어 최적화 (lang="ko")
- 구조화된 페이지 구성

---

## 🔐 보안 고려사항

1. **관리자 인증**: 하드코딩된 비밀번호 사용 (개선 필요)
2. **환경 변수**: `.env` 파일로 민감 정보 관리
3. **API 보호**: 관리자 기능에 대한 인증 필요

---

## 📈 개선 제안

### 보안
- [ ] 관리자 인증을 JWT 또는 세션 기반으로 개선
- [ ] 비밀번호 해싱 적용
- [ ] API 라우트에 인증 미들웨어 추가

### 기능
- [ ] 이미지 최적화 (Next.js Image 컴포넌트 활용)
- [ ] 검색 기능 추가 (공지사항, 솔루션)
- [ ] 다국어 지원 (i18n)
- [ ] 문의 이메일 알림 기능

### 성능
- [ ] 이미지 lazy loading
- [ ] 코드 스플리팅 최적화
- [ ] 캐싱 전략 수립

### UX
- [ ] 로딩 스피너 추가
- [ ] 에러 바운더리 구현
- [ ] 접근성 개선 (ARIA 레이블)

---

## 📝 회사 연혁 요약

- **2016**: 태강마이닝핀테크 설립, BTC 노드 운영 시작
- **2017**: HTS 코인 MOU, TG 코인 자체 개발
- **2019**: 고려대 정보보호대학원 양해각서
- **2020**: FileCoin 노드 운영
- **2022**: 기업부설연구소 등록, QCC 개발 착수
- **2023~2025**: 벤처 기업 등록, QCC 메인넷 오픈, 두바이 재단법인 설립

---

## 🌐 핵심 가치

1. **기술 혁신 (Innovation)**: 연구 개발, 블록체인, 보안 지속 개발
2. **투명성 (Transparency)**: 실시간 데이터 모니터링, 투명한 재무 환경
3. **전문성 (Expertise)**: 분야별 전문 인력의 체계적 시스템 통합
4. **고객중심 (Customer Centric)**: 맞춤형 솔루션 및 지속적 고객지원
5. **글로벌 지향 (Global Vision)**: 국내외 연결 및 글로벌 시장 진출

---

## 📞 문의 정보

프로젝트 관련 문의는 웹사이트의 문의하기 페이지를 통해 가능합니다.

---

**작성일**: 2025-10-13  
**분석 도구**: Windsurf AI  
**프로젝트 경로**: `/Users/sinseonghyeon/Documents/GitHub/01-blockchain-projects/maxpia-project/maxpia-web`
