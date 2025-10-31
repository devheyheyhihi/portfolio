# Windsurf QCN (QuantumChain) 프로젝트 분석

## 📋 프로젝트 개요

**프로젝트명**: QuantumChain (QCN)  
**버전**: 2.1.9.6  
**언어**: Go 1.22.5  
**타입**: 블록체인 네트워크 & GPU 마이닝 클라이언트

QuantumChain은 GPU 기반 마이닝을 지원하는 블록체인 네트워크 클라이언트입니다. CUDA를 활용한 SHA-256 해시 계산과 Raft 합의 알고리즘을 기반으로 한 분산 시스템입니다.

---

## 🏗️ 프로젝트 구조

```
qcn/
├── main.go                 # 메인 엔트리 포인트
├── go.mod                  # Go 모듈 의존성
├── network.ini             # 네트워크 설정 파일
├── init.sh                 # 초기화 스크립트
├── mine/
│   └── mine.cu            # CUDA 기반 마이닝 커널
├── pkg/
│   ├── core/              # 핵심 블록체인 로직
│   │   ├── abi/          # Application Binary Interface
│   │   ├── config/       # 설정 관리
│   │   ├── logging/      # 감사 로깅
│   │   ├── model/        # 데이터 모델 (Block, Transaction, Account)
│   │   ├── network/      # RPC 네트워크 통신
│   │   ├── storage/      # 블록체인 저장소 (RocksDB)
│   │   ├── structure/    # 데이터 구조 (MerkleTree, OrderedMap)
│   │   └── vm/           # 가상 머신 (스마트 컨트랙트 실행)
│   ├── crypto/           # 암호화 & 키 관리
│   ├── program/          # CLI 명령어 구현
│   ├── raft/             # Raft 합의 알고리즘
│   ├── rpc/              # RPC 프로토콜
│   ├── service/          # 서비스 레이어 (Oracle, Consensus)
│   ├── swift/            # 네트워크 통신 프로토콜
│   └── util/             # 유틸리티 함수
├── helptools/            # 헬퍼 도구 (Python 스크립트)
├── saseul-cli/           # CLI 도구
└── test/                 # 테스트 파일
```

---

## 🔑 핵심 기능

### 1. **블록체인 네트워크**
- **합의 알고리즘**: Raft 기반 분산 합의
- **저장소**: RocksDB를 사용한 영구 저장
- **네트워크 프로토콜**: Swift 프로토콜 (TLS 지원)
- **P2P 통신**: 노드 간 핸드셰이크 및 피어 관리

### 2. **GPU 마이닝**
- **CUDA 기반**: SHA-256 해시 계산을 GPU에서 병렬 처리
- **난이도 조정**: `network_diff` 설정으로 마이닝 난이도 제어
- **자동 제출**: 마이닝 성공 시 자동으로 트랜잭션 제출
- **마이닝 프로세스**:
  1. 최신 블록 높이 조회
  2. CUDA 커널로 nonce 계산
  3. 조건 만족 시 해시 제출

### 3. **지갑 관리**
- **키 생성**: Ed25519 기반 공개키/개인키 쌍 생성
- **키 저장소**: 암호화된 키스토어 (WALLET_PASSWORD 환경변수 사용)
- **주소 생성**: 공개키로부터 블록체인 주소 파생
- **트랜잭션 서명**: 개인키로 트랜잭션 서명

### 4. **트랜잭션 타입**
- **Send**: 토큰 전송
- **MultiSend**: 다중 수신자 전송 (최대 5명)
- **Faucet**: 테스트넷 토큰 요청
- **Mining**: 마이닝 결과 제출
- **Count**: 트랜잭션 카운트

### 5. **가상 머신 (VM)**
- 스마트 컨트랙트 실행 환경
- 연산자 지원:
  - 산술 연산 (Arithmetic)
  - 비교 연산 (Comparison)
  - 타입 변환 (Cast)
  - 체인 연산 (Chain)
  - 읽기 연산 (Read)

---

## 🛠️ 기술 스택

### Backend
- **언어**: Go 1.22.5
- **데이터베이스**: RocksDB (linxGnu/grocksdb)
- **로깅**: Uber Zap + Lumberjack (로그 로테이션)
- **CLI**: Cobra (spf13/cobra)
- **암호화**: golang.org/x/crypto

### Mining
- **언어**: CUDA C
- **해시 알고리즘**: SHA-256
- **병렬화**: GPU 스레드 (1,048,576 스레드)

### Network
- **프로토콜**: Swift (커스텀 프로토콜)
- **보안**: TLS 1.2+
- **압축**: 메시지 압축 지원

---

## 📦 주요 의존성

