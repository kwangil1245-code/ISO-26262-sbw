이거 내가 수정하는방법없나

직접 수정할 수 있습니다. 지금 화면 오른쪽 위에 있는 연필 모양 버튼을 누르면 됩니다.

오른쪽 위의 연필 아이콘(Edit this file) 클릭
Preview 화면이 편집 화면으로 변경됩니다.
Markdown 내용을 직접 수정합니다.
수정이 끝나면 오른쪽 위 또는 페이지 아래의 Commit changes… 클릭
변경 설명 입력
예: Update project overview
저장 방식 선택
Commit directly to the main branch: 바로 반영
Create a new branch and start a pull request: 별도 브랜치에서 검토 후 반영
Commit changes를 누르면 저장됩니다.

간단한 문구 수정이라면 main에 바로 커밋해도 됩니다. 여러 문서의 요구사항이나 추적성을 함께 바꾸는 작업이라면 새 브랜치와 PR을 사용하는 것이 안전합니다.

참고로 현재 파일은 .md 형식입니다. 편집 화면에서 다음처럼 작성하면 제목과 표로 표시됩니다.

# 프로젝트 개요

## 1. 문서 목적

본 문서는 조향 오류 복구 및 진단 시스템의 프로젝트 개요를 정의한다.

| 항목 | 내용 |
|---|---|
| 개발 목적 | 조향 입력 오류를 감지하고 안전 상태로 전환한다. |
| 시스템 범위 | CAN 통신, 오류 진단, Fail-Safe 제어 |

편집 중 결과를 확인하려면 상단의 Preview 탭을 누르면 됩니다. 현재 화면의 연필 옆 ▼ 버튼이 아니라 연필 자체를 클릭하면 됩니다.

새로 md하나 추가하는 방법은 없나

가능합니다. GitHub 웹 화면에서 새 .md 파일을 만들 수 있습니다.

새 파일을 넣을 폴더로 이동합니다.
예: driving-alert-workproducts
오른쪽 위 Add file 버튼을 누릅니다.
Create new file을 선택합니다.
위쪽 파일 이름 칸에 다음처럼 입력합니다.
새문서이름.md

예를 들어 HARA 문서를 만들려면:

00d_HARA_Worksheet.md
아래 편집창에 Markdown 내용을 작성합니다.
# HARA Worksheet

## 1. 문서 목적

본 문서는 조향 오류 복구 및 진단 시스템의 위험원을 분석하고
ASIL 및 Safety Goal을 도출하기 위해 작성한다.
우측 상단의 **Commit changes…**를 누릅니다.
커밋 메시지를 입력합니다.
Add HARA worksheet
Commit directly to the main branch를 선택하고 Commit changes를 누르면 추가됩니다.
새 폴더와 파일을 동시에 만드는 방법

GitHub에서는 빈 폴더만 따로 만들 수 없기 때문에 파일 이름 칸에 경로까지 입력하면 됩니다.

governance/00d_HARA_Worksheet.md

이렇게 입력하면 한 번에 다음 구조가 생성됩니다.

driving-alert-workproducts/
└─ governance/
   └─ 00d_HARA_Worksheet.md

즉, driving-alert-workproducts 폴더에 들어간 상태에서:

governance/00d_HARA_Worksheet.md

라고 입력하면 됩니다.

붙여넣은 마크다운(1).md
파일
이내용으로 채워줘봐

첨부 내용을 driving-alert-workproducts/governance/00d_HARA_Worksheet.md로 생성하려 했지만, GitHub 연동 권한이 읽기 전용이라 403 오류가 발생했습니다. 파일은 아직 추가되지 않았습니다.

직접 추가하려면:

GitHub에서 driving-alert-workproducts 폴더로 이동
Add file → Create new file
파일 이름에 아래 경로 입력
governance/00d_HARA_Worksheet.md
첨부한 Markdown 내용을 편집창에 붙여넣기
Commit changes…
커밋 메시지 입력
Add steering HARA worksheet
Commit directly to the main branch → Commit changes

다만 첨부 문서의 표 제목 일부가 붙어 있어 그대로 넣으면 표가 깨집니다. 예를 들어:

| 항목레벨의미 |

는 다음처럼 고쳐야 합니다.

| 항목 | 레벨 | 의미 |
|---|---|---|

GitHub 연동 설정에서 저장소 콘텐츠 쓰기 권한을 허용하면 다음부터는 제가 직접 파일 생성과 수정까지 할 수 있습니다.

HARA 워크시트
Hazard Analysis and Risk Assessment

