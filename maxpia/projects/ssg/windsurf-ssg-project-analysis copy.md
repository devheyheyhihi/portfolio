# Windsurf SSG Project Analysis

## 프로젝트 개요

**프로젝트명**: SASEUL (SSG)  
**버전**: v2.1.9.6  
**개발사**: ArtiFriends Inc.  
**저작권**: 2019-2023  
**라이선스**: 상업적 사용 금지 (Commercial use strictly prohibited)

SASEUL은 PHP 기반의 블록체인 플랫폼으로, 분산 원장 기술과 합의 알고리즘을 구현한 실험적 블록체인 시스템입니다.

---

## 📁 프로젝트 구조

```
ssg/
├── src/                          # 소스 코드 디렉토리
│   ├── Core/                     # 핵심 시스템 컴포넌트
│   │   ├── Api.php              # API 핸들러
│   │   ├── Loader.php           # 클래스 로더
│   │   ├── Logger.php           # 로깅 시스템
│   │   ├── Process.php          # 프로세스 관리
│   │   ├── Result.php           # 결과 처리
│   │   ├── Script.php           # 스크립트 실행기
│   │   └── Service.php          # 서비스 관리
│   │
│   ├── Saseul/                  # SASEUL 블록체인 구현
│   │   ├── Api/                 # API 엔드포인트 (14개)
│   │   │   ├── Block.php
│   │   │   ├── Broadcast.php
│   │   │   ├── Transaction.php
│   │   │   └── ...
│   │   │
│   │   ├── Service/             # 백그라운드 서비스 (6개)
│   │   │   ├── ChainMaker.php   # 블록 생성
│   │   │   ├── Collector.php    # 데이터 수집
│   │   │   ├── DataPool.php     # 데이터 풀 관리
│   │   │   ├── Master.php       # 마스터 노드
│   │   │   ├── PeerSearcher.php # 피어 검색
│   │   │   └── ResourceMiner.php # 리소스 마이닝
│   │   │
│   │   ├── Script/              # CLI 스크립트 (26개)
│   │   │   ├── Genesis.php
│   │   │   ├── Start.php
│   │   │   ├── Stop.php
│   │   │   ├── StartMining.php
│   │   │   └── ...
│   │   │
│   │   ├── Data/                # 데이터 모델 (7개)
│   │   ├── DataSource/          # 데이터 소스 (11개)
│   │   ├── Model/               # 비즈니스 모델 (13개)
│   │   ├── VM/                  # 가상 머신 (16개)
│   │   ├── RPC/                 # RPC 인터페이스 (3개)
│   │   ├── Staff/               # 스태프 유틸리티 (3개)
│   │   ├── Config.php           # 설정 관리
│   │   └── Common.php           # 공통 유틸리티
│   │
│   ├── IPC/                     # 프로세스 간 통신
│   │   ├── TCPBase.php
│   │   ├── TCPClient.php
│   │   ├── TCPSocket.php
│   │   ├── UDPBase.php
│   │   ├── UDPClient.php
│   │   └── UDPSocket.php
│   │
│   ├── Util/                    # 유틸리티 함수
│   ├── Test/                    # 테스트 코드
│   ├── Patch/                   # 하드포크 패치
│   ├── Code/                    # 코드 관리
│   │
│   ├── index.php                # API 진입점
│   ├── load_core.php            # 코어 로더
│   ├── load_saseul.php          # SASEUL 로더
│   ├── saseul-script            # CLI 스크립트 실행기
│   ├── saseul-install           # 설치 스크립트
│   └── saseulsvc                # 서비스 데몬
│
├── mine.cu                      # CUDA 마이닝 프로그램
├── sha256.cuh                   # SHA-256 CUDA 헤더
├── picosha2.h                   # PicoSHA2 라이브러리
├── build.sh                     # 마이닝 빌드 스크립트
├── sync.sh                      # 동기화 스크립트
├── loader.json                  # 클래스 로더 설정
├── saseul.ini                   # 노드 설정 파일
├── saseul.ini-sample            # 설정 샘플
├── Readme.md                    # 프로젝트 문서
└── data/                        # 블록체인 데이터 (gitignore)
```

---

## 🔧 기술 스택

