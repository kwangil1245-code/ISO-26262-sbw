# 요구사항 명세서 (System Requirements Specification)

**Document ID**: STEER-01-SRS  
**ISO 26262 Reference**: Part 4, Cl.6 (System Requirements Specification)  
**ASPICE Reference**: SYS.2 (System Requirements Analysis)  
**Version**: 1.0  
**Date**: 2026-08-22  
**Status**: Draft  
**Project Title**: AUTOSAR 기반 조향 관련 오류에 대한 복구 및 진단 시스템  
**Subtitle**: CAN 입력 감시, WdgM 실행 감시 및 FAIL-SAFE 기반 조향 출력 차단

---

## A. 통합 기본요구사항 (공식 표준 양식)

| Req. ID | 요약 | 설명 | 중요도/긴급도 | 요청자 | 검토자 | 변경사항 | Part |
|---|---|---|---|---|---|---|---|
| Req_001 | 조향 오류 진단 활성화 | 시스템은 조향 제어 기능이 활성화된 동안 조향 입력 및 내부 실행 상태를 감시해야 한다. | 상/상 |  |  |  | P0 |
| Req_002 | 시스템 안전 상태 정의 | 시스템은 진단 결과에 따라 `NORMAL` 또는 `FAIL-SAFE` 상태를 결정해야 한다. | 상/상 |  |  |  | P0 |
| Req_003 | 기능 분리 구조 | 시스템은 SteeringSensor, CanMonitor, SafetyPolicy, ControlCalc 및 Pwm_Actuator 기능을 독립 SWC로 구성해야 한다. | 상/중 |  |  |  | P0 |
| Req_004 | 조향각 입력 수집 | SWC_SteeringSensor는 가변저항으로부터 조향각 입력값을 수집해야 한다. | 상/중 |  |  |  | P1 |
| Req_005 | CAN 주기 송신 | SWC_SteeringSensor는 조향각과 Alive Counter를 포함한 CAN 메시지를 10 ms 주기로 송신해야 한다. | 상/상 |  |  |  | P1 |
| Req_006 | CAN 메시지 형식 | 조향 입력 CAN 메시지는 CAN ID `0x100`, DLC `3 Byte`로 구성해야 한다. | 상/상 |  |  |  | P1 |
| Req_007 | 조향각 신호 범위 | CAN 메시지의 조향각 신호는 `-512~511` 범위의 값을 표현해야 한다. | 상/상 |  |  |  | P1 |
| Req_008 | Alive Counter 갱신 | SWC_SteeringSensor는 CAN 메시지를 송신할 때마다 Alive Counter를 1씩 증가시켜야 한다. | 상/상 |  |  |  | P1 |
| Req_009 | RTE 수신 실패 검출 | SWC_CanMonitor는 조향각 또는 Alive Counter에 대한 RTE Read가 실패하면 입력 Fault를 생성해야 한다. | 상/상 |  |  |  | P2 |
| Req_010 | Alive Counter 이상 검출 | SWC_CanMonitor는 최초 정상 수신 이후 동일한 Alive Counter가 2회 이상 연속 수신되면 Timeout Fault를 생성해야 한다. | 상/상 |  |  |  | P2 |
| Req_011 | 조향각 Invalid 검출 | SWC_CanMonitor는 수신한 조향각이 `-512~511` 범위를 벗어나면 Invalid Fault를 생성해야 한다. | 상/상 |  |  |  | P2 |
| Req_012 | 입력 진단 결과 전달 | SWC_CanMonitor는 검증된 조향각과 입력 Fault Flag를 SWC_SafetyPolicy로 전달해야 한다. | 상/상 |  |  |  | P2 |
| Req_013 | WdgM 실행 상태 감시 | SWC_SafetyPolicy는 WdgM API를 이용하여 조향 제어 관련 SW 실행 상태를 확인해야 한다. | 상/상 |  |  |  | P3 |
| Req_014 | WdgM Fault 판정 | SWC_SafetyPolicy는 WdgM Global Status가 `FAILED`, `EXPIRED` 또는 `STOPPED`이면 내부 실행 Fault로 판정해야 한다. | 상/상 |  |  |  | P3 |
| Req_015 | FAIL-SAFE 전환 조건 | SWC_SafetyPolicy는 입력 Fault 또는 WdgM Fault 중 하나 이상이 발생하면 시스템을 `FAIL-SAFE` 상태로 전환해야 한다. | 상/상 |  |  |  | P3 |
| Req_016 | FAIL-SAFE 안전 출력 | `FAIL-SAFE` 상태에서 SWC_SafetyPolicy는 출력 조향각을 0으로 설정하고 출력 Fault Flag를 활성화해야 한다. | 상/상 |  |  |  | P3 |
| Req_017 | 정상 복귀 조건 | SWC_SafetyPolicy는 `FAIL-SAFE` 상태에서 Fault가 없는 정상 조건이 3회 연속 확인된 경우에만 `NORMAL` 상태로 복귀해야 한다. | 상/상 |  |  |  | P3 |
| Req_018 | 복귀 전 안전 상태 유지 | 정상 조건이 3회 연속 확인되기 전까지 시스템은 `FAIL-SAFE` 상태와 조향 출력 차단을 유지해야 한다. | 상/상 |  |  |  | P3 |
| Req_019 | 조향 변화량 계산 | SWC_ControlCalc는 현재 조향각과 이전 조향각의 차이를 이용하여 조향 변화량을 계산해야 한다. | 상/중 |  |  |  | P4 |
| Req_020 | 조향 방향 판정 | SWC_ControlCalc는 조향 변화량이 `+2`보다 크면 우측, `-2`보다 작으면 좌측으로 판정하고, 그 사이이면 정지로 판정해야 한다. | 상/상 |  |  |  | P4 |
| Req_021 | PWM 계산 입력 제한 | SWC_ControlCalc는 PWM Duty 계산에 사용하는 조향 변화량의 절댓값을 최대 512로 제한해야 한다. | 상/중 |  |  |  | P4 |
| Req_022 | PWM Duty 계산 | SWC_ControlCalc는 제한된 조향 변화량을 기반으로 PWM Duty를 계산하고 설정 가능한 최대 Duty 범위를 초과하지 않도록 제한해야 한다. | 상/상 |  |  |  | P4 |
| Req_023 | Fault 시 제어 출력 차단 | 입력 Fault Flag가 활성화되면 SWC_ControlCalc는 PWM Duty를 0으로 설정하고 좌·우 방향 및 구동 Flag를 모두 비활성화해야 한다. | 상/상 |  |  |  | P4 |
| Req_024 | IoHwAb 하드웨어 출력 | SWC_Pwm_Actuator는 PWM Duty, 좌·우 방향 및 구동 상태를 IoHwAb 포트를 통해 하드웨어로 출력해야 한다. | 상/상 |  |  |  | P5 |
| Req_025 | Actuator 정지 동작 | 구동 Flag가 비활성화되면 SWC_Pwm_Actuator는 PWM Duty를 0으로 설정하고 좌·우 출력을 비활성화하며 정지 LED를 활성화해야 한다. | 상/상 |  |  |  | P5 |
| Req_026 | SWC 인터페이스 연결 | 각 SWC는 Sender-Receiver Interface와 Client-Server Interface를 이용하여 입력부터 하드웨어 출력까지 데이터를 전달해야 한다. | 상/중 |  |  |  | P6 |
| Req_027 | Runnable Task 매핑 | 각 SWC의 Runnable Event는 설계된 Task에 매핑되어 입력 감시, 안전 판단, 제어 계산 및 출력 순서가 보장되어야 한다. | 상/상 |  |  |  | P6 |
| Req_028 | Fault 반응시간 | 시스템은 조향 입력 또는 내부 실행 Fault를 검출한 후 200 ms 이내에 PWM 출력을 안전 상태로 제한해야 한다. | 상/상 |  |  | HARA 연계 신규 | P7 |

