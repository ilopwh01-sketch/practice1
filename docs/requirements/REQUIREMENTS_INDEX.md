# Requirements 관리 인덱스

이 폴더는 Laminator AI 업무 자동화 프로젝트의 요구사항을 따로 관리하는 부문이다.

## 관리 대상

- `BRD_Laminator_AI_Automation.md`: 비즈니스 요구사항, KPI, 범위, 리스크
- `PRD_Laminator_AI_Automation.md`: 기능 요구사항, 입력/출력, 수용 기준
- `brd_detail.html`: 발표/열람용 BRD HTML
- `prd_detail.html`: 발표/열람용 PRD HTML
- `requirements_portal.html`: 요구사항 관리 포털 및 추적표

## 운영 방식

1. 사용자 피드백 또는 메모장 업데이트 내용을 확인한다.
2. 요구사항 ID를 부여하거나 기존 요구사항을 수정한다.
3. BRD/PRD 중 어느 문서에 반영할지 분리한다.
4. 화면설계, 대시보드, 데이터 설명서에 연결되는 항목을 확인한다.
5. 미확정 항목은 `OPEN` 상태로 남긴다.

## 현재 미확정 항목

- `OPEN-01`: DB/MES 활용 범위
- `OPEN-02`: 실제 실행 환경
- `OPEN-03`: 사람 최종 승인 항목의 세부 승인자
- `OPEN-04`: 실제 현장 데이터 컬럼명과 샘플 데이터 컬럼명의 매핑

## 핵심 요구사항 요약

- 07시-07시 기준 생산량, NG 수량, 불량률 집계
- CRACK/BUBBLE 우선 감소
- 설비/시간대별 불량 집중 탐지
- Profile, 알람, BOM/Recipe 조건과 불량의 연계 분석
- 논문 기반 Failure Mode를 활용한 RCA 후보 제시
- AI 결과는 원인 확정이 아니라 검증 우선순위 제안
- HTML 보고서, 분석 Excel, 체크리스트, 히스토리 기록 출력
