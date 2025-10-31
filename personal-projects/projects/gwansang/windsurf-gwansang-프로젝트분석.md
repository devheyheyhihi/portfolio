# Windsurf - Gwansang (관상) 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: Gwansang (관상 - AI 관상가)  
**프로젝트 유형**: Flask 기반 웹 애플리케이션  
**주요 기능**: 얼굴 사진 업로드를 통한 AI 관상 분석 및 운세 풀이  
**개발 언어**: Python, JavaScript, HTML/CSS  
**배포 환경**: Render (클라우드 플랫폼)

---

## 🎯 프로젝트 목적

한국 전통 관상학을 현대적으로 재해석하여, 사용자가 업로드한 얼굴 사진을 분석하고 성격, 운세, 직업운, 건강운 등을 제공하는 웹 애플리케이션입니다. OpenCV를 활용한 얼굴 인식 기술과 전통 관상학 이론을 결합하여 재미있는 사용자 경험을 제공합니다.

---

## 📁 프로젝트 구조

```
gwansang/
├── app.py                          # Flask 메인 애플리케이션
├── analysis.py                     # 얼굴 분석 로직 (현재 미사용)
├── requirements.txt                # Python 의존성 패키지
├── render.yaml                     # Render 배포 설정
├── .gitignore                      # Git 제외 파일 목록
├── shape_predictor_68_face_landmarks.dat  # dlib 얼굴 랜드마크 모델 (약 95MB)
│
├── templates/                      # HTML 템플릿
│   ├── base.html                  # 기본 레이아웃 템플릿
│   ├── index.html                 # 메인 페이지
│   └── result.html                # 결과 페이지
│
├── static/                         # 정적 파일
│   ├── style.css                  # 스타일시트 (한국 전통 디자인)
│   ├── script.js                  # 클라이언트 사이드 JavaScript
│   ├── images/                    # 이미지 리소스
│   │   └── minhwa/               # 민화 장식 이미지들
│   └── uploads/                   # 업로드된 이미지 저장소
│
└── uploads/                        # 서버 업로드 폴더 (gitignore)
```

---

## 🔧 기술 스택

### Backend
- **Flask 2.3.0+**: Python 웹 프레임워크
- **OpenCV 4.8.0+**: 컴퓨터 비전 라이브러리 (얼굴 검출)
- **Pillow 10.0.0+**: 이미지 처리
- **NumPy 1.24.0+**: 수치 연산
- **Gunicorn 20.0.0+**: WSGI HTTP 서버 (프로덕션)

### Frontend
- **HTML5**: 구조
- **CSS3**: 스타일링 (한국 전통 디자인 테마)
- **Vanilla JavaScript**: 클라이언트 로직
- **Google Fonts**: Noto Serif KR, Gowun Batang (한글 폰트)

### 배포
- **Render**: 클라우드 호스팅 플랫폼
- **Python 3.11.0**: 런타임 환경

---

## 🎨 디자인 특징

### 한국 전통 테마
- **색상 팔레트**:
  - `--hanji-bg`: #fdfaf4 (한지 배경색)
  - `--ink-text`: #403a35 (먹물 텍스트)
  - `--point-gold`: #b18f4a (포인트 금색)
  - `--accent-gold`: #d4af37 (강조 금색)

- **민화 장식 요소**: 
  - 호랑이와 까치
  - 용 그림
  - 화조도 (꽃과 새)
  - 책거리
  - 학과 복숭아
  - 강아지와 꽃새

- **애니메이션 효과**:
  - 민화 요소 부유 애니메이션 (floatLeft, floatRight 등)
  - 타이틀 글로우 효과
  - 카드 진입 애니메이션
  - 로딩 스피너 및 펄스 효과

---

## 🚀 주요 기능

### 1. 이미지 업로드
- **드래그 앤 드롭** 지원
- **파일 선택** 버튼
- **실시간 미리보기**
- **파일 크기 제한**: 10MB
- **지원 형식**: PNG, JPG, JPEG, GIF, BMP, WEBP