Document ID: STEER-00D-HARA
ISO 26262 Reference: Part 3 – Concept Phase, Hazard Analysis and Risk Assessment
ASPICE Reference: SYS.2, SYS.3, SUP.10
Version: 1.0
Status: Draft
Project Title: AUTOSAR 기반 조향 관련 오류에 대한 복구 및 진단 시스템

1. 목적 및 범위

본 문서는 AUTOSAR 기반 조향 오류 복구 및 진단 시스템에서 발생 가능한 위험 상황을 식별하고, 각 Hazardous Event에 대해 Severity(S), Exposure(E), Controllability(C)를 평가하여 Safety Goal을 도출하는 것을 목적으로 한다.

대상 시스템은 다음 데이터 흐름을 기준으로 한다.

조향 입력 → SteeringSensor → CAN → CanMonitor → SafetyPolicy → ControlCalc → PWM Actuator

주요 분석 대상은 다음과 같다.

CAN 메시지 미수신에 따른 조향 입력 Timeout
조향각 허용 범위를 벗어난 Invalid 입력
Alive Counter 이상
WdgM 기반 내부 실행 이상
FAIL-SAFE 상태 전환 실패
FAIL-SAFE 상태에서의 부적절한 복귀
PWM 또는 조향 방향 출력 이상

본 HARA는 교육 프로젝트를 위한 내부 분석 기준이며 실제 양산 차량에 대한 공식 ISO 26262 ASIL 판정 결과를 의미하지 않는다.

2. S/E/C 평가 기준
항목레벨의미	
	

Severity	S0~S3	위험 발생 시 운전자 및 탑승자에게 발생할 수 있는 피해의 심각도
Exposure	E0~E4	해당 운행 상황에 노출될 가능성 또는 빈도
Controllability	C0~C3	위험 발생 후 운전자가 차량을 통제할 수 있는 정도
운영 기준
S/E/C 평가는 조향 기능이 실제 차량에 적용되었다는 가정하에 수행한다.
ASIL Candidate는 프로젝트 내부 안전 설계 우선순위를 결정하기 위한 값으로 사용한다.
실제 양산 적용 시 차량 수준 Item Definition 및 상세 운행 시나리오를 바탕으로 재평가해야 한다.
FTTI는 현재 프로젝트에서 정식 산출되지 않았으므로 초기 설계 목표값으로 관리하고 추후 검증 결과를 통해 조정한다.
3. HARA 상세 워크시트
HARA ID관련 ReqHazardous EventOperational SituationSECASIL CandidateSafety Goal IDSafety GoalFTTI / Timing 가정	
	
	
	
	
	
	
	
	
	

HC-01	SR-STEER-001, SR-STEER-003	CAN 메시지 미수신을 감지하지 못해 이전 조향 명령이 계속 사용됨	주행 중 입력 ECU 또는 CAN 통신 이상으로 조향 데이터 갱신 중단	S3	E4	C3	D	SG-01	조향 CAN 데이터가 정상적으로 갱신되지 않는 경우 Timeout을 검출하고 조향 출력을 안전 상태로 전환해야 한다.	200 ms 이내 검출 및 출력 제한
HC-02	SR-STEER-002, SR-STEER-003	비정상 조향각이 정상 입력으로 처리되어 잘못된 조향 출력 발생	주행 중 센서 오류, 센서 단선 또는 비정상 조향 데이터 발생	S3	E4	C3	D	SG-02	허용 범위를 벗어난 조향 입력은 Invalid로 판정하고 해당 입력을 조향 출력에 사용하지 않아야 한다.	입력 수신 후 100 ms 이내
HC-03	SR-STEER-004	SW Runnable 실행 이상을 감지하지 못해 오래된 데이터 또는 비정상 제어 출력 지속	ECU 내부 Task 또는 Runnable 실행 이상 발생	S3	E3	C3	D	SG-03	조향 제어 관련 SW 실행 이상을 WdgM으로 감지하고 이상 발생 시 FAIL-SAFE 상태로 전환해야 한다.	200 ms 이내
HC-04	SR-STEER-005, SR-STEER-006	Timeout/Invalid 또는 내부 실행 이상이 발생했음에도 FAIL-SAFE 전환이 수행되지 않아 조향 출력 지속	차량 주행 중 입력 또는 ECU 내부 Fault 발생	S3	E4	C3	D	SG-04	조향 입력 또는 내부 실행 Fault 발생 시 NORMAL 상태에서 FAIL-SAFE 상태로 전환하고 PWM 출력을 제한해야 한다.	Fault 판단 후 100 ms 이내
HC-05	SR-STEER-007	일시적인 정상 입력만으로 FAIL-SAFE가 해제되어 비정상 상태에서 조향 출력이 재활성화됨	간헐적 CAN 장애 또는 센서 오류 후 일시적으로 정상 데이터 수신	S3	E3	C2	C	SG-05	FAIL-SAFE 상태는 연속 정상 조건이 확인된 경우에만 NORMAL 상태로 복귀해야 한다.	정상 입력 3회 연속 확인 후 복귀
HC-06	SR-STEER-008, SR-STEER-009	PWM Duty 또는 좌·우 방향 출력 오류로 운전자 의도와 다른 조향 출력 발생	정상 조향 입력 중 ControlCalc 또는 Actuator 출력 이상	S3	E4	C3	D	SG-06	조향 방향 및 PWM 출력은 검증된 조향 입력과 SafetyPolicy 상태를 기반으로 생성되어야 하며, Fault 상태에서는 PWM 출력을 차단해야 한다.	제어 Cycle 내 적용
4. Safety Goal 상세 정의
SG-01 – CAN Timeout 대응

