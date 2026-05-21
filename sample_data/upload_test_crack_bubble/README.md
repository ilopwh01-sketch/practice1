# Crack/Bubble 업로드 테스트 CSV

이 폴더의 CSV는 `docs/dashboard/laminator_ai_dashboard.html`에서 한 번에 선택해 테스트하는 용도입니다.

## 선택할 파일

- `crack_bubble_daily_production_summary.csv`
- `crack_bubble_defect_events.csv`
- `crack_bubble_equipment_alarm.csv`
- `crack_bubble_laminator_profile_snapshot.csv`

## 의도된 시나리오

- `LAM-03` 14시대에 `CRACK` 10건 이상 집중
- `LAM-03` 14시대에 진공도 하락, 압력 변동, 온도 상승, cycle time 증가
- `LAM-02` 16시대에 `BUBBLE` 8건 이상 집중
- `LAM-02` 16시대에 vacuum hold time, cooling time, heater deviation 알람 발생
- 수량 불일치가 일부 포함되어 검증/후속 조치 영역이 갱신되도록 구성

## 사용 방법

1. `docs/dashboard/laminator_ai_dashboard.html`을 연다.
2. `프로젝트 CSV 불러오기` 영역에서 위 4개 CSV를 동시에 선택한다.
3. KPI, Pareto, 설비/시간 Heatmap, 알람, AI 원인 후보, 검증 숫자가 바뀌는지 확인한다.