### 2. 얼굴 분석
- **Haar Cascade 얼굴 검출**: OpenCV 기반
- **얼굴 특징 추출**:
  - 얼굴형 (둥근형, 긴형, 타원형)
  - 얼굴 크기 (큰편, 보통, 작은편)
  - 얼굴 비율 (가로/세로)
  - 이마 비율
  - 눈 영역, 입 영역 분석

### 3. 관상학적 해석
- **성격 분석**: 얼굴형에 따른 성격 특성
- **전체 운세**: 복, 재물운, 명예운
- **직업운**: 적합한 직업 분야 추천
- **건강운**: 건강 관리 조언
- **지혜운**: 학업 및 지적 능력
- **행운의 요소**:
  - 행운의 색상
  - 행운의 방향
  - 행운의 숫자
  - 행운의 보석
- **인생 조언**: 개인 맞춤 조언 리스트

### 4. 결과 표시
- **시각적 카드 레이아웃**: 각 운세 항목별 카드
- **애니메이션 효과**: 순차적 카드 진입
- **공유 기능**: Web Share API 또는 URL 복사
- **재분석 기능**: 다시 분석하기 버튼

---

## 📊 데이터 흐름

```
1. 사용자 이미지 업로드
   ↓
2. 클라이언트 검증 (파일 타입, 크기)
   ↓
3. FormData로 서버 전송 (/analyze)
   ↓
4. 서버 측 검증
   ↓
5. OpenCV 얼굴 검출
   ↓
6. 얼굴 특징 분석 (analyze_facial_features)
   ↓
7. 관상학적 해석 생성 (generate_fortune_reading)
   ↓
8. JSON 응답 반환
   ↓
9. 결과 페이지로 리다이렉트 (/result)
   ↓
10. 결과 표시
```

---

## 🔍 핵심 코드 분석

### 1. 얼굴 검출 (app.py)

```python
# Haar Cascade 로드
face_cascade = cv2.CascadeClassifier(
    cv2.data.haarcascades + 'haarcascade_frontalface_default.xml'
)

# 얼굴 검출
faces = face_cascade.detectMultiScale(
    gray_image, 
    scaleFactor=1.1, 
    minNeighbors=5, 
    minSize=(30, 30)
)
```

### 2. 얼굴 특징 분석

```python
def analyze_facial_features(img, x, y, w, h):
    # 얼굴 비율 분석
    face_ratio = w / h
    
    # 얼굴형 분류
    if face_ratio > 0.85:
        face_shape = "둥근형"
    elif face_ratio < 0.75:
        face_shape = "긴형"
    else:
        face_shape = "타원형"
    
    # 이마, 눈, 입 영역 분석
    forehead_height = h // 3
    eye_region = face_roi[h//3 : 2*h//3, :]
    mouth_region = face_roi[2*h//3:, :]
    
    return analysis_data
```

### 3. 관상학적 해석

```python
def generate_fortune_reading(analysis):
    # 얼굴형별 성격 및 운세 매핑
    face_shape_meanings = {
        "둥근형": {
            "personality": "온화하고 친근한 성격...",
            "fortune": "인복이 많아...",
            "career": "서비스업, 상담, 교육...",
            "health": "소화기 계통 주의..."
        },
        # ... 다른 얼굴형들
    }
    
    # 행운의 요소 생성
    lucky_elements = get_lucky_elements(face_shape)
    
    # 인생 조언 생성
    advice = get_life_advice(analysis)
    
    return fortune_data
```

### 4. 클라이언트 업로드 처리 (script.js)

```javascript
function handleFileUpload(file) {
    // 파일 검증
    if (!file.type.startsWith('image/')) {
        alert('이미지 파일만 업로드 가능합니다.');
        return;
    }
    
    // 미리보기 표시
    const reader = new FileReader();
    reader.onload = (e) => showPreview(e.target.result);
    reader.readAsDataURL(file);
    
    // 서버로 전송
    const formData = new FormData();
    formData.append('file', file);
    
    fetch('/analyze', {
        method: 'POST',
        body: formData
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            window.location.href = '/result?data=' + 
                encodeURIComponent(JSON.stringify(data));
        }
    });
}
```