조향 CAN 메시지가 정상적으로 갱신되지 않는 경우 시스템은 Alive Counter를 이용해 Timeout 상태를 감지하고, Timeout이 확인되면 해당 조향 값을 제어에 사용하지 않아야 한다.

SG-02 – Invalid 조향 입력 차단

수신한 조향각 값이 정의된 유효 범위를 벗어난 경우 해당 입력을 Invalid로 판정하고 SafetyPolicy에 Fault 상태를 전달해야 한다.

SG-03 – ECU 내부 실행 이상 감지

조향 제어와 관련된 Runnable 또는 Task의 실행 이상은 WdgM을 통해 감지되어야 하며 실행 이상 발생 시 정상 조향 제어를 지속해서는 안 된다.

SG-04 – FAIL-SAFE 전환

Timeout, Invalid 또는 WdgM Fault 중 하나 이상의 이상 조건이 발생할 경우 시스템 상태를 NORMAL에서 FAIL-SAFE로 전환하고 조향 PWM 출력을 제한하거나 차단해야 한다.

SG-05 – 안전한 정상 상태 복귀

FAIL-SAFE 상태에서 단일 정상 입력만으로 NORMAL 상태로 복귀해서는 안 되며, 연속 3회의 정상 조건을 확인한 후 NORMAL 상태로 전환해야 한다.

SG-06 – 안전한 Actuator 출력

ControlCalc 및 Pwm_Actuator는 SafetyPolicy가 정상 상태임을 확인한 경우에만 조향 입력을 기반으로 PWM Duty 및 방향 신호를 생성해야 한다. FAIL-SAFE 상태에서는 PWM Duty를 0으로 설정하고 조향 출력을 차단해야 한다.

5. Safety Goal → 검증 링크
Safety Goal IDVerification CaseUnit TestIntegration TestSystem Test주요 검증 내용	
	
	
	
	

SG-01	VC-STEER-001	UT-CANMON-001	IT-CAN-001	ST-TIMEOUT-001	CAN 데이터 갱신 중단 시 Timeout 검출 및 FAIL-SAFE 전환
SG-02	VC-STEER-002	UT-CANMON-002	IT-CAN-002	ST-INVALID-001	조향각 허용 범위 초과 입력 감지
SG-03	VC-STEER-003	UT-SAFETY-001	IT-WDGM-001	ST-WDGM-001	Runnable 실행 이상 시 WdgM Fault 검출
SG-04	VC-STEER-004	UT-SAFETY-002	IT-SAFETY-001	ST-FAILSAFE-001	Fault 발생 시 NORMAL → FAIL-SAFE 전환 및 PWM 차단
SG-05	VC-STEER-005	UT-SAFETY-003	IT-SAFETY-002	ST-RECOVERY-001	FAIL-SAFE 상태에서 정상 입력 3회 확인 후 NORMAL 복귀
SG-06	VC-STEER-006	UT-CONTROL-001	IT-ACTUATOR-001	ST-PWM-001	정상 상태에서 좌/우 PWM 출력, Fault 상태에서 PWM 0 확인
6. 주요 검증 시나리오
ST-TIMEOUT-001 – CAN Timeout

Precondition

시스템 상태 NORMAL
CAN 메시지 정상 수신
Alive Counter 정상 증가

Fault Injection

CAN 메시지 또는 Alive Counter 갱신을 중단한다.

Expected Result

CanMonitor가 Timeout을 검출한다.
Fault Flag가 SafetyPolicy로 전달된다.
SafetyPolicy가 FAIL-SAFE 상태로 전환한다.
ControlCalc가 PWM Duty를 0으로 설정한다.
Actuator의 조향 출력이 차단된다.
ST-INVALID-001 – Invalid Steering Angle

Precondition

시스템 상태 NORMAL

