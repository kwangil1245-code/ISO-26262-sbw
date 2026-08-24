# 소프트웨어 요구사항 검증 명세서·결과서

**Document ID**: STEER-07-SWQT  
**ISO 26262 Reference**: Part 6, Cl.11  
**ASPICE Reference**: SWE.6 (Software Qualification Test)  
**Version**: 1.0  
**Date**: 2026-08-24  
**Status**: Completed  
**Project Title**: AUTOSAR 기반 조향 관련 오류에 대한 복구 및 진단 시스템

---

## 1. 문서 목적

본 문서는 통합된 ECU 소프트웨어가 `03_SW_Requirements.md`의 `SWR-*`를 충족하는지 블랙박스 관점에서 검증한 결과를 기록한다.

CANoe/CAPL은 ECU 소프트웨어 외부의 조향 CAN 메시지와 Fault 조건을 생성하고 상태·출력 신호를 관측하는 시험 도구로 사용한다. 시험 대상은 통합된 소프트웨어이며, 실제 모터 토크·PWM 파형·배선·CAN Transceiver와 같은 물리 시스템 특성은 후속 시스템검증 범위로 분리한다.

## 2. SWE.5와 SWE.6의 구분

| 구분 | SWE.5 통합시험 | SWE.6 소프트웨어 요구사항 검증 |
|---|---|---|
| 검증 기준 | SWC, Interface, Runnable 구조 | `SWR-*` 요구사항 |
| 관점 | 구성요소 사이의 연결 | 통합 SW의 외부 동작 |
| 입력 | Stub/Mock 및 직접 Runnable 호출 | CANoe/CAPL, Test Hook 및 ECU Interface |
| 내부 관측 | RTE Buffer와 Mock 호출 | CAN 신호, SW 상태, Fault 및 출력 명령 |
| 주요 질문 | SWC가 올바르게 연결됐는가 | 통합 SW가 요구 동작을 수행하는가 |

## 3. 시험 대상과 환경

| 항목 | 구성 |
|---|---|
| 시험 대상 | 입력 ECU 및 출력 ECU의 통합 Software Build |
| 외부 자극 | CANoe/CAPL 기반 조향 메시지 및 Alive Counter 생성 |
| 내부 Fault 주입 | CodeWarrior 또는 시험용 Hook을 통한 WdgM 상태 설정 |
| 입력 ECU 확인 | 조향 입력 Test Hook 또는 제어 입력과 CAN Trace 비교 |
| 관측 정보 | 조향값, Alive Counter, Fault Flag, NORMAL/FAIL-SAFE 상태, PWM 명령, 방향 명령 |
| 물리 출력 제외 | 실제 PWM 파형, Pin 전압, LED, 모터 회전 및 토크 |
| 판정 | 실제 SW 출력이 기대 결과와 일치하면 PASS |

```mermaid
flowchart LR
    C["CANoe·CAPL/Test Hook"] --> E["통합 ECU Software"]
    E --> O["CAN·상태·출력 명령 관측"]
    O --> V["SWR 기준 판정"]
```

## 4. CAPL 시험 자극 원칙

| 자극 유형 | CAPL 동작 | 검증 목적 |
|---|---|---|
| 정상 메시지 | 유효 조향값과 증가하는 Alive Counter 송신 | 정상 수신·진단·제어 확인 |
| Counter 고정 | 동일 Alive Counter를 반복 송신 | 갱신 이상과 Timeout Fault 확인 |
| Invalid 값 | 정상 범위를 벗어난 조향값 송신 | 입력 유효성 Fault 확인 |
| 정상 복귀 | Fault 해제 후 정상 메시지 연속 송신 | FAIL-SAFE 유지와 NORMAL 복귀 확인 |
| Fault 재발 | 복귀 확인 중 비정상 메시지 재송신 | 복귀 중단 확인 |

> 현재 구현의 통신 갱신 진단은 Data Received Event에서 Alive Counter를 비교한다. 따라서 본 시험에서는 메시지를 완전히 중단하는 방식이 아니라 동일 Counter를 반복 송신하여 갱신 이상 경로를 실행한다.

## 5. SW Qualification Test Case

### 5.1 입력 및 통신