---

## 🌐 API 엔드포인트

### 1. `GET /`
- **설명**: 메인 페이지 렌더링
- **응답**: index.html 템플릿

### 2. `POST /analyze`
- **설명**: 이미지 분석 요청
- **요청 형식**: multipart/form-data
- **요청 파라미터**: 
  - `file`: 이미지 파일
- **응답 형식**: JSON
- **성공 응답**:
```json
{
    "success": true,
    "result": {
        "face_detected": true,
        "analysis": {
            "face_shape": "타원형",
            "face_size": "보통",
            "face_ratio": 0.82,
            "forehead_proportion": 0.33
        },
        "fortune": {
            "overall": "균형잡힌 성격으로...",
            "fortune": "전반적으로 균형잡힌...",
            "career": "관리직, 기획...",
            "health": "전반적으로 건강하나...",
            "wisdom": "균형잡힌 사고력...",
            "lucky_elements": {
                "color": "녹색, 베이지",
                "direction": "동쪽",
                "number": "3, 8",
                "stone": "에메랄드, 옥"
            },
            "advice": ["조언1", "조언2", ...]
        },
        "face_region": {
            "x": 100,
            "y": 150,
            "width": 200,
            "height": 250
        }
    }
}
```
- **실패 응답**:
```json
{
    "success": false,
    "error": "오류 메시지"
}
```

### 3. `GET /result`
- **설명**: 분석 결과 페이지
- **쿼리 파라미터**: 
  - `data`: JSON 인코딩된 분석 결과
- **응답**: result.html 템플릿

---

## 🎭 얼굴형별 관상 해석 로직

### 둥근형 (face_ratio > 0.85)
- **성격**: 온화하고 친근함, 포용력 뛰어남
- **운세**: 인복이 많음, 안정적 발전
- **직업**: 서비스업, 상담, 교육
- **건강**: 소화기 계통 주의
- **행운**: 주황/노랑, 남쪽, 2/7, 호박/황수정

### 긴형 (face_ratio < 0.75)
- **성격**: 신중하고 사려깊음, 계획성 뛰어남
- **운세**: 중년 이후 크게 성공
- **직업**: 연구, 기술, 전문직
- **건강**: 스트레스 관리 중요
- **행운**: 파랑/보라, 북쪽, 1/6, 사파이어/자수정

### 타원형 (0.75 ≤ face_ratio ≤ 0.85)
- **성격**: 균형잡힌 성격, 리더십 뛰어남
- **운세**: 균형잡힌 운세, 노력한 만큼 성과
- **직업**: 관리직, 기획, 창업
- **건강**: 전반적으로 건강, 과로 피하기
- **행운**: 녹색/베이지, 동쪽, 3/8, 에메랄드/옥

---

## 🔒 보안 및 검증

### 파일 업로드 보안
- **허용 확장자 제한**: PNG, JPG, JPEG, GIF, BMP, WEBP
- **파일 크기 제한**: 
  - 클라이언트: 10MB
  - 서버: 16MB (Flask MAX_CONTENT_LENGTH)
- **파일명 보안**: `secure_filename()` 사용
- **MIME 타입 검증**: 클라이언트 측에서 `file.type` 체크

### 에러 처리
- 얼굴 검출 실패 시 사용자 친화적 메시지
- Haar Cascade 로드 실패 시 서버 로그 출력
- 이미지 처리 오류 시 예외 처리 및 로그

---

## 🚀 배포 설정 (Render)

### render.yaml
```yaml
services:
  - type: web
    name: gwansang-app
    plan: free
    env: python
    buildCommand: "pip install --upgrade pip setuptools wheel && pip install -r requirements.txt"
    startCommand: "gunicorn app:app"
    envVars:
      - key: PYTHON_VERSION
        value: 3.11.0
```

