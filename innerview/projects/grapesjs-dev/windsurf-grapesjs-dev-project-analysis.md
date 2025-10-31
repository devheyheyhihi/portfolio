# Windsurf - GrapesJS 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: GrapesJS  
**버전**: 0.21.6  
**라이선스**: BSD-3-Clause  
**작성자**: Artur Arseniev  
**홈페이지**: http://grapesjs.com  
**저장소**: https://github.com/GrapesJS/grapesjs.git

### 프로젝트 설명
GrapesJS는 **무료 오픈소스 웹 빌더 프레임워크**로, HTML 템플릿을 빠르고 쉽게 구축할 수 있도록 도와주는 도구입니다. 주로 CMS 내부에서 동적 템플릿 생성을 가속화하기 위해 설계되었으며, 웹사이트, 뉴스레터, 모바일 앱 등에 사용할 수 있습니다.

---

## 🏗️ 프로젝트 구조

### 디렉토리 구조
```
grapesjs-dev/
├── src/                    # 소스 코드 (TypeScript)
│   ├── abstract/          # 추상 클래스 및 모듈
│   ├── asset_manager/     # 에셋 관리 모듈
│   ├── block_manager/     # 블록 관리 모듈
│   ├── canvas/            # 캔버스 모듈
│   ├── code_manager/      # 코드 관리 모듈
│   ├── commands/          # 명령어 시스템
│   ├── common/            # 공통 유틸리티
│   ├── css_composer/      # CSS 작성 모듈
│   ├── device_manager/    # 디바이스 관리 모듈
│   ├── dom_components/    # DOM 컴포넌트 시스템
│   ├── editor/            # 에디터 코어
│   ├── i18n/              # 국제화
│   ├── keymaps/           # 키보드 단축키
│   ├── modal_dialog/      # 모달 다이얼로그
│   ├── navigator/         # 네비게이터
│   ├── pages/             # 페이지 관리
│   ├── panels/            # 패널 시스템
│   ├── parser/            # HTML/CSS 파서
│   ├── plugin_manager/    # 플러그인 관리
│   ├── rich_text_editor/  # 리치 텍스트 에디터
│   ├── selector_manager/  # 셀렉터 관리
│   ├── storage_manager/   # 스토리지 관리
│   ├── style_manager/     # 스타일 관리
│   ├── trait_manager/     # 속성 관리
│   ├── undo_manager/      # 실행 취소 관리
│   ├── utils/             # 유틸리티 함수
│   └── index.ts           # 메인 엔트리 포인트
├── dist/                   # 빌드된 파일
├── docs/                   # 문서
├── test/                   # 테스트 파일
├── index.html             # 개발 데모 페이지
├── package.json           # 패키지 설정
├── tsconfig.json          # TypeScript 설정
└── webpack.config.js      # Webpack 설정
```

---

## 🔧 기술 스택

### 핵심 의존성
- **Backbone.js** (1.4.1) - MVC 프레임워크
- **Underscore.js** (^1.13.1) - 유틸리티 라이브러리
- **CodeMirror** (^5.63.0) - 코드 에디터
- **TypeScript** - 타입 안전성

### 개발 도구
- **Webpack** - 모듈 번들러
- **Jest** - 테스트 프레임워크
- **ESLint** - 코드 린팅
- **Prettier** - 코드 포맷팅
- **Sass** - CSS 전처리기
- **VuePress** - 문서화 도구
- **Husky** - Git 훅 관리

---

## 🎯 주요 기능

### 1. **Block Manager** (블록 관리자)
- 드래그 앤 드롭으로 블록 추가
- 사용자 정의 블록 생성 가능
- 카테고리별 블록 분류

### 2. **Style Manager** (스타일 관리자)
- 비주얼 스타일 편집
- CSS 속성 실시간 수정
- 반응형 디자인 지원

### 3. **Layer Manager** (레이어 관리자)
- 컴포넌트 계층 구조 시각화
- 드래그 앤 드롭으로 요소 재배치
- 요소 표시/숨김 제어

### 4. **Code Viewer** (코드 뷰어)
- HTML/CSS 코드 직접 편집
- 실시간 코드 미리보기
- 코드 하이라이팅

### 5. **Asset Manager** (에셋 관리자)
- 이미지 및 파일 업로드
- 로컬 및 원격 스토리지 지원
- 에셋 라이브러리 관리

### 6. **Storage System** (스토리지 시스템)
- 로컬 스토리지 지원
- 원격 스토리지 연동 가능
- 자동 저장 기능

---

## 📦 빌드 시스템

### 빌드 스크립트
```json
{
  "build": "npm run check && npm run build-all && npm run ts:check",
  "build:js": "grapesjs-cli build --targets=\"> 1%, ie 11, safari 8, not dead\"",
  "build:mjs": "BUILD_MODULE=true grapesjs-cli build --dts='skip'",
  "build:css": "sass src/styles/scss/main.scss dist/css/grapes.min.css"
}
```