| SVT ID | 시험 조건·절차 | 기대 결과 | 추적 SW 요구사항 | 결과 |
|---|---|---|---|---|
| SVT-COM-001 | 입력 ECU에 최소·중간·최대 조향 입력을 순차 적용하고 CAN Trace 확인 | 대응하는 조향 정보가 생성되어 제공됨 | SWR-IN-001 | PASS |
| SVT-COM-002 | 정상 조향값과 Alive Counter를 사용하여 조향 메시지 주기 송신 | 출력 ECU가 조향 정보와 갱신 정보를 정상 수신함 | SWR-COM-001, SWR-COM-002 | PASS |
| SVT-COM-003 | CAPL에서 Alive Counter를 메시지마다 증가시켜 연속 송신 | 통신 Fault가 설정되지 않고 조향 정보가 제어에 사용됨 | SWR-COM-001, SWR-DIAG-001 | PASS |

### 5.2 통신·입력·실행 진단

| SVT ID | 시험 조건·절차 | 기대 결과 | 추적 SW 요구사항 | 결과 |
|---|---|---|---|---|
| SVT-DIAG-001 | CAPL에서 동일 Alive Counter를 진단 기준까지 반복 송신 | 갱신 이상이 감지되고 통신 Fault가 설정됨 | SWR-DIAG-001, SWR-DIAG-003 | PASS |
| SVT-DIAG-002 | 정상 범위의 하한·상한 조향값 송신 | Invalid Fault가 설정되지 않음 | SWR-DIAG-002 | PASS |
| SVT-DIAG-003 | 정상 범위 미만과 초과 조향값 송신 | Invalid Fault가 설정되고 안전 판단에 전달됨 | SWR-DIAG-002, SWR-DIAG-003 | PASS |
| SVT-WDG-001 | 시험용 Hook으로 WdgM 상태를 OK로 설정 | 내부 실행 Fault가 설정되지 않음 | SWR-WDG-001, SWR-WDG-002 | PASS |
| SVT-WDG-002 | 시험용 Hook으로 WdgM 상태를 FAILED, EXPIRED, STOPPED로 각각 설정 | 각 조건에서 내부 실행 Fault가 설정되고 안전 판단에 반영됨 | SWR-WDG-001, SWR-WDG-002 | PASS |

### 5.3 안전 상태 전환·유지·복귀

| SVT ID | 시험 조건·절차 | 기대 결과 | 추적 SW 요구사항 | 결과 |
|---|---|---|---|---|
| SVT-SAFE-001 | Timeout, Invalid 및 WdgM Fault를 각각 발생 | 각 Fault에서 NORMAL에서 FAIL-SAFE로 전환 | SWR-SAFE-001 | PASS |
| SVT-SAFE-002 | FAIL-SAFE 상태에서 조향 입력을 변화 | 조향값이 안전값으로 제한되고 출력 명령이 비활성화됨 | SWR-SAFE-002 | PASS |
| SVT-SAFE-003 | Fault 조건을 여러 처리 Cycle 동안 유지 | FAIL-SAFE와 안전 출력이 계속 유지됨 | SWR-SAFE-003 | PASS |
| SVT-SAFE-004 | Fault 해제 후 정상 메시지를 복귀 기준만큼 연속 송신 | 기준 충족 전 FAIL-SAFE 유지, 기준 충족 후 NORMAL 복귀 | SWR-SAFE-004 | PASS |
| SVT-SAFE-005 | 정상 복귀 확인 중 동일 Counter 또는 Invalid 값을 재주입 | 정상 복귀가 중단되고 FAIL-SAFE가 유지됨 | SWR-SAFE-005 | PASS |

### 5.4 제어·출력·모니터링

| SVT ID | 시험 조건·절차 | 기대 결과 | 추적 SW 요구사항 | 결과 |
|---|---|---|---|---|
| SVT-CTRL-001 | NORMAL 상태에서 조향값을 증가·감소 | 조향 변화에 따라 좌·우 방향과 출력 크기가 계산됨 | SWR-CTRL-001 | PASS |
| SVT-CTRL-002 | 조향 변화량을 정지 조건 이내로 입력 | 정지 상태와 출력 비활성 명령 생성 | SWR-CTRL-002 | PASS |
| SVT-ACT-001 | 정상 방향·PWM 계산 결과 생성 | 계산 결과가 Actuator Interface의 방향·PWM 명령으로 제공됨 | SWR-ACT-001 | PASS |
| SVT-ACT-002 | FAIL-SAFE 또는 정지 상태 생성 | PWM과 방향 출력 명령이 비활성화됨 | SWR-ACT-002 | PASS |
| SVT-MON-001 | NORMAL, Timeout, Invalid, WdgM Fault 및 복귀 조건을 순차 실행 | 시스템 상태·Fault 종류·출력 결과가 구분되어 관측됨 | SWR-MON-001, SWR-MON-002, SWR-MON-003 | PASS |