### 배포 프로세스
1. **빌드 단계**: 
   - pip, setuptools, wheel 업그레이드
   - requirements.txt 의존성 설치
2. **시작 단계**: 
   - Gunicorn WSGI 서버로 Flask 앱 실행
3. **환경 변수**: 
   - Python 3.11.0 사용

---

## 📦 의존성 패키지

```txt
Flask>=2.3.0           # 웹 프레임워크
opencv-python>=4.8.0   # 컴퓨터 비전
Pillow>=10.0.0         # 이미지 처리
numpy>=1.24.0          # 수치 연산
gunicorn>=20.0.0       # WSGI 서버
setuptools>=68.0.0     # 패키지 관리
wheel>=0.38.0          # 패키지 빌드
```

---

## 🎨 UI/UX 특징

### 반응형 디자인
- **모바일 최적화**: 768px 이하 브레이크포인트
- **그리드 레이아웃**: CSS Grid 활용
- **유연한 카드**: `minmax(300px, 1fr)` 자동 조절

### 사용자 경험
- **드래그 앤 드롭**: 직관적인 파일 업로드
- **실시간 피드백**: 로딩 애니메이션, 미리보기
- **애니메이션**: 부드러운 전환 효과
- **접근성**: 시맨틱 HTML, ARIA 레이블

### 시각적 요소
- **민화 배경**: 한국 전통 미술 요소
- **한지 텍스처**: 배경 패턴 애니메이션
- **금색 강조**: 포인트 컬러로 고급스러움
- **그림자 효과**: 깊이감 있는 카드 디자인

---

## 🐛 알려진 이슈 및 개선 사항

### 현재 이슈
1. **analysis.py 미사용**: 
   - `analysis.py` 파일이 존재하지만 실제로는 `app.py`의 함수들이 사용됨
   - 코드 중복 및 혼란 가능성

2. **얼굴 검출 정확도**: 
   - Haar Cascade는 기본적인 검출만 가능
   - 측면, 각도 있는 얼굴 검출 어려움

3. **대용량 모델 파일**: 
   - `shape_predictor_68_face_landmarks.dat` (95MB)가 포함되어 있으나 실제 사용되지 않음

### 개선 제안
1. **딥러닝 모델 적용**: 
   - dlib, MediaPipe, 또는 Face Recognition 라이브러리 활용
   - 더 정확한 얼굴 랜드마크 검출

2. **코드 리팩토링**: 
   - `analysis.py` 제거 또는 통합
   - 함수 모듈화 개선

3. **데이터베이스 추가**: 
   - 분석 결과 저장
   - 사용자 히스토리 관리

4. **AI 모델 통합**: 
   - 실제 머신러닝 모델로 더 정교한 분석
   - 감정 인식, 나이 추정 등 추가 기능

5. **성능 최적화**: 
   - 이미지 리사이징 및 압축
   - 캐싱 전략 적용

6. **테스트 코드**: 
   - 단위 테스트 추가
   - 통합 테스트 작성

---

## 📈 확장 가능성

### 추가 기능 아이디어
1. **소셜 로그인**: OAuth 통합
2. **결과 저장 및 공유**: 이미지로 저장, SNS 공유
3. **다국어 지원**: 영어, 중국어 등
4. **프리미엄 기능**: 상세 분석, PDF 리포트
5. **커뮤니티**: 사용자 간 결과 공유 및 댓글
6. **통계 대시보드**: 관리자용 분석 통계
7. **API 제공**: 외부 서비스 연동

### 기술적 확장
1. **마이크로서비스 아키텍처**: 분석 엔진 분리
2. **메시지 큐**: Celery, RabbitMQ로 비동기 처리
3. **CDN 통합**: 정적 파일 배포 최적화
4. **컨테이너화**: Docker, Kubernetes 배포
5. **모니터링**: Sentry, New Relic 통합