### Backend
- **언어**: PHP 7.x+
- **필수 확장**:
  - `bcmath` - 고정밀 수학 연산
  - `mbstring` - 멀티바이트 문자열 처리
  - `sodium` - 암호화
  - `posix` / `pcntl` - 프로세스 제어 (Linux)
  - `com_dotnet` - COM 인터페이스 (Windows)
  - `mysqlnd` - MySQL 드라이버 (선택)

### Mining
- **언어**: CUDA C++
- **라이브러리**: 
  - CUDA Runtime
  - OpenSSL (libssl, libcrypto)
  - PicoSHA2

### Database
- **지원**: MySQL (선택적)
- **기본**: 파일 기반 저장소

### Network
- **프로토콜**: TCP/UDP
- **통신**: IPC (Inter-Process Communication)
- **API**: RESTful HTTP API

---

## 🏗️ 아키텍처

### 1. 레이어 구조

```
┌─────────────────────────────────────────┐
│         API Layer (HTTP/REST)           │
│  - Block, Transaction, Peer, Status     │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│        Service Layer (Daemon)           │
│  - ChainMaker, Collector, DataPool      │
│  - Master, PeerSearcher, ResourceMiner  │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         Core Layer (Framework)          │
│  - Process, Logger, Loader, Script      │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│      Data Layer (Storage/Network)       │
│  - Chain, Bunch, Status, Tracker        │
│  - TCP/UDP Socket, IPC                  │
└─────────────────────────────────────────┘
```

### 2. 노드 타입

**Full Ledger Node**
- 모든 블록체인 데이터 저장
- 용량 가득 차면 노드 중지
- 아카이브 노드 역할

**Partial Ledger Node** (기본)
- 부분 데이터만 유지
- 용량 가득 차면 오래된 데이터 삭제
- 경량 노드 운영 가능

### 3. 합의 메커니즘

**Resource-based Consensus**
- 리소스 마이닝을 통한 검증자 선출
- 9명의 검증자 (VALIDATOR_COUNT = 9)
- 60% 합의율 (MAIN_CONSENSUS_PER = 0.6)

**블록 생성 주기**
- Main Chain: 1초 (1,000,000 마이크로초)
- Resource Chain: 60초 (60,000,000 마이크로초)
- Mining Interval: 15초 (15,000,000 마이크로초)

---

## 🔑 핵심 기능

### 1. API 엔드포인트 (14개)

| API | 설명 |
|-----|------|
| `/block` | 블록 정보 조회 |
| `/broadcast` | 네트워크 브로드캐스트 |
| `/info` | 노드 정보 |
| `/main` | 메인 체인 정보 |
| `/peer` | 피어 목록 |
| `/ping` | 노드 상태 확인 |
| `/rawrequest` | Raw 요청 처리 |
| `/request` | 일반 요청 처리 |
| `/round` | 라운드 정보 |
| `/sendrawtransaction` | Raw 트랜잭션 전송 |
| `/sendtransaction` | 트랜잭션 전송 |
| `/status` | 상태 조회 |
| `/transaction` | 트랜잭션 조회 |
| `/weight` | 노드 가중치 |

### 2. CLI 스크립트 (26개)

**노드 관리**
```bash
php src/saseul-script start        # 노드 시작
php src/saseul-script stop         # 노드 중지
php src/saseul-script restart      # 노드 재시작
php src/saseul-script kill         # 강제 종료
php src/saseul-script log          # 로그 확인
```

**블록체인 초기화**
```bash
php src/saseul-script setenv       # 환경 설정
php src/saseul-script genesis      # 제네시스 블록 생성
php src/saseul-script genesisresource  # 제네시스 리소스 생성
```

**마이닝**
```bash
php src/saseul-script startmining  # 마이닝 시작
php src/saseul-script stopmining   # 마이닝 중지
```

**데이터 관리**
```bash
php src/saseul-script reset        # 데이터 리셋
php src/saseul-script resetdb      # DB 리셋
php src/saseul-script refine       # 데이터 정리
php src/saseul-script rebundling   # 번들링 재실행
php src/saseul-script forcesync    # 강제 동기화
```

**블록 관리**
```bash
php src/saseul-script restoreblock # 블록 복원
php src/saseul-script rewindblock  # 블록 되돌리기
```

**네트워크**
```bash
php src/saseul-script peer         # 피어 정보
php src/saseul-script addtracker   # 트래커 추가
php src/saseul-script resettracker # 트래커 리셋
```

### 3. 백그라운드 서비스 (6개)

