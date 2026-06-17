# 1일차 구현 결과보고서

## 1. 프로젝트 개요

- 프로젝트명: Laminator 공정 AI 업무 자동화 대시보드
- 진행일: 2026-05-21
- 진행 차수: 1일차
- 대상 업무: 07시-익일 07시 기준 생산량, NG 수량, 불량률, 설비 Profile, 알람, CRACK/BUBBLE 불량 집중성 분석
- 목표: 수작업 집계와 이상 징후 누락을 줄이고, 불량 원인 후보를 데이터 기반으로 빠르게 정리하는 파일 기반 AI 분석 대시보드 프로토타입 구현

## 2. 1일차 구현 목표

1일차 목표는 실제 현장 DB 연동까지가 아니라, 비식별 샘플 CSV를 기반으로 동작하는 대시보드와 제출용 문서 패키지를 만드는 것이었다.

핵심 방향은 다음과 같다.

- BRD/PRD/화면 설계서를 프로젝트 산출물로 분리
- HTML 기반 대시보드로 최종 결과물 예시 구현
- CSV 업로드 후 KPI와 차트가 자동 갱신되는 구조 구현
- 고정 양식이 아닌 파일도 컬럼 매핑 추천으로 분석 가능한 구조 구현
- 논문 기반 RCA를 단순 참고가 아니라 원인 후보 판단 근거로 연결
- GitHub와 GitHub Pages에 업로드해 다른 PC에서도 확인 가능하게 구성

## 3. 오늘 구현한 주요 내용

### 3.1 문서 산출물

- BRD 상세 문서 작성
- PRD 상세 문서 작성
- 화면 시각화 설계서 작성
- 요구사항 관리 포털 작성
- 논문 레퍼런스 화면 작성
- PV 모듈 Failure Mode 기반 RCA 프레임워크 작성
- 최종 10문항 발표 요약 작성
- 다른 PC 실행 가이드 작성
- Codex 프로젝트 요구사항 문서와 HTML 탭 작성

### 3.2 대시보드 기능

- 생산량, 검사량, NG 수량, NG율 KPI 카드 구현
- 불량 Pareto 구현
- 설비별 NG율 표 구현
- 설비 x 시간대 NG Heatmap 구현
- 시간대별 NG 추이 그래프 구현
- 설비별 시간분포 Boxplot 구현
- Profile/알람 Timeline 구현
- AI 원인 후보 및 근거 표시 구현
- 논문 기반 RCA 근거 화면 구현
- 검증/조치 체크리스트 구현

### 3.3 CSV 업로드 및 데이터 처리

- 프로젝트 CSV 다중 업로드 기능 구현
- 생산 요약, 불량 이벤트, 설비 알람, Profile 파일 자동 분류 구현
- 업로드 데이터 기준으로 KPI, Pareto, Heatmap, 시간별 그래프, Boxplot 자동 갱신 구현
- 브라우저 내부에서만 파일을 읽도록 구성해 원본 데이터 외부 전송을 방지

### 3.4 컬럼 매핑 추천 기능

현장 CSV는 설비별, 담당자별로 컬럼명이 다를 수 있으므로 컬럼 매핑 추천 기능을 추가했다.

구현된 자동 분류 카테고리는 다음과 같다.

- 시간
- 설비
- 불량
- 알람
- Profile
- 생산수량
- BOM/Recipe
- 기타 수치

예시로 `timestamp`, `equipment_id`, `defect_code`, `vacuum_kpa`, `upper_temp_c`, `process_time_sec`, `recipe_id`, `bom_material` 같은 영어 컬럼을 표준 분석 컬럼으로 인식하도록 구성했다. 한국어 컬럼도 인식 가능하지만, 공유와 제출 안정성을 위해 영어 컬럼을 권장 샘플로 추가했다.

### 3.5 위험도 점수 기능

기존의 `긴급`, `주의`, `P1/P2` 중심 표현보다 판단이 직관적이도록 100점 만점 위험도 점수를 추가했다.

위험도 점수 계산에 반영한 요소는 다음과 같다.

- 동일 설비/동일 불량 시간대 집중도
- CRACK/BUBBLE 우선 관리 불량 여부
- Profile 데이터 중첩 여부
- 설비 알람 동시간대 발생 여부
- 수량 불일치 여부
- 설비별 NG율