```go
require (
    github.com/linxGnu/grocksdb v1.9.8           // RocksDB 바인딩
    github.com/spf13/cobra v1.8.1                // CLI 프레임워크
    github.com/stretchr/testify v1.10.0          // 테스트 유틸리티
    go.uber.org/zap v1.27.0                      // 구조화 로깅
    golang.org/x/crypto v0.29.0                  // 암호화 라이브러리
    gopkg.in/ini.v1 v1.67.0                      // INI 파일 파싱
    gopkg.in/natefinch/lumberjack.v2 v2.2.1      // 로그 로테이션
)
```

---

## 🚀 시스템 요구사항

### 최소 사양
- **메모리**: 8GB (권장 12GB)
- **CPU**: 4코어 (권장 8코어)
- **디스크**: 96GB (권장 256GB)
- **GPU**: NVIDIA GPU (CUDA 지원)

### 소프트웨어 요구사항
- **CUDA**: 12.4
- **NVIDIA Driver**: 550.127.05
- **OS**: Ubuntu 22.04 (AWS Deep Learning AMI 권장)
- **필수 명령어**: `nvcc`, `nvidia-smi`

---

## 💻 CLI 명령어 구조

### Network 명령어
```bash
./qt network start      # 네트워크 노드 시작
./qt network stop       # 네트워크 노드 중지
./qt network info       # 네트워크 정보 조회
./qt network metrics    # 네트워크 메트릭 조회
./qt network replica    # 레플리카 등록
```

### Wallet 명령어
```bash
./qt wallet create      # 새 지갑 생성
./qt wallet set         # 기본 지갑 설정
./qt wallet balance     # 잔액 조회
./qt wallet send        # 토큰 전송
./qt wallet multisend   # 다중 전송
./qt wallet faucet      # 테스트넷 토큰 요청
```

### Mining 명령어
```bash
./qt mining start       # 마이닝 시작
./qt mining stop        # 마이닝 중지
./qt mining submit      # 마이닝 결과 제출
```

### Node 명령어
```bash
./qt node ping          # 노드 핑
./qt node peer          # 피어 목록 조회
./qt node listmemtx     # 멤풀 트랜잭션 조회
./qt node status        # 노드 상태 조회
./qt node connect       # 노드 연결
```

---

## 🔐 보안 기능

### 1. **암호화**
- Ed25519 서명 알고리즘
- TLS 1.2+ 네트워크 통신
- 암호화된 키스토어

### 2. **감사 로깅**
- 모든 서비스 시작/중지 로깅
- 트랜잭션 검증 로깅
- 네트워크 이벤트 로깅

### 3. **트랜잭션 검증**
- 서명 검증
- 타임스탬프 검증
- 잔액 검증

---

## 📊 네트워크 설정 (network.ini)

```ini
[network]
version = 2.1.9.6
swift_port = 9002
swift_host = localhost
cli_default_request = localhost:9001
network_diff = 00000001b8e5b8a77d028ffce94e9488f150c1fdd6bcb8fb87ba12d4cf1a0ead
sg_hardfork_start_height = 475135
initial_supply = 669332766000000000000000000
is_replica = false
is_testnet = true

[storage]
status_db_path = status.rocks
tx_db_path = tx.rocks

[policy]
max_requests_in_batch = 128
publish_fee = 1000000000000000000
send_fee = 5000000000000
transfer_fee = 5000000000000
mint_fee = 10000000000000000000000
commit_block_interval_unit = 10
```

---

## 🔄 마이닝 프로세스 상세

### 1. **마이닝 시작**
```bash
./qt mining start
```

### 2. **프로세스 흐름**
1. **노드 정보 수집**: ipinfo.io API로 노드 정보 수집
2. **마이닝 등록**: api.saseulgold.org에 마이닝 노드 등록
3. **최신 블록 조회**: `./qt api lastheight`로 최신 블록 높이 조회
4. **Seed 생성**: `blk-{height},{timestamp}`
5. **CUDA 마이닝**: `./mine/cmine {height} {timestamp}` 실행
6. **결과 제출**: 조건 만족 시 `./qt mining submit` 실행
7. **반복**: 성공 또는 실패 후 1분 대기 후 재시도

### 3. **CUDA 커널 동작**
- **스레드 수**: 1,048,576 (1024 * 1024)
- **블록 크기**: 256 스레드/블록
- **해시 조건**: `state[0] < 0x00000001` (상위 32비트가 매우 작은 값)
- **Atomic 연산**: 최초 발견 nonce만 기록

---

## 🗄️ 저장소 구조