**ChainMaker**
- 블록 생성 및 체인 관리
- 트랜잭션 번들링
- 합의 프로토콜 실행

**Collector**
- 네트워크 데이터 수집
- 피어 정보 수집

**DataPool**
- 트랜잭션 풀 관리
- 메모리 풀 운영
- IPC 통신 (포트 9934)

**Master**
- 마스터 노드 역할
- 서비스 조정
- IPC 통신 (포트 9933)

**PeerSearcher**
- 피어 검색 및 발견
- 네트워크 토폴로지 관리

**ResourceMiner**
- 리소스 마이닝 실행
- GPU 마이닝 연동
- 검증자 자격 획득

---

## ⚙️ 설정 (saseul.ini)

### 디렉토리 설정
```ini
[Directory]
log_file = "saseul.log"
err_file = "debug.log"
data_dir = "data"
```

### 노드 설정
```ini
[Node]
ledger_type = "partial"    # full | partial
database = false           # MySQL 사용 여부
mining = false             # 마이닝 활성화
```

### 네트워크 설정
```ini
[Network]
peers[] = "main.saseul.net"
peers[] = "aroma.saseul.net"
peers[] = "blanc.saseul.net"

network_name = "SASEUL PUBLIC NETWORK"
system_nonce = "Fiat lux. "
genesis_address = "c63e540b26715f490d763338f1b3f1f60990935f0837"
```

### 데이터베이스 설정 (선택)
```ini
[Database]
mysql_host = "localhost"
mysql_port = "3306"
mysql_user = ""
mysql_password = ""
mysql_database = ""
```

---

## 🔐 블록체인 상수 (Config.php)

### 체인 파라미터
```php
MAIN_CHAIN_INTERVAL = 1,000,000 μs (1초)
RESOURCE_INTERVAL = 60,000,000 μs (60초)
MINING_INTERVAL = 15,000,000 μs (15초)
```

### 합의 파라미터
```php
VALIDATOR_COUNT = 9
MAIN_CONSENSUS_PER = 0.6 (60%)
RESOURCE_CONFIRM_COUNT = 10
```

### 난이도 조정
```php
DIFFICULTY_CHANGE_CYCLE = 1440 블록
DEFAULT_DIFFICULTY = '100000'
MAX_DIFFICULTY_WEIGHT = 4
MIN_DIFFICULTY_WEIGHT = 0.25
```

### 블록 제한
```php
BLOCK_TX_SIZE_LIMIT = 16,777,216 bytes (16MB)
BLOCK_TX_COUNT_LIMIT = 2,048 트랜잭션
TX_SIZE_LIMIT = 1,048,576 bytes (1MB)
STATUS_SIZE_LIMIT = 65,536 bytes (64KB)
```

### 토큰 경제
```php
EXA = 10^18 (최소 단위)
STANDARD_AMOUNT = 2,000 * 10^18
CREDIT_AMOUNT = 60,000,000 * 10^18
```

---

## ⛏️ GPU 마이닝 (mine.cu)

### 빌드
```bash
./build.sh
# 또는
nvcc -o mine -O3 mine.cu -lssl -lcrypto
sudo cp mine /usr/bin/mine
```

### 구조
- **언어**: CUDA C++
- **해시**: SHA-256 (GPU 가속)
- **알고리즘**: Proof-of-Work
- **입력**: 블록 헤더 컴포넌트
  - Height (4 bytes)
  - Receipt Root (32 bytes)
  - Main Height (4 bytes)
  - Main Block Hash (39 bytes)
  - Validator Address (22 bytes)
  - Miner Address (22 bytes)

### 특징
- CUDA 병렬 처리
- OpenSSL 통합
- 멀티스레드 지원
- 실시간 해시레이트 모니터링

---

## 🚀 설치 및 실행

### 1. 초기 설정
```bash
# 설정 파일 복사
cp saseul.ini-sample saseul.ini

# 환경 설정
php src/saseul-script setenv

# 권한 설정
usermod -a -G www saseul
usermod -a -G www nginx
chown -Rf saseul:www data/
```

### 2. 설정 수정
```bash
vi saseul.ini
# genesis_address 수정
```

### 3. 노드 시작
```bash
# 노드 시작
php src/saseul-script start

# 로그 확인
php src/saseul-script log

# 제네시스 블록 생성
php src/saseul-script genesis
php src/saseul-script genesisresource
```

