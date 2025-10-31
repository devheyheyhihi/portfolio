# 🔮 Gwansang AI 파이프라인 아키텍처

## AI 분석 파이프라인 개요

```
┌─────────────────────────────────────────────────────────────────┐
│                    사용자 이미지 업로드                          │
└─────────────────────┬───────────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────────┐
│                 이미지 전처리 단계                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │   이미지    │  │   품질      │  │   보안      │            │
│  │   검증     │  │   평가      │  │   검사     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
└─────────────────────┬───────────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────────┐
│                 얼굴 검출 및 분석                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │   MTCNN     │  │   FAN       │  │   품질      │            │
│  │ (얼굴검출)  │  │ (랜드마크)  │  │   평가     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
└─────────────────────┬───────────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────────┐
│                 AI 모델 분석 단계                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │   관상      │  │   성격      │  │   운세      │            │
│  │   분석     │  │   분석     │  │   예측     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
└─────────────────────┬───────────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────────┐
│                 결과 통합 및 후처리                            │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │   결과      │  │   신뢰도   │  │   설명     │            │
│  │   통합     │  │   계산     │  │   생성     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
└─────────────────────┬───────────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────────┐
│                 사용자 결과 제공                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │   시각화    │  │   텍스트    │  │   데이터    │            │
│  │   결과     │  │   설명     │  │   삭제     │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
└─────────────────────────────────────────────────────────────────┘
```

## 상세 AI 모델 구조

### 1. 얼굴 검출 및 전처리
```python
class FaceDetectionPipeline:
    def __init__(self):
        self.mtcnn = MTCNN(
            min_face_size=20,
            thresholds=[0.6, 0.7, 0.7],
            factor=0.709
        )
        self.quality_assessor = ImageQualityModel()
        self.face_aligner = FaceAligner()
    
    async def process_image(self, image_path: str) -> ProcessedFace:
        # 1. 얼굴 검출
        faces = self.mtcnn.detect(image_path)
        if not faces:
            raise NoFaceDetectedError("얼굴을 찾을 수 없습니다")
        
        # 2. 품질 평가
        quality_score = await self.quality_assessor.assess(faces[0])
        if quality_score < 0.7:
            raise LowQualityError("이미지 품질이 부족합니다")
        
        # 3. 얼굴 정렬
        aligned_face = await self.face_aligner.align(faces[0])
        
        return ProcessedFace(
            face=aligned_face,
            quality_score=quality_score,
            landmarks=faces[0].landmarks
        )
```

### 2. 랜드마크 추출 및 특성 분석
```python
class LandmarkExtractor:
    def __init__(self):
        self.fan_model = FAN(
            num_modules=4,
            end_relu=False,
            num_landmarks=68
        )
        self.feature_extractor = GwansangFeatureExtractor()
    
    async def extract_features(self, face: ProcessedFace) -> FaceFeatures:
        # 68개 랜드마크 추출
        landmarks = await self.fan_model.predict(face.face)
        
        # 관상 특성 추출
        features = self.feature_extractor.extract(landmarks)
        
        return FaceFeatures(
            landmarks=landmarks,
            eye_features=features.eye_features,
            nose_features=features.nose_features,
            mouth_features=features.mouth_features,
            face_shape=features.face_shape,
            symmetry_score=features.symmetry_score
        )
```

### 3. 관상 분석 모델
```python
class GwansangAnalyzer:
    def __init__(self):
        self.face_analyzer = GwansangCNN()
        self.trait_classifier = TraitClassifier()
        self.confidence_calculator = ConfidenceCalculator()
    
    async def analyze_face(self, features: FaceFeatures) -> GwansangResult:
        # 관상 특성 분석
        face_analysis = await self.face_analyzer.analyze(features)
        
        # 특성 분류
        traits = await self.trait_classifier.classify(face_analysis)
        
        # 신뢰도 계산
        confidence = await self.confidence_calculator.calculate(
            features, face_analysis, traits
        )
        
        return GwansangResult(
            face_type=traits.face_type,
            characteristics=traits.characteristics,
            strengths=traits.strengths,
            weaknesses=traits.weaknesses,
            confidence=confidence,
            detailed_analysis=face_analysis
        )
```

### 4. 성격 분석 모델
```python
class PersonalityAnalyzer:
    def __init__(self):
        self.personality_model = PersonalityCNN()
        self.big5_classifier = Big5Classifier()
        self.trait_predictor = TraitPredictor()
    
    async def analyze_personality(self, features: FaceFeatures) -> PersonalityResult:
        # 성격 특성 예측
        personality_features = await self.personality_model.predict(features)
        
        # 5대 성격 특성 분류
        big5_scores = await self.big5_classifier.classify(personality_features)
        
        # 세부 성격 특성 예측
        detailed_traits = await self.trait_predictor.predict(
            personality_features, big5_scores
        )
        
        return PersonalityResult(
            big5_scores=big5_scores,
            detailed_traits=detailed_traits,
            personality_type=self._determine_personality_type(big5_scores),
            confidence=personality_features.confidence
        )
```