## B. 요구사항 요약 블록

### Part 범례

| Part | 의미 |
|---|---|
| P0 | 시스템 공통 및 아키텍처 |
| P1 | 조향 입력 및 CAN 송신 |
| P2 | CAN 입력 감시 및 유효성 진단 |
| P3 | WdgM 감시 및 FAIL-SAFE 상태 관리 |
| P4 | 조향 방향 및 PWM 제어 계산 |
| P5 | IoHwAb 및 하드웨어 출력 |
| P6 | AUTOSAR SWC 통합 및 실행 구조 |
| P7 | 기능안전 Timing 요구사항 |

### 우선순위 요구사항 요약

| 우선순위 등급 | 기준(중요도/긴급도) | 개수 | 포함 Req. ID |
|---|---|---:|---|
| Critical (P0) | 상/상 | 23개 | Req_001, Req_002, Req_005~Req_018, Req_020, Req_022~Req_025, Req_027, Req_028 |
| High (P1) | 상/중 | 5개 | Req_003, Req_004, Req_019, Req_021, Req_026 |

> 위 표의 등급명 `P0/P1`은 요구사항 우선순위를 의미하며 Part 번호 `P0~P7`과는 별개의 분류이다.

### 안전 등급 요약 (HARA 기준)

| 안전 등급 | HARA ID | 관련 Req. ID |
|---|---|---|
| ASIL D (Provisional) | HC-01 | Req_005, Req_008~Req_010, Req_012, Req_015, Req_016, Req_023, Req_025, Req_028 |
| ASIL D (Provisional) | HC-02 | Req_007, Req_009, Req_011, Req_012, Req_015, Req_016, Req_023, Req_025, Req_028 |
| ASIL D (Provisional) | HC-03 | Req_013~Req_016, Req_023, Req_025, Req_028 |
| ASIL D (Provisional) | HC-04 | Req_015, Req_016, Req_023, Req_025, Req_028 |
| ASIL C (Provisional) | HC-05 | Req_017, Req_018 |
| ASIL D (Provisional) | HC-06 | Req_019~Req_025 |