---

## 🔧 로컬 개발 환경 설정

### 1. 저장소 클론
```bash
git clone <repository-url>
cd gwansang
```

### 2. 가상환경 생성 및 활성화
```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
# venv\Scripts\activate   # Windows
```

### 3. 의존성 설치
```bash
pip install -r requirements.txt
```

### 4. 필요한 디렉토리 생성
```bash
mkdir -p static/uploads
mkdir -p uploads
```

### 5. 애플리케이션 실행
```bash
python app.py
```

### 6. 브라우저에서 접속
```
http://localhost:5000
```

---

## 📝 코드 스타일 및 컨벤션

### Python
- **PEP 8** 스타일 가이드 준수
- **함수 네이밍**: snake_case
- **클래스 네이밍**: PascalCase (현재 클래스 없음)
- **상수**: UPPER_CASE

### JavaScript
- **ES6+** 문법 사용
- **함수 네이밍**: camelCase
- **이벤트 리스너**: addEventListener 사용
- **Fetch API**: 비동기 통신

### HTML/CSS
- **시맨틱 태그** 사용
- **BEM 방법론** 일부 적용
- **CSS 변수**: :root에 테마 색상 정의
- **반응형**: 모바일 우선 접근

---

## 🎓 학습 포인트

이 프로젝트를 통해 학습할 수 있는 주요 개념:

1. **Flask 웹 프레임워크**: 라우팅, 템플릿, 파일 업로드
2. **OpenCV**: 이미지 처리, 얼굴 검출
3. **프론트엔드 통합**: HTML/CSS/JS와 백엔드 연동
4. **RESTful API**: JSON 응답, HTTP 메서드
5. **파일 처리**: 이미지 업로드, 검증, 저장
6. **배포**: Render 클라우드 플랫폼 사용
7. **UI/UX 디자인**: 한국 전통 테마, 애니메이션
8. **에러 처리**: 예외 처리, 사용자 피드백

---

## 📚 참고 자료

### 기술 문서
- [Flask 공식 문서](https://flask.palletsprojects.com/)
- [OpenCV Python 튜토리얼](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)
- [Haar Cascade 얼굴 검출](https://docs.opencv.org/3.4/db/d28/tutorial_cascade_classifier.html)

### 디자인 참고
- [한국 민화 (Korean Minhwa)](https://en.wikipedia.org/wiki/Minhwa)
- [전통 색상 팔레트](https://www.korean.go.kr/)

### 관상학 참고
- 한국 전통 관상학 서적
- 동양 철학 및 오행 이론

---

## 👥 기여 및 라이선스

### 기여 방법
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

### 라이선스
이 프로젝트는 교육 및 재미 목적으로 제작되었습니다. 모든 관상 풀이는 엔터테인먼트 목적이며 실제 운세와는 무관합니다.

---

## 📞 문의 및 지원

프로젝트 관련 문의사항이나 버그 리포트는 GitHub Issues를 통해 제출해주세요.

---

**마지막 업데이트**: 2024년  
**프로젝트 상태**: 활성 개발 중  
**버전**: 1.0.0

---

## 🎉 결론

Gwansang 프로젝트는 한국 전통 문화와 현대 AI 기술을 결합한 독특한 웹 애플리케이션입니다. Flask와 OpenCV를 활용한 기본적인 얼굴 인식부터 시작하여, 향후 더 정교한 딥러닝 모델과 다양한 기능으로 확장할 수 있는 잠재력을 가지고 있습니다.

프로젝트의 강점:
- ✅ 직관적이고 아름다운 UI/UX
- ✅ 한국 전통 테마의 독특한 디자인
- ✅ 실용적인 얼굴 검출 및 분석
- ✅ 확장 가능한 아키텍처
- ✅ 클라우드 배포 준비 완료

이 문서가 프로젝트 이해와 향후 개발에 도움이 되기를 바랍니다! 🚀
