# 다른 컴퓨터에서 실행하는 방법

이 문서는 Laminator AI 업무 자동화 day3 프로젝트를 다른 컴퓨터에서 그대로 열고, Codex로 이어 작업하기 위한 요약 가이드다.

## 1. 가져오기

GitHub에 업로드된 저장소를 다른 컴퓨터에서 clone한다.

```powershell
git clone <GITHUB_REPOSITORY_URL>
cd day3
```

ZIP으로 받은 경우에는 압축을 풀고 `day3` 폴더를 연다.

## 2. 바로 열어볼 파일

- 전체 포털: `docs_portal.html`
- Codex 프로젝트 탭: `codex_project_requirements.html`
- AI 분석 대시보드: `docs/dashboard/laminator_ai_dashboard.html`
- 최종 산출물 예시: `docs/dashboard/final_output_example.html`

브라우저에서 파일을 직접 열면 된다. 별도 서버가 필요 없다.

## 3. CSV 업로드 테스트

AI 분석 대시보드에서 아래 4개 파일을 한 번에 선택한다.

```text
sample_data/upload_test_crack_bubble/crack_bubble_daily_production_summary.csv
sample_data/upload_test_crack_bubble/crack_bubble_defect_events.csv
sample_data/upload_test_crack_bubble/crack_bubble_equipment_alarm.csv
sample_data/upload_test_crack_bubble/crack_bubble_laminator_profile_snapshot.csv
```

의도된 결과:

- `LAM-03` 14시대 CRACK 집중
- `LAM-02` 16시대 BUBBLE 집중
- 알람/Profile 이상 연계
- 수량 불일치 검증 항목 표시

## 4. Codex Skill 설치

프로젝트 안에 포함된 skill을 다른 컴퓨터의 Codex skills 폴더로 복사한다.

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.codex\skills" | Out-Null
Copy-Item -Recurse -Force ".\skills\laminator-ai-automation" "$env:USERPROFILE\.codex\skills\laminator-ai-automation"
```

설치 후 새 Codex 세션에서 `laminator-ai-automation` skill이 이 프로젝트 작업 기준으로 사용된다.

## 5. Codex 작업 기준

다른 컴퓨터에서 Codex로 이어 작업할 때는 먼저 아래 파일을 확인한다.

- `CODEX_PROJECT_REQUIREMENTS.md`
- `codex_project_requirements.html`

핵심 규칙:

- 현재 프로젝트 기준은 `day3`.
- 인터뷰는 항상 HTML로 작성.
- 메모장 요청 시 `docs/interviews/question_memo.txt`를 먼저 확인.
- 최종 산출물 피드백은 `docs/dashboard/final_output_example.html`에 먼저 반영.
- 실제 CSV 분석 반영은 `docs/dashboard/laminator_ai_dashboard.html`에서 수행.
- AI는 원인을 확정하지 않고 원인 후보와 검증 우선순위를 제안.

## 6. 주요 폴더

```text
docs/
  dashboard/      HTML 대시보드와 최종 산출물 예시
  requirements/   BRD/PRD와 제출용 요구사항 문서
  design/         화면 시각화 설계서
  references/     논문 레퍼런스와 RCA 프레임워크
  data/           샘플 데이터 설명서
  interviews/     인터뷰 HTML과 질문 메모장
  presentation/   발표 요약
sample_data/      더미 CSV/TXT 데이터
skills/           다른 PC에 설치할 Codex skill
```

## 7. 보안 주의

- 실제 설비 원본 데이터, 고객 정보, 내부 Recipe 상세, 원본 검사 이미지는 GitHub에 올리지 않는다.
- 현재 포함된 CSV는 테스트용 더미 데이터다.
- 현장 데이터로 테스트할 경우 로컬 PC에서만 처리하고, 업로드 전 반드시 비식별 여부를 확인한다.