### 출력 파일
- `dist/grapes.min.js` - CommonJS 번들
- `dist/grapes.mjs` - ES Module 번들
- `dist/css/grapes.min.css` - 스타일시트
- `dist/index.d.ts` - TypeScript 타입 정의

### 브라우저 지원
- 시장 점유율 1% 이상
- Internet Explorer 11
- Safari 8 이상
- 최신 브라우저

---

## 🚀 개발 환경 설정

### 설치
```bash
git clone https://github.com/GrapesJS/grapesjs.git
cd grapesjs
yarn install
```

### 개발 서버 실행
```bash
yarn start
```
- 개발 서버: http://localhost:8080
- 핫 리로드 지원
- CORS 허용

### 테스트 실행
```bash
yarn test          # 전체 테스트
yarn test:dev      # 워치 모드
```

### 린팅 및 포맷팅
```bash
yarn lint          # ESLint + TypeScript 체크
yarn format        # Prettier 포맷팅
```

---

## 💻 사용 예제

### 기본 초기화
```javascript
const editor = grapesjs.init({
  container: '#gjs',
  height: '100%',
  fromElement: true,
  storageManager: { autoload: 0 },
  styleManager: {
    sectors: [
      {
        name: 'General',
        buildProps: ['float', 'display', 'position']
      },
      {
        name: 'Dimension',
        buildProps: ['width', 'height', 'margin', 'padding']
      }
    ]
  }
});
```

### 커스텀 블록 추가
```javascript
editor.BlockManager.add('testBlock', {
  label: 'Block',
  attributes: { class: 'gjs-fonts gjs-f-b1' },
  content: `<div style="padding:50px; text-align:center">Test block</div>`
});
```

### HTML/CSS 가져오기/내보내기
```javascript
// 가져오기
const html = editor.getHtml();
const css = editor.getCss();

// 내보내기
editor.setComponents(html);
editor.setStyle(css);
```

---

## 🔌 플러그인 시스템

### 공식 플러그인

#### Wrappers
- **@grapesjs/react** - React 래퍼

#### Extensions
- **grapesjs-plugin-export** - ZIP 아카이브 내보내기
- **grapesjs-plugin-ckeditor** - CKEditor 통합
- **grapesjs-blocks-basic** - 기본 블록 세트
- **grapesjs-plugin-forms** - 폼 컴포넌트
- **grapesjs-navbar** - 네비게이션 바
- **grapesjs-style-gradient** - 그라디언트 스타일
- **grapesjs-custom-code** - 커스텀 코드 임베드
- **grapesjs-indexeddb** - IndexedDB 스토리지
- **grapesjs-firestore** - Firestore 스토리지

#### Presets
- **grapesjs-preset-webpage** - 웹페이지 빌더
- **grapesjs-preset-newsletter** - 뉴스레터 빌더
- **grapesjs-mjml** - MJML 뉴스레터 빌더

### 플러그인 사용법
```javascript
const editor = grapesjs.init({
  container: '#gjs',
  plugins: ['grapesjs-blocks-basic', 'grapesjs-plugin-forms'],
  pluginsOpts: {
    'grapesjs-blocks-basic': { /* options */ },
    'grapesjs-plugin-forms': { /* options */ }
  }
});
```

---

## 🏛️ 아키텍처 패턴

### MVC 패턴
- **Model**: Backbone.Model 기반 데이터 모델
- **View**: Backbone.View 기반 UI 렌더링
- **Collection**: Backbone.Collection 기반 데이터 컬렉션

### 모듈 시스템
각 기능은 독립적인 모듈로 구성:
- `config/` - 설정
- `model/` - 데이터 모델
- `view/` - UI 뷰
- `index.ts` - 모듈 엔트리

### 이벤트 시스템
Backbone의 이벤트 시스템을 활용한 느슨한 결합

---

## 📝 TypeScript 설정

### 컴파일러 옵션
```json
{
  "target": "es5",
  "lib": ["dom", "dom.iterable", "esnext"],
  "strict": true,
  "esModuleInterop": true,
  "moduleResolution": "node"
}
```

### 타입 정의
- 모든 주요 클래스에 대한 타입 내보내기
- `dist/index.d.ts`에 통합된 타입 정의
- Backbone 타입 지원 (`@types/backbone`)

---

## 🧪 테스트

### Jest 설정
```json
{
  "testMatch": ["<rootDir>/test/specs/**/*.(t|j)s"],
  "setupFiles": ["<rootDir>/test/setup.js"],
  "testURL": "http://localhost/"
}
```

### 테스트 구조
- `test/specs/` - 테스트 스펙 파일
- `test/setup.js` - 테스트 환경 설정

---

## 📚 문서화

### VuePress 문서
- `docs/` - 문서 소스
- `docs/api/` - API 레퍼런스
- `docs/guides/` - 가이드
- `docs/modules/` - 모듈 문서