### 4. 마이닝 (선택)
```bash
# 마이너 빌드
./build.sh

# 마이닝 시작
php src/saseul-script startmining
```

---

## 📊 데이터 구조

### 파일 기반 저장소
```
data/
├── env                    # 환경 변수
├── peers                  # 피어 목록
├── known_hosts           # 알려진 호스트
├── chain_info            # 체인 정보
├── bunch/                # 트랜잭션 번들
├── main_chain/           # 메인 체인 블록
├── resource_chain/       # 리소스 체인 블록
└── status_bundle/        # 상태 번들
```

### 블록 구조
```
Block Header:
- height: uint32
- receiptRoot: bytes[32]
- mainHeight: uint32
- mainBlockHash: bytes[39]
- validator: bytes[22]
- miner: bytes[22]
```

---

## 🌐 네트워크 프로토콜

### IPC 포트
- **Master**: 127.0.0.1:9933
- **DataPool**: 127.0.0.1:9934

### 피어 통신
- **프로토콜**: TCP/UDP
- **기본 피어**:
  - main.saseul.net
  - aroma.saseul.net
  - blanc.saseul.net

### API 통신
- **프로토콜**: HTTP/REST
- **CORS**: 활성화
- **메모리**: 128MB 제한

---

## 🔄 v2.2.x 로드맵

### Partial Ledger 개선
- **Chain Slot**: 체인 슬롯 메커니즘
- **Peer Status Collection**: 피어 상태 수집
- **Data Pruning**: 부분 데이터 삭제 최적화

---

## 📝 주요 특징 요약

### ✅ 장점
1. **경량 노드 지원**: Partial Ledger로 저장 공간 절약
2. **GPU 마이닝**: CUDA 기반 고성능 마이닝
3. **모듈러 아키텍처**: 명확한 레이어 분리
4. **풍부한 CLI**: 26개의 관리 스크립트
5. **유연한 합의**: Resource-based Consensus
6. **IPC 통신**: 효율적인 프로세스 간 통신

### ⚠️ 주의사항
1. **실험적 소프트웨어**: 프로덕션 사용 주의
2. **상업적 사용 금지**: 라이선스 제한
3. **재시작 가능성**: 네트워크 재시작 가능
4. **PHP 의존성**: 특정 확장 필수

---

## 🛠️ 개발 도구

### 로깅
```bash
# 실시간 로그
php src/saseul-script log

# 로그 파일
tail -f saseul.log
tail -f debug.log
```

### 디버깅
```bash
# 노드 정보
php src/saseul-script info

# 피어 정보
php src/saseul-script peer

# 환경 확인
php src/saseul-script getenv
```

### 테스트
```bash
# 로컬 요청
php src/saseul-script localrequest

# 트랜잭션 전송
php src/saseul-script sendtransaction
```

---

## 📚 참고 정보

### 프로젝트 정보
- **저장소**: `/Users/sinseonghyeon/Documents/GitHub/01-blockchain-projects/maxpia-project/ssg`
- **문서**: `Readme.md`
- **설정 샘플**: `saseul.ini-sample`

### 주요 파일
- **API 진입점**: `src/index.php`
- **설정 관리**: `src/Saseul/Config.php`
- **클래스 로더**: `loader.json`
- **마이닝 프로그램**: `mine.cu`

### 빌드 스크립트
- **마이너 빌드**: `build.sh`
- **동기화**: `sync.sh`

---

## 🎯 사용 사례

### 1. 풀 노드 운영
```bash
# saseul.ini 설정
ledger_type = "full"
database = true
mining = false

# 노드 시작
php src/saseul-script start
```

### 2. 마이닝 노드
```bash
# saseul.ini 설정
ledger_type = "partial"
mining = true

# 마이너 빌드 및 시작
./build.sh
php src/saseul-script start
php src/saseul-script startmining
```

### 3. 경량 노드
```bash
# saseul.ini 설정
ledger_type = "partial"
database = false
mining = false

# 노드 시작
php src/saseul-script start
```

---

## 📞 지원

- **저작권**: ArtiFriends Inc. (2019-2023)
- **라이선스**: 상업적 사용 금지
- **경고**: 무단 사용 시 법적 책임 가능

---

**문서 생성일**: 2025-10-13  
**분석 도구**: Windsurf AI  
**프로젝트 버전**: SASEUL v2.1.9.6
