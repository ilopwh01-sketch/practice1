# PRD: AI 일일 설비/불량 현황 보고서 자동 작성 도구

## 1. 문서 목적

본 PRD는 Laminator 공정의 일일 설비/불량 현황 보고 자동화 도구가 제공해야 할 기능, 화면, 데이터 입력/출력, 이상 징후 판단 로직, 검증 요건을 정의한다.

BRD가 비즈니스 필요성과 성과 기준을 설명하는 문서라면, 본 PRD는 실제 제품 또는 자동화 도구가 무엇을 해야 하는지 구체화하는 문서이다.

## 2. 제품 개요

- 제품명: AI 일일 설비/불량 현황 보고서 자동 작성 도구
- 대상 공정: MODULE-A / Laminator
- 대상 기준: 07시-익일 07시
- 주요 출력: 팀장 보고용 HTML, 데이터 분석용 엑셀, 알람/체크리스트, 반복 이슈 히스토리
- 주요 입력: 생산 실적 엑셀/CSV, 불량 이벤트 CSV, 설비 알람 CSV, Laminator Profile CSV, BOM 자재 조건, Recipe 정보, 수기 메모
- 핵심 가치: FQC 수율 개선, CRACK/BUBBLE 불량 집중 조기 탐지, BOM/Recipe 조건별 반복 패턴 탐지, Profile 이상 연계, 원인 후보 도출
- 분석 보강: 외부 태양광 모듈 Failure Mode 논문을 참고해 Crack, Bubble/Delamination, Inactive area, PID suspect 등 일반 원인분석 기준을 함께 제공

## 3. 사용자 정의

| 사용자 | 주요 목적 | 사용 기능 |
|---|---|---|
| 모듈공정기술 담당자 | 일일 데이터 분석 및 보고 작성 | 파일 업로드, 자동 분석, 원인 후보 검토 |
| 공정팀원 | 불량 집중 및 조치 필요 사항 확인 | HTML 보고서 조회, 엑셀 상세 분석 |
| 팀장 | 주요 이슈와 후속 조치 판단 | Overview, 핵심 이슈, 조치 필요 사항 확인 |
| 설비 담당자 | 알람/Profile 이상 확인 | Profile Timeline, 알람 연계 화면 확인 |
| 품질/검사 담당자 | NG 유형과 검사 이슈 확인 | 불량 Pareto, 검사 NG 상세 확인 |

## 4. MVP 범위

### 4.1 1차 MVP

1차 MVP는 파일 기반 분석과 보고서 자동 생성을 목표로 한다.

- CSV/엑셀 파일 업로드
- 07시-익일 07시 기준 데이터 필터링
- 생산량, 검사량, NG 수량, 불량률 계산
- 불량 코드 Pareto 생성
- 설비별 NG율 비교
- 시간대별 NG 집중 Heatmap 생성
- 수량 불일치 탐지
- 팀장 보고용 HTML 생성
- 분석 엑셀 생성
- AI 원인 후보와 근거 데이터 표시
- BOM/Recipe 조건별 반복 이슈 히스토리 기록

### 4.2 2차 확장

- 공정 time, 온도, 진공도 Profile 이상 탐지
- 설비 알람과 불량 집중 시간 연계
- AI 원인 후보 및 신뢰도 표시
- 수기 메모 자동 요약 및 조치 사항 반영

### 4.3 3차 확장

- EL/AOI 이미지 기반 유사 불량 검색
- 과검/오검 패턴 분류
- 이미지 학습 기반 불량 원인 추천
- 다공정 확장

## 5. 입력 데이터 요구사항

### 5.1 생산 요약 데이터

파일 예시: `daily_production_summary_2026-05-20.csv`

필수 컬럼:

- work_date
- time_bucket
- line
- equipment_id
- product_model
- input_qty
- output_qty
- inspection_qty
- ng_qty
- rejudge_qty
- reinput_qty
- top_defect_code
- top_defect_name

사용 목적:

- 07시-07시 기준 생산량/검사량/NG 수량 집계
- 불량률 계산
- 설비별/시간대별 집중 분석
- 수량 불일치 탐지

### 5.2 불량 이벤트 데이터

파일 예시: `defect_events_2026-05-20.csv`

필수 컬럼:

- event_time
- line
- equipment_id
- module_id
- product_model
- defect_code
- defect_name
- inspection_tool
- judge
- rejudge_flag
- reinput_flag
- image_ref

사용 목적:

- 불량 Pareto 분석
- 동일 설비/동일 불량 집중 탐지
- 검사 도구별 NG 현황 확인
- 재판정/재투입 이력 확인

### 5.3 설비 알람 데이터

파일 예시: `equipment_alarm_2026-05-20.csv`

필수 컬럼:

- alarm_time
- line
- equipment_id
- alarm_code
- alarm_name
- severity
- duration_sec

사용 목적:

- 불량 집중 시간과 알람 발생 시간 연계
- 알람 심각도별 원인 후보 표시
- 설비 담당자 확인 항목 생성

### 5.4 Laminator Profile 데이터

파일 예시: `laminator_profile_snapshot_2026-05-20.csv`

필수 컬럼:

- sample_time
- line
- equipment_id
- recipe_id
- process_time 또는 cycle_time_sec
- temperature 관련 컬럼
- vacuum 관련 컬럼
- profile_status

1차 핵심 컬럼:

- 공정 time
- 온도
- 진공도

사용 목적:

- Profile 이상 탐지
- 불량 집중 시간과 Profile 이상 시간 비교
- 원인 후보 도출

### 5.5 수기 메모

파일 예시: `manual_memo_2026-05-20.txt`

사용 목적:

- 담당자 관찰 내용 반영
- 보고서의 조치 필요 사항 및 익일 확인 항목 보강
- 데이터만으로 설명하기 어려운 현장 맥락 반영

### 5.6 BOM/Recipe 조건 데이터

파일 예시: BOM/Recipe 조건 엑셀, MES 조회 결과, 수기 Recipe 변경 이력

사용 목적:

- BOM 자재 종류별 CRACK/BUBBLE 발생률 비교
- Recipe 조건별 평균, 표준편차, 이상 발생 이력 확인
- 동일 자재/Recipe 조합에서 반복 발생한 불량 히스토리 누적
- 개선 Item 또는 특허 아이디어 후보 도출

### 5.7 일반 PV 모듈 레퍼런스 샘플

파일 예시: `sample_data/general_pv_module/pv_failure_mode_reference.csv`

사용 목적:

- 실제 현장 데이터가 없을 때 일반 태양광 모듈 Failure Mode 기준으로 원인분석 구조를 설명한다.
- CRACK, BUBBLE, Delamination, Inactive area, Discoloration, PID suspect, Interconnect issue를 일반 샘플로 제공한다.
- 외부 논문 기반의 원인 후보, 확인 데이터, 권장 조치를 대시보드 로직에 연결한다.

## 6. 기능 요구사항

### FR-01. 파일 업로드 및 데이터 검증

사용자는 생산 요약, 불량 이벤트, 설비 알람, Profile, 수기 메모 파일을 업로드할 수 있어야 한다.

검증 요건:

- 필수 컬럼 누락 시 오류 메시지 표시
- 날짜/시간 컬럼 파싱 실패 시 오류 표시
- 숫자 컬럼에 문자 포함 시 경고 표시
- 파일별 데이터 기간이 07시-익일 07시 범위와 맞는지 확인

### FR-01A. 컬럼 매핑 추천 및 자동 구조화

현장 CSV/엑셀은 설비별, DB별, 담당자별로 컬럼명이 다를 수 있으므로 도구는 업로드 직후 컬럼 구조를 먼저 분석해야 한다.

기능 요구사항:

- 파일 컬럼을 시간, 설비, 불량, 알람, Profile, 생산수량, BOM/Recipe, 기타 수치 카테고리로 자동 분류한다.
- `event_time`, `sample_time` 같은 표준 컬럼명이 없어도 `측정일시`, `발생시각`, `장비명`, `NG유형`, `진공도`, `상부온도`, `프레스압력`, `공정Time`, `Recipe`, `BOM자재` 같은 현장식 명칭을 표준 컬럼으로 추천 매핑한다.
- PC/엑셀/GitHub 환경에서 한글 인코딩 이슈가 있을 수 있으므로 제출용·공유용 샘플 데이터의 권장 컬럼명은 영어로 관리한다.
- 권장 영어 컬럼 예시는 `timestamp`, `equipment_id`, `line`, `defect_code`, `judge`, `vacuum_kpa`, `upper_temp_c`, `pressure_kpa`, `process_time_sec`, `recipe_id`, `bom_material`, `image_ref`, `note`이다.
- 추천 결과에는 원본 컬럼명, 추천 카테고리, 표준 컬럼명, 추천 신뢰도를 표시한다.
- 추천 신뢰도가 낮은 파일은 자동 분석 전에 사람이 확인해야 할 항목으로 표시한다.
- 하나의 시계열 파일 안에 불량 정보와 Profile 정보가 같이 들어있으면 불량 분석과 Profile 분석 양쪽에 모두 활용한다.
- 컬럼 매핑은 원본 데이터를 수정하지 않고 브라우저 내부에서 표준 분석용 구조로만 변환한다.

분석 연결 요구사항:

- 시간 컬럼은 시간대별 NG 그래프, Heatmap, 알람/Profile Timeline에 연결한다.
- 설비 컬럼은 설비별 NG율, Boxplot, 집중성 분석에 연결한다.
- 불량 컬럼은 Pareto, CRACK/BUBBLE 우선순위, 원인 후보 생성에 연결한다.
- Profile 컬럼은 공정 time, 온도, 진공도, 압력 이상 탐지에 연결한다.
- BOM/Recipe 컬럼은 조건별 반복 패턴과 재발 히스토리 분석에 연결한다.

### FR-02. 07시-07시 기준 집계

도구는 하루 기준을 00시-24시가 아니라 07시-익일 07시로 적용해야 한다.

출력 항목:

- 총 input_qty
- 총 output_qty
- 총 inspection_qty
- 총 ng_qty
- 불량률
- 설비별 NG 수량/NG율
- 시간대별 NG 수량

### FR-03. 불량 Pareto 분석

도구는 defect_code 기준으로 NG 수량을 집계하고, 발생 비중과 누적 비중을 계산해야 한다.

표시 요건:

- CRACK, BUBBLE을 우선 관리 불량으로 표시
- 상위 불량 코드 내림차순 표시
- 상위 불량의 전체 NG 대비 비중 표시
- 특정 불량이 전체 NG의 40% 이상이면 주요 불량으로 강조

### FR-04. 설비별/시간대별 집중성 분석

도구는 equipment_id와 time_bucket 기준으로 NG 집중 구간을 탐지해야 한다.

이상 기준:

- 동일 설비에서 동일 불량이 1시간 내 5건 이상 발생
- 특정 설비의 시간당 불량률이 전체 평균의 2배 이상
- 특정 시간대 NG 수량이 직전 3시간 평균 대비 3배 이상

표시 방식:

- 설비별 NG율 Bar Chart
- 설비 x 시간대 Heatmap
- 100점 만점 위험도 점수

위험도 점수 요구사항:

- 핵심 이슈는 단순 `긴급`, `주의`, `P1/P2` 문구보다 100점 만점 위험도 점수로 우선 표시한다.
- 위험도는 불량 집중도, CRACK/BUBBLE 우선순위, Profile/알람 중첩, 수량 불일치, 설비 NG율을 합산해 계산한다.
- 80점 이상은 높은 점검 우선순위, 55~79점은 중간 점검 우선순위, 54점 이하는 모니터링 대상으로 분류하되 화면에는 점수를 먼저 표시한다.
- 위험도 점수는 원인 확정값이 아니라 당일 점검 우선순위를 정하기 위한 보조 지표로 사용한다.

### FR-05. Profile 이상 탐지

도구는 공정 time, 온도, 진공도 데이터를 기준으로 Profile 이상을 탐지해야 한다.

이상 기준:

- 공정 time이 기준값 또는 최근 평균 대비 10% 이상 증가
- 온도가 관리 기준 상한/하한을 벗어남
- 진공도가 기준 범위에서 벗어남
- Profile 이상 발생 시간과 NG 집중 시간의 차이가 30분 이내

표시 방식:

- Profile Timeline
- 이상 구간 색상 강조
- NG 집중 시간과 겹치는 구간 표시

### FR-06. 설비 알람 연계