### RocksDB 데이터베이스
- **main_chain/**: 메인 체인 블록 데이터
- **status.rocks/**: 계정 상태 및 잔액
- **tx.rocks/**: 트랜잭션 인덱스
- **metastore.rocks/**: 메타데이터 (블록 높이, 체인 정보)

### 파일 시스템
- **chain_info**: 체인 정보 파일
- **proc/**: 프로세스 PID 파일
- **.wallet**: 암호화된 지갑 파일

---

## 🧪 테스트 도구

### Python 헬퍼 도구
- **blkcheck.py**: 블록 검증
- **chainreader.py**: 체인 읽기
- **indexer.py**: 인덱싱
- **txindexreader.py**: 트랜잭션 인덱스 읽기
- **verifyblock.py**: 블록 검증

### 테스트 스크립트
- **rafttest.py**: Raft 합의 테스트
- **stress.py**: 스트레스 테스트
- **sendscript.py**: 트랜잭션 전송 테스트

---

## 🌐 API 엔드포인트

### 외부 API
- **마이닝 등록**: `https://api.saseulgold.org/mining/v2/{address}`
- **노드 정보**: `http://ipinfo.io/json`

### 내부 RPC
- **PacketTypePing**: 노드 핑
- **PacketTypePeerRequest**: 피어 목록 요청
- **PacketTypeHandshakeCMDRequest**: 핸드셰이크
- **PacketTypeBroadcastTransactionRequest**: 트랜잭션 브로드캐스트
- **PacketTypeListMempoolTransactionRequest**: 멤풀 조회
- **PacketTypeOracleGetInfoRequest**: 오라클 정보 조회
- **PacketTypeShutdownOracleRequest**: 오라클 종료

---

## 🔧 개발 환경 설정

### 1. **프로젝트 다운로드**
```bash
wget https://raw.githubusercontent.com/Quantumchainlab/qt-cli/refs/heads/main/qt-main.zip
unzip qt-main.zip -d qt_network
cd qt_network
```

### 2. **초기화**
```bash
sh init.sh
```

### 3. **지갑 생성**
```bash
./qt wallet create
```

### 4. **지갑 설정**
```bash
./qt wallet set -k {private_key}
```

### 5. **마이닝 시작**
```bash
./qt mining start
```

---

## 📈 성능 최적화

### 1. **GPU 마이닝**
- 병렬 스레드 수 조정 가능
- CUDA 블록 크기 최적화
- Atomic 연산으로 중복 방지

### 2. **네트워크**
- 메시지 압축
- TLS 암호화 스위트 최적화
- 배치 요청 처리 (최대 128개)

### 3. **저장소**
- RocksDB 최적화
- 인덱싱 구조
- 레플리카 버퍼링

---

## 🐛 디버깅 & 로깅

### 로그 파일
- **mining.log**: 마이닝 로그
- **nohup.out**: 백그라운드 프로세스 로그
- **audit.log**: 감사 로그

### 디버그 모드
```bash
./qt network start --debug
```

---

## 🚦 프로젝트 상태

### 현재 버전
- **버전**: 2.1.9.6
- **하드포크 높이**: 475135
- **테스트넷**: 활성화

### 주요 특징
- ✅ GPU 마이닝 지원
- ✅ Raft 합의 알고리즘
- ✅ 스마트 컨트랙트 VM
- ✅ 다중 전송 지원
- ✅ 레플리카 노드 지원
- ✅ TLS 보안 통신

---

## 📝 개발 노트

### 코드 스타일
- Go 표준 포맷 준수
- Cobra CLI 패턴 사용
- 구조화 로깅 (Zap)

### 아키텍처 패턴
- **레이어드 아키텍처**: Core, Service, Program 레이어 분리
- **커맨드 패턴**: CLI 명령어 구조화
- **팩토리 패턴**: RPC 요청 생성

### 주요 개선 사항
- 암호화된 키스토어 도입
- 감사 로깅 시스템
- 메트릭 수집
- 레플리카 동기화

---

## 🔮 향후 개선 방향

1. **성능 최적화**
   - GPU 마이닝 알고리즘 개선
   - 네트워크 프로토콜 최적화
   - 저장소 압축

2. **기능 추가**
   - 스마트 컨트랙트 배포 기능
   - DEX (탈중앙화 거래소) 기능 확장
   - 크로스체인 브릿지

3. **보안 강화**
   - 멀티시그 지갑
   - 하드웨어 지갑 지원
   - 추가 암호화 알고리즘

4. **개발자 경험**
   - API 문서 자동 생성
   - SDK 제공
   - 테스트넷 파우셋 자동화

---

## 📚 참고 자료

- **GitHub**: https://github.com/Quantumchainlab/qt-cli
- **API 문서**: https://api.saseulgold.org
- **CUDA 프로그래밍**: NVIDIA CUDA Toolkit Documentation
- **Raft 합의**: https://raft.github.io

---

## 👥 기여

이 프로젝트는 QuantumChain Lab에서 개발 및 유지보수하고 있습니다.

---

## 📄 라이선스

프로젝트 라이선스 정보는 저장소를 참조하세요.

---

**문서 작성일**: 2025-10-13  
**분석 버전**: 2.1.9.6  
**작성자**: Windsurf AI Assistant