> 안전 등급의 상세 판정과 Safety Goal은 [`governance/00d_HARA_Worksheet.md`](governance/00d_HARA_Worksheet.md)를 기준으로 관리한다.

## C. HARA-요구사항 추적성

| HARA ID | Safety Goal ID | 요구사항 범위 | 대표 검증 ID |
|---|---|---|---|
| HC-01 | SG-01 | Req_005, Req_008~Req_010, Req_012, Req_015, Req_016, Req_023, Req_025, Req_028 | `UT-CANMON-001`, `IT-CAN-001`, `ST-TIMEOUT-001` |
| HC-02 | SG-02 | Req_007, Req_009, Req_011, Req_012, Req_015, Req_016, Req_023, Req_025, Req_028 | `UT-CANMON-002`, `IT-CAN-002`, `ST-INVALID-001` |
| HC-03 | SG-03 | Req_013~Req_016, Req_023, Req_025, Req_028 | `UT-SAFETY-001`, `IT-WDGM-001`, `ST-WDGM-001` |
| HC-04 | SG-04 | Req_015, Req_016, Req_023, Req_025, Req_028 | `UT-SAFETY-002`, `IT-SAFETY-001`, `ST-FAILSAFE-001` |
| HC-05 | SG-05 | Req_017, Req_018 | `UT-SAFETY-003`, `IT-SAFETY-002`, `ST-RECOVERY-001` |
| HC-06 | SG-06 | Req_019~Req_025 | `UT-CONTROL-001`, `IT-ACTUATOR-001`, `ST-PWM-001` |

---

본 문서의 요구사항은 결과보고서의 구현 코드와 구성 내용을 기준으로 작성하였다. `Req_028`의 200 ms Fault 반응시간은 HARA에서 도출된 검증 대상이며, 실제 측정 결과가 확보되기 전까지 설계 목표로 관리한다.