도구는 설비 알람 발생 시점과 불량 이벤트 발생 시점을 비교해야 한다.

이상 기준:

- 알람 발생 전후 30분 이내 동일 불량 연속 발생
- severity가 HIGH인 알람과 NG 집중이 겹침

출력 항목:

- 관련 알람 코드
- 알람명
- 심각도
- 지속 시간
- 관련 불량 코드
- 시간 차이

### FR-07. 수량 불일치 탐지

도구는 생산량, 검사량, NG 수량, 재판정, 재투입 이력을 기준으로 수량 불일치 가능성을 표시해야 한다.

탐지 예시:

- output_qty와 inspection_qty가 다름
- ng_qty와 defect_events의 NG 이벤트 수가 다름
- 재판정 또는 재투입 이력이 있는 시간대에서 수량 차이 발생

표시 방식:

- 검증 필요 경고
- 관련 시간대/설비 표시
- 재판정/재투입 건수 표시

### FR-08. AI 원인 후보 생성

도구는 불량 집중, Profile 이상, 설비 알람, 수량 불일치 정보를 종합하여 원인 후보를 제시해야 한다.

원인 후보 예시:

- LAM-03 14시대 CRACK 집중은 진공도 하락 및 압력 변동 알람과 시간적으로 겹침
- 재판정/재투입 이력이 있어 수량 불일치 확인 필요
- 15시 이후 CRACK은 감소했으나 추가 모니터링 필요
- Bubble/Delamination은 진공도, 라미네이션 온도, 압력, 냉각 시간, 외관 Bubble count를 함께 확인
- EL의 dark line, inactive area, 반복 crack pattern은 cell crack 또는 interconnect issue 후보로 표시

제약:

- AI는 원인을 확정하지 않는다.
- 표현은 "원인 후보", "확인 필요", "연관 가능성"으로 제한한다.
- 담당자가 최종 승인한다.
- 원인 후보는 1차 후보, 경쟁 가설, 배제/분리해야 할 가설을 함께 표시한다.

근거 표시 방식:

- 근거 데이터는 최소 3개 이상 함께 표시한다.
- 근거 유형은 불량 집중, Profile 이상, 설비 알람, 수량 불일치, 수기 메모로 구분한다.
- 신뢰도는 High, Medium, Low로 표시한다.
- High는 불량 집중, Profile 이상, 설비 알람이 같은 시간대에 동시에 확인되는 경우로 제한한다.
- Medium은 불량 집중과 Profile 이상 또는 알람 중 2개 근거가 연결되는 경우로 표시한다.
- Low는 단일 근거만 존재하거나 수량 불일치 검증이 필요한 경우로 표시한다.
- CRACK 분석에는 EL pattern, pre-lamination EL 여부, handling/stringing log, lamination pressure/cycle, vacuum profile을 함께 표시한다.
- BUBBLE/DELAMINATION 분석에는 vacuum, lamination temperature, pressure, cooling time, encapsulant lot, visual bubble count를 함께 표시한다.

문헌 기반 RCA 보강 요구사항:

- 도구는 논문/리뷰의 내용을 원문 요약으로만 보여주지 않고, `Failure Mode`, `현장 데이터 신호`, `원인 후보`, `추가 검증 항목`으로 변환해 표시해야 한다.
- CSV 업로드 또는 컬럼 매핑 결과가 바뀌면 주요 불량 코드, 설비 집중, Profile/알람 중첩을 다시 계산하고 그 결과에 맞는 문헌 근거를 자동 재선택해야 한다.
- 문헌 근거는 고정 참고자료가 아니라 `CRACK`, `BUBBLE/DELAMINATION`, `PID`, `기타/미분류` 불량 모드에 따라 다른 판단 기준과 검증 항목을 보여줘야 한다.
- CRACK은 `EL crack pattern`, `inactive area`, `cell/string 위치`, `pre-lamination EL 여부`, `handling/stringing log`, `lamination pressure/cycle/vacuum profile`을 함께 비교해야 한다.
- BUBBLE/DELAMINATION은 `encapsulant lot`, `interface 추정 위치`, `vacuum_kpa`, `temperature`, `pressure`, `cooling_time`, `visual_bubble_count`를 함께 비교해야 한다.
- 원인 후보는 항상 `1차 후보`, `경쟁 가설`, `아직 배제 불가한 가설`로 나누어 표시해야 한다.
- RCA 신뢰도는 불량 집중성, Profile 이상, 설비 알람, 문헌 Failure Mode 일치성, BOM/Recipe 반복성을 가중치로 계산하되, 결과는 설비 자동 제어가 아닌 사람 검증 우선순위로 사용해야 한다.
- 문헌 근거가 표시될 때는 원문 링크, 적용한 판단 기준, 현장 데이터 컬럼, 현재 사례에서의 해석을 함께 제공해야 한다.