위험도 점수는 원인을 확정하는 값이 아니라 당일 점검 우선순위를 정하기 위한 보조 지표로 정의했다.

### 3.6 샘플 데이터

다음 샘플 데이터 세트를 준비했다.

- 기본 생산/불량/알람/Profile CSV
- CRACK/BUBBLE 집중 발생 테스트 CSV
- 일반 PV 모듈 Failure Mode 샘플 CSV
- 자유 형식 한국어 컬럼 CSV
- 자유 형식 영어 컬럼 CSV

특히 영어 컬럼 샘플은 다른 PC, GitHub, Excel 환경에서 인코딩 문제가 적도록 권장 샘플로 추가했다.

## 4. 생성된 주요 파일

| 구분 | 파일 |
|---|---|
| 문서 포털 | `docs_portal.html` |
| 대시보드 | `docs/dashboard/laminator_ai_dashboard.html` |
| 최종 결과물 예시 | `docs/dashboard/final_output_example.html` |
| BRD | `docs/requirements/BRD_Laminator_AI_Automation.md`, `docs/requirements/brd_detail.html` |
| PRD | `docs/requirements/PRD_Laminator_AI_Automation.md`, `docs/requirements/prd_detail.html` |
| 화면 설계서 | `docs/design/screen_visualization_design.html` |
| 논문 레퍼런스 | `docs/references/paper_reference.html` |
| RCA 프레임워크 | `docs/references/pv_root_cause_framework.html` |
| 발표 요약 | `docs/presentation/final_10q_fqc_summary.html` |
| 데이터 사전 | `docs/data/data_dictionary.html` |
| 샘플 데이터 | `sample_data/` |
| 자유 형식 영어 샘플 | `sample_data/flexible_schema/free_format_laminator_timeseries_en.csv` |
| 결과보고서 폴더 | `docs/reports/` |

## 5. 검증 결과

1일차 기준으로 수행한 검증은 다음과 같다.

- HTML 대시보드 JavaScript 문법 검사: 통과
- 자유 형식 영어 CSV 컬럼 매핑 테스트: 통과
- `timestamp` 컬럼의 `event_time` 변환 확인
- `equipment_id`, `defect_code`, `vacuum_kpa`, `upper_temp_c` 표준 매핑 확인
- 샘플 CSV 기준 위험도 점수 계산 확인
- GitHub `day3-laminator-ai-automation` 브랜치 업로드 완료
- GitHub Pages `gh-pages` 브랜치 업로드 완료

## 6. 현장 CSV 데이터 요청 항목 및 더미 예시

AI 분석의 핵심은 단순 집계가 아니라 `왜 발생했을 가능성이 높은지`를 통계 분석, 설비 조건, 문헌 근거와 함께 설명하는 것이다. 따라서 2일차부터는 아래 CSV 항목을 현장 데이터 요청 기준으로 사용한다.

| 파일명 | 목적 | 핵심 컬럼 |
|---|---|---|
| `defect_events.csv` | 불량 1건 단위 이벤트 분석 | `event_time`, `line`, `equipment_id`, `module_id`, `defect_code`, `defect_position`, `inspection_tool` |
| `production_summary.csv` | 불량률 계산을 위한 생산/검사 분모 | `work_date`, `time_bucket`, `equipment_id`, `input_qty`, `inspection_qty`, `ng_qty` |
| `laminator_profile.csv` | 설비 조건과 불량 집중 시간 연결 | `sample_time`, `equipment_id`, `recipe_id`, `process_time_sec`, `upper_temp_c`, `vacuum_kpa`, `pressure_kpa` |
| `equipment_alarm.csv` | 알람과 불량 발생 시간 연계 | `alarm_time`, `equipment_id`, `alarm_code`, `alarm_name`, `severity`, `duration_sec` |
| `bom_recipe_lot.csv` | 자재/BOM/Recipe 반복 패턴 분석 | `module_id`, `product_model`, `cell_lot`, `eva_lot`, `recipe_id`, `recipe_version` |

간단 더미데이터 예시는 다음과 같다.

### defect_events.csv

