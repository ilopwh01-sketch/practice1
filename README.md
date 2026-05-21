# Day3: AI 업무 자동화 인터뷰

## 실행 파일

- `index.html`
- `docs_portal.html`
- `codex_project_requirements.html`

## 목적

한화큐셀 모듈공정팀 업무를 기준으로, 평소 반복 업무를 인터뷰하고 AI 자동화 KPI 성과가 큰 업무 문제를 우선순위로 추천하는 로컬 HTML 도구입니다.

현재 프로젝트 기준 폴더는 `C:\Users\QCELL\Desktop\A1\day3`입니다. `day1`은 이전 Git 작업 폴더입니다.

## 사용 방법

1. 인터뷰 도구는 `index.html`을 브라우저로 엽니다.
2. 제출/발표용 문서 패키지는 `docs_portal.html`을 브라우저로 엽니다.
3. Codex 작업 기준은 `codex_project_requirements.html`을 엽니다.
4. 현재 역할, 반복 업무, 데이터 제약, 자동화 후보를 입력합니다.
5. 마지막 단계에서 `분석하기`를 누릅니다.
6. 추천 결과와 제출/보고용 요약을 확인합니다.

## 다른 컴퓨터에서 실행

다른 PC로 옮겨서 실행하는 방법은 `OTHER_COMPUTER_SETUP.md`를 확인합니다.

Codex skill은 `skills/laminator-ai-automation` 폴더에 포함되어 있습니다.

## 폴더 구조

- `docs/requirements`: BRD/PRD 상세 문서와 HTML 변환본
- `docs/interviews`: 인터뷰 질문지와 인터뷰 요약 HTML
- `docs/presentation`: 최종 발표용 10문항 요약
- `docs/design`: 화면 시각화 설계서
- `docs/dashboard`: AI 원인 분석 대시보드
- `docs/data`: 샘플 데이터 설명서
- `docs/references`: 태양광 모듈 불량 논문 레퍼런스
- `docs/roadmap`: 구현 로드맵
- `sample_data`: 비식별 샘플 CSV/TXT 데이터
- `CODEX_PROJECT_REQUIREMENTS.md`: Codex 작업 운영 기준
- `codex_project_requirements.html`: Codex 프로젝트 요구사항 탭 및 프로젝트 CSV 불러오기
- `OTHER_COMPUTER_SETUP.md`: 다른 컴퓨터 실행/이관 가이드
- `skills/laminator-ai-automation`: 재사용 가능한 Codex skill

## 설계 조건

- 외부 인터넷 연결 없이 동작합니다.
- 설비 원본 데이터 업로드 없이 인터뷰와 점수화만 수행합니다.
- 입력 내용은 브라우저의 로컬 저장소에만 임시 저장됩니다.