### FR-09. BOM/Recipe 조건별 반복 패턴 분석

도구는 BOM 자재 종류와 Recipe 조건별로 NG 수량, 불량률, 평균, 표준편차를 계산하고 반복 패턴을 탐지해야 한다.

분석 항목:

- BOM 자재 종류별 CRACK/BUBBLE 발생률
- Recipe 조건별 NG율 평균 및 표준편차
- 동일 BOM/Recipe 조합에서 반복 발생한 불량 이력
- 설비/시간대/Profile 이상과 BOM/Recipe 조건의 중첩 여부
- 개선 Item 후보 및 특허 아이디어 후보 태깅

### FR-10. HTML 보고서 생성

팀장 보고용 HTML은 한 화면에서 핵심 이슈를 빠르게 파악할 수 있어야 한다.

필수 영역:

- 금일 핵심 이슈
- Overview KPI 카드
- P1 이상 징후
- 최우선 AI 원인 후보
- 조치 필요 사항
- 불량 Pareto
- 설비별 NG율
- 시간대별 NG Heatmap
- Profile/알람 Timeline
- 수량 불일치 경고
- 익일 확인 항목
- BOM/Recipe 조건별 반복 이슈
- 논문 기반 RCA 근거 매트릭스
- 1차 후보/경쟁 가설/배제 불가 가설 비교
- AI 액션아이템
- 히스토리 기록 요약

### FR-11. 분석 엑셀 생성

데이터 분석용 엑셀은 담당자가 원본 근거를 다시 확인할 수 있도록 구성한다.

권장 시트:

- Overview
- Defect_Pareto
- Equipment_NG_Rate
- Time_Heatmap
- Profile_Anomaly
- Alarm_Link
- Quantity_Validation
- Action_Items
- Evidence_Log
- Literature_Map
- Literature_Evidence_Matrix
- RCA_Hypothesis_Test
- BOM_Recipe_History

### FR-12. 알람/체크리스트 생성

도구는 이상 징후가 기준을 초과하면 알람 또는 체크리스트 형태로 조치 항목을 생성해야 한다.

예시:

- LAM-03 CRACK 1시간 내 5건 이상 발생 시 P1 알람
- 동일 Recipe 조건에서 CRACK/BUBBLE 반복 시 Recipe 확인 체크리스트 생성
- Profile 이상과 알람이 겹치면 설비 점검 체크리스트 생성
- 조치 완료 여부와 개선 결과를 히스토리로 기록

## 7. 화면 요구사항

화면 시각화 설계는 `../design/screen_visualization_design.html`을 기준으로 한다.

### 7.1 첫 화면

첫 화면에는 다음 항목을 표시한다.

- 금일 핵심 이슈
- 총 생산량
- 총 검사량
- 총 NG 수량
- 불량률
- P1 이상 징후 수
- 최우선 원인 후보
- 즉시 조치 필요 사항

### 7.2 불량 분석 화면

- 불량 Pareto Bar Chart
- 상위 불량 비중
- 전일 또는 평균 대비 증가 여부

### 7.3 설비/시간 집중 화면

- 설비별 NG율
- 시간대별 NG Heatmap
- 집중 구간 클릭 시 상세 이벤트 목록

### 7.4 Profile/알람 화면

- 공정 time, 온도, 진공도 Timeline
- 알람 발생 시점 표시
- NG 집중 시간과 겹치는 구간 강조

### 7.5 검증 화면

- 07시-07시 시간 범위 확인
- NG 수량/종류/불량률 검증
- 수량 불일치 확인
- 재판정/재투입 확인

## 8. 비기능 요구사항

### 8.1 보안

