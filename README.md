# SystemVerilog Verification: ASCII Sender·FIFO·UART TX Testbench

> SystemVerilog OOP 기반 계층형 검증 환경 구축 및 100% 기능 커버리지 달성

## 📌 프로젝트 개요

SystemVerilog OOP를 활용해 UART/FIFO 모듈의 계층형 검증 환경을 설계하고 구현했습니다. 제약 조건 기반 랜덤화와 Self-checking Scoreboard를 구축하여 검증 자동화를 실현했으며, 시뮬레이션 중 발생한 레이스 컨디션을 샘플링 시점 최적화로 해결했습니다. 최종적으로 100% 기능 커버리지를 달성했습니다.

**개발 기간:** 2026.02.23 ~ 2026.03.03  
**팀 구성:** 3인 팀 프로젝트  
**담당 업무:** UART_TX, FIFO, ASCII_Sender Verification

## 🛠 기술 스택

- **HDL:** Verilog, SystemVerilog
- **FPGA 툴:** Xilinx Vivado

## 🏗 검증 환경 구조

### 계층형 테스트벤치
- **Transaction:** 검증 데이터를 캡슐화한 객체
- **Generator:** 제약 조건 기반 랜덤 테스트 생성
- **Driver:** DUT 입력 신호 구동
- **Monitor:** DUT 출력 신호 샘플링
- **Scoreboard:** Golden Model 기반 자동 검증

### 검증 대상 모듈
- **ASCII_Sender:** ASCII 명령어 디코더
- **FIFO:** 데이터 버퍼링 및 흐름 제어
- **UART_TX:** 직렬 통신 송신기

### 제약 조건 기반 랜덤화
- **Constrained Randomization:** 다양한 테스트 시나리오 자동 생성
- **Random Seed:** 100회 이상 반복 테스트로 재현성 확보
- **경계 조건 테스트:** FIFO Full/Empty, UART 글리치 등

### 커버리지 기반 검증
- **Functional Coverage:** Covergroup, Cross Coverage 측정
- **100% 커버리지 달성:** 모든 시나리오 검증 완료
- **데이터 무결성:** 전체 경로에서 데이터 유실 없음 확인

## 📊 검증 결과

- ✅ ASCII 명령어 → FIFO → UART TX 전 구간 데이터 무결성 확인
- ✅ FIFO Full/Empty 경계 조건 100% 커버리지 달성
- ✅ UART 글리치 주입 시나리오 정상 처리
- ✅ 100회 이상 Random Seed 반복 테스트 통과

## 🔧 Troubleshooting

### Race Condition 문제

**문제:**  
FIFO 검증 중 Generator의 예측값과 Monitor의 실제값이 일치하지 않아 다수의 FAIL 발생

**원인:**  
Driver의 신호 업데이트와 Monitor의 샘플링 시점이 모두 posedge로 일치하여 데이터 안정성이 깨지는 레이스 컨디션 발생

**해결:**  
- Driver 측에 `#1` 지연을 추가하여 Hold Time 마진 확보
- Monitor가 negedge에서 샘플링하도록 수정하여 안정적 데이터 획득

**결과:** ✅ 레이스 컨디션 완전 해소, 100% 커버리지 달성

## 📚 배운 점

- **계층형 검증 환경 구축:** Transaction → Generator → Driver → Monitor → Scoreboard 구조 설계
- **검증 자동화:** Self-checking Scoreboard로 Pass/Fail 자동 판정
- **타이밍 이슈 디버깅:** 레이스 컨디션 원인 분석 및 샘플링 최적화