Fault Injection

정의된 조향각 허용 범위를 벗어난 값을 CAN 메시지로 입력한다.

Expected Result

CanMonitor가 Invalid 상태를 검출한다.
Fault Flag가 SafetyPolicy에 전달된다.
FAIL-SAFE 상태로 전환된다.
비정상 조향 값이 Actuator 출력으로 전달되지 않는다.
ST-WDGM-001 – 내부 실행 이상

Fault Injection

조향 제어 Runnable의 정상 실행 조건을 위반한다.

Expected Result

WdgM이 Supervised Entity 이상을 검출한다.
SafetyPolicy가 WdgM 상태를 Fault로 판단한다.
FAIL-SAFE 상태로 전환한다.
PWM 출력이 차단된다.
ST-RECOVERY-001 – FAIL-SAFE Recovery

Precondition

시스템 상태 FAIL-SAFE

Input

정상 CAN 메시지를 연속 입력한다.

Expected Result

정상 입력 1회 → FAIL-SAFE 유지
정상 입력 2회 → FAIL-SAFE 유지
정상 입력 3회 → NORMAL 복귀
정상 복귀 후 조향 출력 재활성화
7. 요구사항 추적성
Requirement ID요구사항관련 HARASafety Goal	
	
	

SR-STEER-001	시스템은 Alive Counter를 이용해 CAN 메시지 갱신 상태를 확인해야 한다.	HC-01	SG-01
SR-STEER-002	시스템은 조향각 값의 유효 범위를 검사해야 한다.	HC-02	SG-02
SR-STEER-003	Timeout 또는 Invalid 발생 시 Fault Flag를 생성해야 한다.	HC-01, HC-02	SG-01, SG-02
SR-STEER-004	시스템은 WdgM을 이용해 내부 실행 상태를 감시해야 한다.	HC-03	SG-03
SR-STEER-005	입력 또는 실행 이상 발생 시 FAIL-SAFE 상태로 전환해야 한다.	HC-04	SG-04
SR-STEER-006	FAIL-SAFE 상태에서는 조향 PWM 출력을 제한해야 한다.	HC-04	SG-04
SR-STEER-007	FAIL-SAFE 상태에서 정상 조건이 연속 3회 확인된 경우에만 NORMAL 상태로 복귀해야 한다.	HC-05	SG-05
SR-STEER-008	정상 상태에서 조향 값에 따라 PWM Duty와 조향 방향을 계산해야 한다.	HC-06	SG-06
SR-STEER-009	Fault 상태에서는 PWM Duty를 0으로 설정해야 한다.	HC-06	SG-06
8. 현재 판단 및 남은 작업
항목현재 상태다음 조치	
	

Hazardous Event 정의	Draft 완료	운행 시나리오 기준 검토
S/E/C 평가	Candidate 정의	평가 근거 상세 작성
ASIL Candidate	Draft	HARA Review 후 확정
Safety Goal	SG-01~SG-06 정의	FSR과 연결
FTTI	초기 가정값 적용	실제 실행시간 및 Fault Detection Time 측정
Unit Test	계획	SWC별 Test Case 작성
Integration Test	계획	SWC Interface 및 데이터 흐름 검증
System Test	일부 수행	Fault Injection 기반 FAIL-SAFE 검증 추가
Traceability	기본 구조 작성	Requirement–Design–Test 양방향 링크 구성
9. HARA 승인 게이트
항목담당승인 기준상태	
	
	

Hazardous Event 검토	System/Safety 담당	HC-01~HC-06 위험 시나리오 타당성 검토	Draft
S/E/C 검토	Safety 담당	각 평가값에 대한 근거 확보	Draft
ASIL Candidate 검토	Safety 담당	S/E/C와 ASIL 분류 일치 확인	Draft
Safety Goal 검토	System 담당	모든 주요 Hazard가 SG에 의해 커버되는지 확인	Draft
FTTI 검토	SW/System 담당	Fault Detection + Reaction Time이 목표 시간 내인지 확인	Planned
검증 링크 검토	Validation 담당	SG → VC → Test Case 추적성 확인	Planned
내부 승인 기준

본 문서의 승인 상태는 교육 프로젝트 내부 Baseline을 의미하며 ISO 26262 인증 또는 실제 양산 차량의 공식 안전 승인을 의미하지 않는다.

10. 개정 이력
Version변경 사항	

1.0	AUTOSAR 기반 조향 오류 복구 및 진단 시스템 HARA 초안 작성. CAN Timeout, Invalid 입력, WdgM 이상, FAIL-SAFE 전환, Recovery 및 PWM 출력 이상에 대한 Hazardous Event와 Safety Goal 정의
닫기
