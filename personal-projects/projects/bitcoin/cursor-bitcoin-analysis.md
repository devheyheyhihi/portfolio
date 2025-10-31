# Cursor Bitcoin Core 프로젝트 분석

## 프로젝트 개요

**Bitcoin Core**는 비트코인 네트워크의 참조 구현체로, 비트코인 프로토콜의 핵심 기능을 제공하는 오픈소스 소프트웨어입니다. 이 프로젝트는 C++로 작성되어 있으며, CMake 빌드 시스템을 사용합니다.

### 기본 정보
- **라이선스**: MIT License
- **언어**: C++ (C++20 표준)
- **빌드 시스템**: CMake 3.22+
- **버전**: 28.99.0 (현재 개발 버전)
- **저장소**: https://github.com/bitcoin/bitcoin

## 프로젝트 구조 분석

### 1. 루트 디렉토리 구조

```
bitcoin/
├── src/                    # 핵심 소스 코드
├── test/                   # 테스트 코드
├── doc/                    # 문서
├── contrib/                # 기여 도구 및 스크립트
├── ci/                     # CI/CD 관련 파일
├── cmake/                  # CMake 설정 파일
├── depends/                # 의존성 관리
├── share/                  # 공유 리소스
├── CMakeLists.txt          # 메인 빌드 설정
├── README.md              # 프로젝트 설명
├── CONTRIBUTING.md        # 기여 가이드라인
└── INSTALL.md             # 설치 가이드
```

### 2. 핵심 소스 코드 구조 (`src/`)

#### 2.1 주요 실행 파일
- **`bitcoind.cpp`**: 비트코인 데몬 (백그라운드 서비스)
- **`bitcoin-cli.cpp`**: 비트코인 CLI 도구
- **`bitcoin-tx.cpp`**: 트랜잭션 조작 도구
- **`bitcoin-util.cpp`**: 유틸리티 도구
- **`bitcoin-wallet.cpp`**: 지갑 관리 도구

#### 2.2 핵심 모듈

##### **네트워킹 (`net.cpp`, `net_processing.cpp`)**
- P2P 네트워크 통신
- 노드 간 연결 관리
- 메시지 처리 및 전파

##### **검증 (`validation.cpp`)**
- 블록 및 트랜잭션 검증
- 합의 규칙 적용
- 체인 상태 관리

##### **합의 (`consensus/`)**
- `amount.h`: 비트코인 금액 처리
- `merkle.cpp`: 머클 트리 구현
- `tx_check.cpp`: 트랜잭션 검증
- `tx_verify.cpp`: 트랜잭션 서명 검증

##### **스크립트 (`script/`)**
- `script.cpp`: 비트코인 스크립트 엔진
- `interpreter.cpp`: 스크립트 해석기
- `sign.cpp`: 트랜잭션 서명
- `descriptor.cpp`: 지갑 디스크립터

##### **지갑 (`wallet/`)**
- 지갑 기능 구현
- 트랜잭션 생성 및 관리
- 키 관리 및 암호화
- RPC 인터페이스

##### **RPC (`rpc/`)**
- JSON-RPC API 서버
- 블록체인 데이터 조회
- 트랜잭션 처리
- 네트워크 정보 제공

### 3. 빌드 시스템 분석

#### 3.1 CMake 설정
- **최소 CMake 버전**: 3.22
- **C++ 표준**: C++20
- **크로스 컴파일 지원**: Windows, Linux, macOS
- **의존성 관리**: Boost, Libevent, SQLite3, Berkeley DB

#### 3.2 주요 빌드 옵션
```cmake
option(BUILD_DAEMON "Build bitcoind executable." ON)
option(BUILD_GUI "Build bitcoin-qt executable." OFF)
option(BUILD_CLI "Build bitcoin-cli executable." ON)
option(ENABLE_WALLET "Enable wallet." ON)
option(WITH_SQLITE "Enable SQLite wallet support." ON)
option(WITH_BDB "Enable Berkeley DB wallet support." OFF)
```

### 4. 테스트 구조

