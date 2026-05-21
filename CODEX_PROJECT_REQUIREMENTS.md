# Codex Project Requirements

이 파일은 제출용 BRD/PRD 요구사항이 아니라, Codex가 이 프로젝트를 계속 작업할 때 지켜야 하는 프로젝트 운영 기준이다.

## 1. 현재 프로젝트 기준

- 현재 프로젝트 폴더는 `C:\Users\QCELL\Desktop\A1\day3`이다.
- `C:\Users\QCELL\Desktop\A1\day1`은 이전 Git 작업 폴더이며, 이번 과제 산출물 기준 프로젝트가 아니다.
- 이번 과제의 시작 화면은 `docs_portal.html`이다.
- Codex 작업 기준 탭은 `codex_project_requirements.html`이다.

## 2. 프로젝트 목적

Laminator 공정에서 반복적으로 수행되는 FQC 수율/불량/설비 이상 징후 확인 업무를 AI로 보조하여, CRACK/BUBBLE 불량 감소와 원인 파악 리드타임 단축에 기여하는 과제 패키지를 만든다.

## 3. Codex 작업 원칙

- 사용자가 "인터뷰"를 요청하면 항상 HTML로 만든다.
- 사용자가 "메모장"을 언급하면 `docs/interviews/question_memo.txt`를 먼저 확인한다.
- 사용자가 피드백을 주면 먼저 피드백용 HTML 산출물에 반영하고, 그 다음 BRD/PRD/화면설계/대시보드 스펙에 반영한다.
- 제출 문서와 Codex 운영 기준을 섞지 않는다.
- `docs/requirements`는 제출용 BRD/PRD 요구사항 관리 영역이다.
- `CODEX_PROJECT_REQUIREMENTS.md`와 `codex_project_requirements.html`은 Codex 작업 운영 기준이다.
- 현장 원본 데이터는 외부 반출하지 않는 전제로 작성한다.
- 프로젝트 CSV 불러오기는 브라우저 로컬 FileReader 기반으로만 동작하며, 파일을 외부 서버로 업로드하지 않는다.
- `codex_project_requirements.html`의 CSV 불러오기는 컬럼 구조 확인용이다.
- 실제 KPI, Pareto, 설비/시간 집중, 검증 숫자 반영은 `docs/dashboard/laminator_ai_dashboard.html`의 CSV 불러오기 기능에서 수행한다.
- AI는 설비 원인을 확정하지 않고, 원인 후보와 검증 우선순위를 제안한다.
- 자동 Recipe 변경 또는 설비 제어는 파일럿 범위에서 제외한다.

## 4. 주요 산출물

- `docs_portal.html`: 전체 문서 포털
- `docs/dashboard/final_output_example.html`: 최종 산출물 예시 및 피드백 기준 화면
- `docs/dashboard/laminator_ai_dashboard.html`: AI 원인 분석 대시보드
- `docs/requirements/BRD_Laminator_AI_Automation.md`: BRD
- `docs/requirements/PRD_Laminator_AI_Automation.md`: PRD
- `docs/design/screen_visualization_design.html`: 화면 시각화 설계서
- `docs/references/paper_reference.html`: 논문 레퍼런스
- `docs/references/pv_root_cause_framework.html`: 전문 RCA 프레임워크
- `docs/interviews/question_memo.txt`: 사용자 질문/요청 메모장
- `codex_project_requirements.html`: Codex 운영 기준 탭 및 프로젝트 CSV 불러오기

## 5. 현재 핵심 방향

- FQC 수율 개선이 중심이다.
- 보고서 작성 시간보다 CRACK/BUBBLE 불량 감소와 원인 파악이 더 중요하다.
- BOM/Recipe 조건이 다양하므로 평균, 표준편차, 반복 이력 분석이 필요하다.
- 논문 기반 Failure Mode를 현장 데이터와 연결해 RCA 설득력을 높인다.
- 결과물은 HTML 대시보드, 분석 Excel, 알람/체크리스트, 히스토리 기록 형태로 제안한다.
- 프로젝트 CSV는 `sample_data` 또는 `sample_data/general_pv_module`의 파일을 우선 대상으로 하며, 컬럼 구조와 누락 필드를 확인한다.

## 6. 미확정 항목

- DB/MES 활용 범위
- 실제 실행 환경: 라인 PC, 사내망 PC, 별도 분석 PC 중 선택
- 사람 최종 승인자와 승인 항목
- 실제 현장 데이터 컬럼명
- EL/AOI 원본 이미지 활용 가능 범위

## 7. 다음 작업 규칙

사용자가 피드백을 주면 다음 순서로 처리한다.

1. `codex_project_requirements.html`의 현재 상태 또는 미확정 항목이 바뀌는지 확인한다.
2. 피드백이 최종 산출물 화면에 관한 것이면 `docs/dashboard/final_output_example.html`을 먼저 수정한다.
3. 피드백이 제출 스펙에 관한 것이면 BRD/PRD/화면설계 문서에 반영한다.
4. 피드백이 프로젝트 운영 방식에 관한 것이면 이 파일과 `codex_project_requirements.html`을 수정한다.
5. 피드백이 CSV 데이터 구조에 관한 것이면 `codex_project_requirements.html`의 CSV 검사 기준과 PRD의 필수 컬럼 정의를 함께 확인한다.
6. 피드백이 CSV 분석 반영 방식에 관한 것이면 `docs/dashboard/laminator_ai_dashboard.html`의 CSV 분석 로직을 수정한다.
