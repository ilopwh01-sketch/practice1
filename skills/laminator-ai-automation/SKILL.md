---
name: laminator-ai-automation
description: Maintain and extend the Laminator FQC AI 업무 자동화 project. Use when Codex needs to update the day3 project documents, HTML dashboards, BRD/PRD, interview HTML files, sample CSVs, paper-based RCA analysis, or portability/GitHub setup for this project.
---

# Laminator AI Automation

## Overview

Use this skill to keep the Laminator FQC AI automation project consistent across documents, dashboards, sample data, and Codex operating rules. The project is a local HTML/CSV package for explaining an AI-assisted daily Laminator defect/equipment report.

## Project Root

Default project root:

```text
C:\Users\QCELL\Desktop\A1\day3
```

Do not confuse this with `C:\Users\QCELL\Desktop\A1\day1`, which was an earlier Git folder.

## Core Rules

- Treat `docs_portal.html` as the main user-facing entry point.
- Treat `codex_project_requirements.html` and `CODEX_PROJECT_REQUIREMENTS.md` as Codex operating rules, not submission BRD/PRD.
- Keep `docs/requirements` for submission requirements such as BRD and PRD.
- When the user asks for interviews, create or update HTML interview files.
- When the user mentions the memo pad, read `docs/interviews/question_memo.txt` before changing documents.
- When feedback changes the final output direction, update `docs/dashboard/final_output_example.html` first, then update specs.
- Keep AI cause language non-final: use "원인 후보", "확인 필요", and "검증 우선순위".
- Never imply automatic equipment recipe control in the pilot scope.

## Standard Workflow

1. Identify whether the request affects project operations, submission specs, dashboard behavior, sample data, or presentation material.
2. Read the relevant reference:
   - `references/project-map.md` for file ownership and update order.
   - `references/data-schemas.md` for CSV expectations.
3. Make focused edits in the matching files.
4. If HTML dashboard behavior changes, verify script syntax with `node --check` on the embedded script when possible.
5. Update `README.md`, `docs_portal.html`, or `CODEX_PROJECT_REQUIREMENTS.md` if the project entry points or workflow changed.

## Dashboard Data Rules

- Use `docs/dashboard/laminator_ai_dashboard.html` for real CSV-driven analysis.
- Use `codex_project_requirements.html` CSV upload only for project/column inspection.
- Keep upload handling browser-local with `FileReader` or `file.text()`.
- Expected upload set:
  - daily production summary CSV
  - defect events CSV
  - equipment alarm CSV
  - laminator profile snapshot CSV
- Test data for CRACK/BUBBLE is under `sample_data/upload_test_crack_bubble`.

## RCA Rules

- CRACK analysis should consider EL pattern, inactive area, upstream handling/stringing, lamination pressure, vacuum, and cycle time.
- BUBBLE/Delamination analysis should consider encapsulant/interface, vacuum, temperature, pressure, cooling time, and material lot.
- Show first candidate, competing hypothesis, and missing verification data.
- Connect literature references through `docs/references/paper_reference.html` and `docs/references/pv_root_cause_framework.html`.

## GitHub/Portability Rules

- Include project setup guidance in `OTHER_COMPUTER_SETUP.md`.
- Include this skill folder in the repository so another PC can copy it into the Codex skills directory.
- Do not commit real plant data, credentials, tokens, or private images.
- It is safe to commit the provided sample CSV files because they are dummy/non-sensitive.

## Validation Checklist

- Main portal opens: `docs_portal.html`.
- Codex project tab opens: `codex_project_requirements.html`.
- Dashboard CSV upload works with the four CSVs in `sample_data/upload_test_crack_bubble`.
- BRD/PRD remain under `docs/requirements`.
- GitHub upload excludes no required dummy sample assets.