#### 4.1 테스트 유형
- **Unit Tests**: `src/test/` - 개별 컴포넌트 테스트
- **Functional Tests**: `test/functional/` - 통합 테스트
- **Fuzz Tests**: `test/fuzz/` - 퍼징 테스트
- **Lint Tests**: `test/lint/` - 코드 품질 검사

#### 4.2 테스트 실행
```bash
# 단위 테스트
ctest

# 기능 테스트
build/test/functional/test_runner.py

# 퍼징 테스트
build/test/fuzz/test_runner.py
```

### 5. 주요 기능 모듈

#### 5.1 블록체인 관리
- **체인 상태**: `chain.cpp`, `chain.h`
- **블록 저장**: `txdb.cpp`, `txdb.h`
- **UTXO 관리**: `coins.cpp`, `coins.h`
- **메모리 풀**: `txmempool.cpp`, `txmempool.h`

#### 5.2 암호화
- **해시 함수**: `hash.cpp`, `hash.h`
- **키 관리**: `key.cpp`, `key.h`
- **서명**: `pubkey.cpp`, `pubkey.h`
- **secp256k1**: 타원곡선 암호화 라이브러리

#### 5.3 네트워크
- **주소 관리**: `addrman.cpp`, `addrman.h`
- **연결 관리**: `net.cpp`, `net.h`
- **메시지 처리**: `net_processing.cpp`
- **프로토콜**: `protocol.cpp`, `protocol.h`

### 6. 개발 환경 및 도구

#### 6.1 개발 도구
- **Guix**: 재현 가능한 빌드
- **Docker**: 컨테이너화된 개발 환경
- **Valgrind**: 메모리 디버깅
- **Sanitizers**: Address, Thread, Undefined Behavior

#### 6.2 코드 품질
- **정적 분석**: Clang Static Analyzer
- **코드 스타일**: 자동 포맷팅
- **의존성 검사**: 순환 의존성 방지
- **문서화**: Doxygen 지원

### 7. 보안 및 성능

#### 7.1 보안 기능
- **하드닝**: 스택 보호, ASLR, DEP
- **메모리 보호**: Address Sanitizer
- **암호화**: secp256k1, SHA-256
- **랜덤성**: 안전한 난수 생성

#### 7.2 성능 최적화
- **캐싱**: 메모리 풀 캐시
- **병렬 처리**: 멀티스레딩
- **압축**: 블록 압축
- **인덱싱**: 빠른 조회를 위한 인덱스

### 8. 확장성 및 모듈성

#### 8.1 플러그인 아키텍처
- **인덱스**: 블록 필터, 코인 통계
- **RPC**: 사용자 정의 RPC 명령
- **지갑**: 다양한 지갑 백엔드

#### 8.2 설정 및 초기화
- **설정 파일**: `bitcoin.conf`
- **명령행 옵션**: 다양한 런타임 옵션
- **환경 변수**: 시스템 설정

### 9. 문서화 및 기여

#### 9.1 문서 구조
- **API 문서**: RPC 인터페이스
- **개발자 가이드**: `doc/developer-notes.md`
- **빌드 가이드**: `doc/build-*.md`
- **릴리즈 노트**: `doc/release-notes/`

#### 9.2 기여 프로세스
- **코드 리뷰**: 필수 피어 리뷰
- **테스트**: 모든 변경사항에 대한 테스트
- **문서화**: 코드 변경에 따른 문서 업데이트
- **라이선스**: MIT 라이선스 준수

## 결론

Bitcoin Core는 매우 잘 구조화된 대규모 C++ 프로젝트로, 다음과 같은 특징을 가지고 있습니다:

1. **모듈화된 아키텍처**: 각 기능이 명확히 분리된 모듈로 구성
2. **강력한 테스트 시스템**: 단위, 기능, 퍼징 테스트의 완전한 커버리지
3. **보안 중심 설계**: 암호화, 메모리 보호, 하드닝 기능
4. **크로스 플랫폼 지원**: Windows, Linux, macOS 지원
5. **활발한 개발 커뮤니티**: 오픈소스 기여 프로세스

이 프로젝트는 비트코인 네트워크의 핵심 인프라를 제공하며, 블록체인 기술의 참조 구현체로서 중요한 역할을 합니다.