- 원본 데이터는 외부로 전송하지 않는다.
- 로컬 PC, 라인 PC 또는 사내망 환경에서 실행한다.
- 비식별 샘플 데이터로 테스트한다.
- 실행 환경은 최종 확인 전까지 라인 PC 또는 사내망 PC로 가정한다.
- DB 활용 범위는 사내 보안/IT/MES 정책에 따라 결정하며, 파일 기반 분석을 1차 MVP로 적용한다.

### 8.2 사용성

- 팀장 보고용 HTML은 별도 설명 없이 읽을 수 있어야 한다.
- 이상 징후는 P1/P2/P3로 우선순위를 표시한다.
- AI 원인 후보 옆에는 근거 데이터를 함께 표시한다.

### 8.3 검증 가능성

- 모든 계산 결과는 사용 파일과 컬럼 기준을 확인할 수 있어야 한다.
- AI 요약 결과는 담당자가 수정할 수 있어야 한다.
- 최종 보고 전 사람이 승인한다.
- 사람이 최종 승인해야 하는 항목은 원인 후보 확정, 조치 계획, 팀장 보고 전 최종 문구이다.

### 8.4 확장성

- Laminator 외 다른 설비에도 적용할 수 있도록 equipment_id 기준으로 확장 가능해야 한다.
- Profile 컬럼은 공정별로 매핑을 변경할 수 있어야 한다.
- 불량 코드 기준은 현장 기준에 맞게 수정 가능해야 한다.

## 9. 사용자 흐름

1. 사용자가 당일 생산/불량/알람/Profile 파일을 선택한다.
2. 도구가 필수 컬럼과 07시-07시 시간 범위를 검증한다.
3. 도구가 생산량, 검사량, NG 수량, 불량률을 자동 집계한다.
4. 도구가 불량 Pareto와 설비/시간 집중 구간을 생성한다.
5. 도구가 Profile 이상 및 알람 연계를 분석한다.
6. 도구가 수량 불일치와 재판정/재투입 이력을 표시한다.
7. 도구가 AI 원인 후보와 조치 필요 사항을 생성한다.
8. 담당자가 결과를 검토하고 수정한다.
9. 팀장 보고용 HTML과 분석 엑셀을 저장한다.

## 10. 수용 기준

| ID | 수용 기준 |
|---|---|
| AC-01 | 07시-익일 07시 기준으로 데이터가 필터링된다. |
| AC-02 | NG 수량, 불량 종류, 불량률이 자동 계산된다. |
| AC-03 | 설비별/시간대별 NG 집중 구간이 표시된다. |
| AC-04 | 동일 설비/동일 불량 1시간 5건 이상 발생 시 P1 경고가 표시된다. |
| AC-05 | 공정 time, 온도, 진공도 이상이 Profile 화면에 표시된다. |
| AC-06 | 알람 전후 30분 내 불량 집중 발생 시 알람 연계 원인 후보가 생성된다. |
| AC-07 | 수량 불일치와 재판정/재투입 이력이 검증 화면에 표시된다. |
| AC-08 | 팀장 보고용 HTML 파일이 생성된다. |
| AC-09 | 분석 엑셀 파일이 생성된다. |
| AC-10 | AI 원인 후보는 확정 표현이 아니라 검토 필요 표현으로 출력된다. |
| AC-11 | AI 원인 후보마다 근거 데이터와 High/Medium/Low 신뢰도가 표시된다. |
| AC-12 | CRACK, BUBBLE 불량이 우선 관리 대상으로 표시된다. |
| AC-13 | BOM/Recipe 조건별 반복 불량 히스토리가 기록된다. |
| AC-14 | AI 액션아이템, 알람, 체크리스트가 생성된다. |

## 11. 추가 확인 필요 사항

최종 구현 전 추가 확인이 필요한 항목은 다음과 같다.

- 실제 실행 환경 상세: 라인 PC, 사내망 PC, 또는 별도 분석 PC 중 선택
- 신뢰도 High/Medium/Low 기준에 대한 현장 검토
- 팀장 보고용 HTML 첫 화면 구성 최종 검토
- 분석 엑셀 시트 구성 최종 검토

위 항목은 `../interviews/final_interview_brd_prd.html`에서 한 번에 답변할 수 있다.
