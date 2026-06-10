# Laminator Recipe & Sheet Manager

라미네이터 레시피, 멤브레인 시트, 테프론 시트 관리를 위한 HTML 프로토타입입니다.

## 주요 기능

- 레시피, 멤브레인, 테프론 시트 탭 분리
- 설비라인, 호기, 시트 제조사 필터
- 레시피 파라미터 항목명/값/메모 수정
- 라인/라미호기 선택 시 레시피 표 자동 전환
- 라인/라미호기/적용일자 기준 레시피 버전 저장
- 레시피 항목 숨김 및 복구
- 사용자 신규 등록 데이터 로컬 저장
- 변경이력 표시
- AI 검토용 요약 패널
- 엑셀 파일 다운로드
- 메일 본문 생성

## 실행 방법

브라우저에서 `lami-management.html`을 열면 바로 실행됩니다.

```text
file:///C:/Users/QCELL/Documents/day5/lami-management.html
```

`index.html`은 GitHub Pages나 웹서버 기본 진입용으로 함께 유지합니다.

## 현재 구조

```text
lami-management.html  # 사용자가 직접 열기 쉬운 실행 파일
index.html            # 웹 기본 진입용 동일 사본
README.md             # 프로젝트 설명
.gitignore            # 로컬 백업/임시 파일 제외
```

## 다음 개발 방향

- Python 백엔드 연결
- 실제 `.xlsx` 읽기/쓰기
- OpenAI API 기반 실제 AI 검토
- 메일 자동 발송 또는 Outlook 연동
- 변경이력 영구 저장
- 다중 사용자용 데이터베이스 연결