### 문서 빌드
```bash
yarn docs          # 개발 서버
yarn docs:build    # 프로덕션 빌드
yarn docs:deploy   # 배포
```

---

## 🔄 Git 워크플로우

### Pre-commit 훅
```json
{
  "lint-staged": {
    "{src,test}/**/*.(t|j)s": [
      "eslint --ext .ts,.js --fix",
      "prettier --single-quote --write",
      "git add"
    ]
  }
}
```

### 자동화
- ESLint 자동 수정
- Prettier 자동 포맷팅
- 커밋 전 자동 검증

---

## 🎨 현재 개발 상태 (index.html 분석)

### 데모 페이지 구성
현재 `index.html`은 개발용 데모 페이지로 다음 기능을 테스트하고 있습니다:

1. **에디터 초기화**
   - 컨테이너: `#gjs`
   - 높이: 100%
   - 오프셋 표시 활성화

2. **스타일 매니저 섹터**
   - General (float, display, position 등)
   - Flex (flexbox 속성)
   - Dimension (width, height, margin, padding)
   - Typography (폰트 관련)
   - Decorations (border, background 등)
   - Extra (transition, transform 등)

3. **커스텀 블록**
   - `testBlock`: 단일 블록
   - `testBlock2`: Flexbox 레이아웃 블록

4. **로컬 스토리지 테스트**
   - 빨간색 버튼 (`#test`): HTML/CSS 저장
   - 초록색 버튼 (`#test2`): HTML/CSS 불러오기

---

## 🌐 CDN 및 배포

### CDN 옵션
```html
<!-- UNPKG (최신 버전) -->
<link rel="stylesheet" href="https://unpkg.com/grapesjs/dist/css/grapes.min.css">
<script src="https://unpkg.com/grapesjs"></script>

<!-- CDNJS (특정 버전) -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/grapesjs/0.21.6/css/grapes.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/grapesjs/0.21.6/grapes.min.js"></script>
```

### NPM 설치
```bash
npm install grapesjs
```

---

## 🤝 기여 가이드

### 코드 스타일
- ESLint 규칙 준수
- Prettier 포맷팅 (single quotes, 120 line width)
- TypeScript strict 모드

### 커밋 전 체크리스트
- [ ] 린트 통과
- [ ] 테스트 통과
- [ ] 타입 체크 통과
- [ ] 문서 업데이트

---

## 📊 프로젝트 통계

- **소스 파일**: 288개 (src 디렉토리)
- **테스트 파일**: 79개
- **문서 파일**: 61개
- **주요 언어**: TypeScript
- **코드 스타일**: ESLint + Prettier
- **테스트 프레임워크**: Jest

---

## 🔗 유용한 링크

- **공식 웹사이트**: http://grapesjs.com
- **문서**: https://grapesjs.com/docs/
- **API 레퍼런스**: https://grapesjs.com/docs/api/
- **GitHub**: https://github.com/GrapesJS/grapesjs
- **Discord**: https://discord.gg/QAbgGXq
- **데모 - 웹페이지**: http://grapesjs.com/demo.html
- **데모 - 뉴스레터**: http://grapesjs.com/demo-newsletter-editor.html

---

## 📄 라이선스

BSD 3-Clause License

---

## 🎯 프로젝트 목표

GrapesJS는 다음을 목표로 합니다:

1. **빠른 템플릿 개발**: 드래그 앤 드롭으로 빠르게 HTML 템플릿 생성
2. **CMS 통합**: 다양한 CMS에 쉽게 통합 가능한 구조
3. **확장성**: 플러그인 시스템을 통한 기능 확장
4. **오픈소스**: 커뮤니티 기반 개발 및 무료 사용
5. **반응형 디자인**: 다양한 디바이스 지원

---

## 🚧 개발 시 주의사항

1. **Backbone 의존성**: 프로젝트는 Backbone.js에 크게 의존하고 있습니다.
2. **jQuery 대체**: `utils/cash-dom`을 jQuery 대신 사용합니다.
3. **모듈 해상도**: Webpack alias를 통해 `src` 디렉토리를 루트로 설정합니다.
4. **빌드 타겟**: IE11 지원을 위해 ES5로 트랜스파일됩니다.
5. **스토리지**: 자동 로드가 기본적으로 비활성화되어 있습니다 (`autoload: 0`).

---

## 📈 향후 개선 방향

1. **모던 프레임워크 지원**: React, Vue 등과의 더 나은 통합
2. **성능 최적화**: 대규모 프로젝트 처리 성능 개선
3. **UI/UX 개선**: 더 직관적인 사용자 인터페이스
4. **플러그인 생태계**: 더 많은 공식 플러그인 개발
5. **TypeScript 완전 전환**: 모든 코드를 TypeScript로 마이그레이션

---

*이 문서는 Windsurf AI에 의해 자동 생성되었습니다.*  
*작성일: 2025-09-30*
