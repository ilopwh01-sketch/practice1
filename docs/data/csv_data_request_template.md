# CSV 데이터 요청서

## 목적

이 요청서는 Laminator 공정 CRACK/BUBBLE 불량에 대해 단순 집계가 아니라 `왜 발생했을 가능성이 높은지`를 분석하기 위한 데이터 항목을 정리한다.

AI 분석의 핵심은 다음 3가지를 연결하는 것이다.

- 불량 발생 현상: 언제, 어느 설비, 어떤 위치에서 어떤 불량이 발생했는가
- 통계 분석: 평소 대비 얼마나 이상한가, 특정 설비/시간/Recipe/BOM에 집중되는가
- 근거 해석: 설비 Profile, 알람, BOM/Recipe, 문헌 Failure Mode와 연결되는가

## 요청 파일 목록

| 우선순위 | 파일명 | 목적 | 핵심 컬럼 |
|---|---|---|---|
| 필수 | `defect_events.csv` | 불량 1건 단위 이벤트 분석 | `event_time`, `line`, `equipment_id`, `module_id`, `defect_code`, `defect_position`, `inspection_tool` |
| 필수 | `production_summary.csv` | 불량률 계산을 위한 생산/검사 분모 | `work_date`, `time_bucket`, `equipment_id`, `input_qty`, `inspection_qty`, `ng_qty` |
| 필수 | `laminator_profile.csv` | 설비 조건과 불량 집중 시간 연결 | `sample_time`, `equipment_id`, `recipe_id`, `process_time_sec`, `upper_temp_c`, `vacuum_kpa`, `pressure_kpa` |
| 권장 | `equipment_alarm.csv` | 알람과 불량 발생 시간 연계 | `alarm_time`, `equipment_id`, `alarm_code`, `alarm_name`, `severity`, `duration_sec` |
| 권장 | `bom_recipe_lot.csv` | 자재/BOM/Recipe 반복 패턴 분석 | `module_id`, `product_model`, `cell_lot`, `eva_lot`, `recipe_id`, `recipe_version` |
| 권장 | `defect_position_image.csv` | 불량 위치/이미지 패턴 분석 | `module_id`, `defect_code`, `cell_row`, `cell_col`, `zone`, `defect_size_mm`, `image_ref` |

## 더미 데이터 예시

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

### equipment_alarm.csv

| alarm_time | line | equipment_id | alarm_code | alarm_name | severity | duration_sec |
|---|---|---|---|---|---|---:|
| 2026-05-21 13:58:20 | MODULE-A | LAM-03 | VAC-DEV | Vacuum pressure deviation | High | 180 |
| 2026-05-21 14:06:33 | MODULE-A | LAM-03 | PRS-FLU | Press pressure fluctuation | Medium | 95 |
| 2026-05-21 16:02:11 | MODULE-A | LAM-02 | COOL-TIME | Cooling time warning | Medium | 120 |

### bom_recipe_lot.csv

| module_id | product_model | cell_lot | eva_lot | glass_lot | recipe_id | recipe_version |
|---|---|---|---|---|---|---|
| MOD-0006 | MOD-A1 | CELL-L03 | EVA-A21 | GLS-11 | RCP-A | v3 |
| MOD-0007 | MOD-A1 | CELL-L03 | EVA-A21 | GLS-11 | RCP-A | v3 |
| MOD-0014 | MOD-A1 | CELL-L04 | EVA-B08 | GLS-12 | RCP-B | v2 |

## 분석 품질 기준

- NG 데이터만 있으면 `어디서 많이 났는가`까지 분석 가능하다.
- 생산 분모 데이터가 있으면 `불량률`과 `설비별 비교`가 가능하다.
- 설비 Profile과 알람이 있으면 `왜 났을 가능성이 높은가`에 대한 후보를 만들 수 있다.
- BOM/Recipe와 위치 데이터가 있으면 `반복 조건`과 `Failure Mode` 연결 분석이 가능하다.