| event_time | line | equipment_id | module_id | defect_code | defect_position | inspection_tool |
|---|---|---|---|---|---|---|
| 2026-05-21 14:03:11 | MODULE-A | LAM-03 | MOD-0006 | CRACK | cell_row=4;cell_col=7;edge | EL |
| 2026-05-21 14:07:28 | MODULE-A | LAM-03 | MOD-0007 | CRACK | cell_row=4;cell_col=8;edge | EL |
| 2026-05-21 16:10:47 | MODULE-A | LAM-02 | MOD-0014 | BUBBLE | zone=center;size=12mm | AOI |

### production_summary.csv

| work_date | time_bucket | line | equipment_id | input_qty | inspection_qty | ng_qty |
|---|---|---|---|---:|---:|---:|
| 2026-05-21 | 14:00 | MODULE-A | LAM-03 | 190 | 186 | 8 |
| 2026-05-21 | 15:00 | MODULE-A | LAM-03 | 188 | 188 | 2 |
| 2026-05-21 | 16:00 | MODULE-A | LAM-02 | 192 | 191 | 4 |

### laminator_profile.csv

| sample_time | line | equipment_id | recipe_id | process_time_sec | upper_temp_c | vacuum_kpa | pressure_kpa |
|---|---|---|---|---:|---:|---:|---:|
| 2026-05-21 14:00:00 | MODULE-A | LAM-03 | RCP-A | 101.4 | 151.2 | -86.4 | 431 |
| 2026-05-21 14:05:00 | MODULE-A | LAM-03 | RCP-A | 102.0 | 151.7 | -85.9 | 434 |
| 2026-05-21 16:10:00 | MODULE-A | LAM-02 | RCP-B | 95.9 | 148.7 | -90.2 | 416 |

별도 요청서는 `docs/data/csv_data_request_template.html`에 생성했다.

## 7. 현재 완성도 판단

현실적으로 보면 현재 완성도는 기준에 따라 다르다.

| 기준 | 완성도 |
|---|---:|
| 과제/발표용 기획 산출물 | 약 90% |
| 화면 프로토타입 | 약 90% |
| 파일 기반 분석 MVP | 약 70% |
| 실제 현장 데이터 적용 | 약 35~45% |
| 프로젝트 전체 범위 | 약 60~65% |

정리하면, 프로그래밍과 발표용 프로토타입은 거의 완성 단계에 가깝지만 실제 현장 적용은 실제 데이터 검증과 운영 보정이 필요하다.

## 8. 남은 이슈와 리스크

- 실제 MES/설비 CSV 컬럼 구조가 아직 검증되지 않았다.
- `.xlsx` 직접 업로드 분석은 아직 구현되지 않았다.
- 위험도 점수 산식은 초기 가설이며 현장 데이터로 보정해야 한다.
- CRACK/BUBBLE 원인 후보는 데이터 기반 추정이며 확정 원인은 담당자 검증이 필요하다.
- 설비 Recipe 자동 변경은 side effect가 커서 파일럿 범위에서 제외해야 한다.
- 반복 이력 DB화, 알림 시스템, 사용자 승인 플로우는 아직 구현되지 않았다.

## 9. 2일차 진행 계획

2일차에는 프로토타입을 더 현장형으로 만드는 것을 목표로 한다.

- 실제 또는 비식별 현장 CSV 1~2개로 업로드 테스트
- 컬럼 매핑 추천 정확도 확인 및 보정
- 위험도 점수 산식 현장 감각에 맞게 조정
- 분석 결과 엑셀 자동 생성 방식 설계
- 사람 최종 승인 항목 정의
- 반복 이슈 히스토리 관리 방식 구체화
- 발표용 스크립트와 시연 순서 정리

## 10. 1일차 결론

1일차에는 Laminator 공정 AI 업무 자동화 프로젝트의 기본 골격을 만들었다. 단순 문서 작성이 아니라 실제로 CSV를 업로드하고 대시보드가 계산되는 프로토타입까지 구현했다.

현재 결과물은 과제 제출과 발표에는 충분히 사용할 수 있는 수준이며, 다음 단계는 실제 현장 데이터와 비교하면서 컬럼 매핑, 위험도 점수, 원인 후보 판단 기준을 보정하는 것이다.