## 6. SW 요구사항 추적성

| SW 요구사항 | 검증 Test Case |
|---|---|
| SWR-IN-001 | SVT-COM-001 |
| SWR-COM-001 | SVT-COM-002, SVT-COM-003 |
| SWR-COM-002 | SVT-COM-002 |
| SWR-DIAG-001 | SVT-COM-003, SVT-DIAG-001 |
| SWR-DIAG-002 | SVT-DIAG-002, SVT-DIAG-003 |
| SWR-DIAG-003 | SVT-DIAG-001, SVT-DIAG-003 |
| SWR-WDG-001 | SVT-WDG-001, SVT-WDG-002 |
| SWR-WDG-002 | SVT-WDG-001, SVT-WDG-002 |
| SWR-SAFE-001 | SVT-SAFE-001 |
| SWR-SAFE-002 | SVT-SAFE-002 |
| SWR-SAFE-003 | SVT-SAFE-003 |
| SWR-SAFE-004 | SVT-SAFE-004 |
| SWR-SAFE-005 | SVT-SAFE-005 |
| SWR-CTRL-001 | SVT-CTRL-001 |
| SWR-CTRL-002 | SVT-CTRL-002 |
| SWR-ACT-001 | SVT-ACT-001 |
| SWR-ACT-002 | SVT-ACT-002 |
| SWR-MON-001 | SVT-MON-001 |
| SWR-MON-002 | SVT-MON-001 |
| SWR-MON-003 | SVT-MON-001 |

## 7. 시험 결과 요약

| 시험 그룹 | 전체 | PASS | FAIL | BLOCKED |
|---|---:|---:|---:|---:|
| 입력·통신 | 3 | 3 | 0 | 0 |
| 통신·입력·실행 진단 | 5 | 5 | 0 | 0 |
| 안전 상태 전환·유지·복귀 | 5 | 5 | 0 | 0 |
| 제어·출력·모니터링 | 5 | 5 | 0 | 0 |
| 합계 | 18 | 18 | 0 | 0 |

### 수행 결과

- 정상 조향 메시지의 수신·진단·제어 경로가 정상 동작하였다.
- 동일 Alive Counter와 범위 밖 조향값을 통해 Timeout 및 Invalid Fault를 확인하였다.
- WdgM 상태 주입 시 내부 실행 Fault와 FAIL-SAFE 전환을 확인하였다.
- FAIL-SAFE 상태에서 출력 명령이 제한되고 정상 조건 충족 시 NORMAL로 복귀하였다.
- 20개 SW 요구사항이 하나 이상의 SWE.6 Test Case에 연결되었다.

> CAPL Source, CANoe Configuration, CAN Trace, 상태값 캡처 및 시험용 Hook 설정은 각 `SVT-*` ID와 함께 시험 증적으로 관리한다.

## 8. 시스템검증으로 이관되는 항목

| 검증 항목 | SWE.6 제외 이유 |
|---|---|
| 입력 ECU와 출력 ECU의 실제 물리 CAN 연결 | Network HW와 Transceiver를 포함함 |
| 실제 10 ms 송신 주기 정밀 측정 | ECU OS와 실제 Network Timing을 포함함 |
| PWM Pin Duty와 주파수 측정 | MCAL·IoHwAb·Pin·계측기를 포함함 |
| 방향 Pin 및 LED 전압 확인 | 실제 HW 출력 경로를 포함함 |
| 모터 좌·우 회전과 정지 | Actuator와 전원·배선을 포함함 |

## 9. 완료 기준

- 모든 `SWR-*`가 하나 이상의 `SVT-*`에 연결되어야 한다.
- 모든 Test Case가 PASS이거나 승인된 편차와 연결되어야 한다.
- Software Build, CAPL 및 CANoe 설정 버전이 시험 결과와 연결되어야 한다.
- SW 요구사항 변경 시 영향받는 Test Case를 재수행해야 한다.
- 시스템검증 입력으로 통합 Software Build와 미검증 HW 항목을 전달해야 한다.

---

본 문서는 CANoe/CAPL을 사용한 SWE.6 소프트웨어 요구사항 검증의 기준 산출물이다. 실제 ECU 간 통신과 물리 출력은 후속 시스템검증에서 확인한다.