### 5. 운세 예측 모델
```python
class FortunePredictor:
    def __init__(self):
        self.fortune_model = FortuneLSTM()
        self.luck_calculator = LuckCalculator()
        self.trend_predictor = TrendPredictor()
    
    async def predict_fortune(self, 
                            face_features: FaceFeatures,
                            personality: PersonalityResult) -> FortuneResult:
        # 운세 특성 추출
        fortune_features = self._extract_fortune_features(face_features, personality)
        
        # 운세 예측
        fortune_prediction = await self.fortune_model.predict(fortune_features)
        
        # 행운도 계산
        luck_score = await self.luck_calculator.calculate(fortune_features)
        
        # 트렌드 예측
        trends = await self.trend_predictor.predict(fortune_features)
        
        return FortuneResult(
            overall_fortune=fortune_prediction.overall,
            career_fortune=fortune_prediction.career,
            love_fortune=fortune_prediction.love,
            health_fortune=fortune_prediction.health,
            luck_score=luck_score,
            trends=trends,
            confidence=fortune_prediction.confidence
        )
```

## 데이터 플로우 및 처리 과정

### 1. 이미지 업로드 및 검증
```
사용자 이미지 → 파일 형식 검증 → 크기 제한 확인 → 보안 스캔 → 암호화 저장
```

### 2. 얼굴 검출 및 품질 평가
```
이미지 → MTCNN 얼굴 검출 → 품질 점수 계산 → 최적 얼굴 선택 → 얼굴 정렬
```

### 3. AI 분석 파이프라인
```
얼굴 이미지 → 랜드마크 추출 → 특성 분석 → 관상 분석 → 성격 분석 → 운세 예측
```

### 4. 결과 통합 및 후처리
```
관상 결과 + 성격 분석 + 운세 예측 → 결과 통합 → 신뢰도 계산 → 설명 생성
```

### 5. 개인정보 보호 처리
```
분석 완료 → 결과 암호화 → 사용자 전송 → 원본 이미지 삭제 → 로그 정리
```

## 성능 최적화 전략

### 1. 모델 최적화
- **양자화**: INT8 양자화로 추론 속도 2배 향상
- **프루닝**: 불필요한 가중치 제거로 모델 크기 30% 감소
- **배치 처리**: 여러 이미지 동시 처리로 GPU 활용률 향상

### 2. 캐싱 전략
- **모델 캐싱**: 자주 사용되는 모델을 GPU 메모리에 상주
- **결과 캐싱**: 유사한 얼굴 특성에 대한 결과 캐싱
- **특성 캐싱**: 중간 단계 특성 벡터 캐싱

### 3. 병렬 처리
- **파이프라인 병렬화**: 각 단계별 독립적 처리
- **GPU 스트리밍**: 여러 GPU에서 동시 처리
- **비동기 처리**: I/O 작업과 계산 작업 분리

## 모니터링 및 품질 관리

### 1. 모델 성능 모니터링
```python
class ModelMonitor:
    def __init__(self):
        self.performance_tracker = PerformanceTracker()
        self.accuracy_monitor = AccuracyMonitor()
        self.drift_detector = DriftDetector()
    
    async def monitor_analysis(self, 
                              input_data: AnalysisInput,
                              output_data: AnalysisOutput) -> MonitoringResult:
        # 성능 지표 추적
        performance = await self.performance_tracker.track(input_data, output_data)
        
        # 정확도 모니터링
        accuracy = await self.accuracy_monitor.check(output_data)
        
        # 데이터 드리프트 감지
        drift = await self.drift_detector.detect(input_data)
        
        return MonitoringResult(
            performance=performance,
            accuracy=accuracy,
            drift_detected=drift,
            recommendations=self._generate_recommendations(performance, accuracy, drift)
        )
```

### 2. 품질 보증 체계
- **A/B 테스트**: 모델 버전별 성능 비교
- **인간 평가**: 전문가 검증을 통한 정확도 측정
- **사용자 피드백**: 결과 만족도 기반 모델 개선
- **편향성 감지**: 인구통계학적 편향 지속적 모니터링

## 보안 및 개인정보 보호

### 1. 데이터 암호화
```python
class PrivacyProtection:
    def __init__(self):
        self.encryption = AESEncryption(key_size=256)
        self.secure_deletion = SecureDeletion()
        self.audit_logger = AuditLogger()
    
    async def secure_process(self, image_data: bytes) -> str:
        # 이미지 암호화
        encrypted_data = self.encryption.encrypt(image_data)
        
        # 감사 로그 기록
        await self.audit_logger.log_access(image_data.hash())
        
        # 자동 삭제 스케줄링
        self.secure_deletion.schedule_deletion(encrypted_data, delay_hours=24)
        
        return encrypted_data
```

### 2. 개인정보 보호 메커니즘
- **최소 수집 원칙**: 분석에 필요한 최소한의 데이터만 수집
- **자동 삭제**: 분석 완료 후 24시간 내 자동 삭제
- **익명화**: 개인 식별 정보 완전 제거
- **접근 제어**: 승인된 사용자만 데이터 접근 가능

---

*이 AI 파이프라인은 최신 딥러닝 기술을 활용하여 정확하고 신뢰할 수 있는 관상 분석을 제공하면서도, 사용자의 개인정보 보호를 최우선으로 고려한 설계입니다.*
