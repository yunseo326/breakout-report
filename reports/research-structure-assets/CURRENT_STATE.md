# CURRENT_STATE — 2026-09-14

## 현재 위치

- 새 연구 엔진: `strategy_evaluation/engine.py`; 실행: `./run_research.ps1 run ...`.
- JSON 조합/threshold/제거 실험, 원장 분리, 고유 run_id, 스냅샷/코드 해시 기록을 구현했다.
- 저장된 새 Core 실행 6건은 모두 합성 검증이다. 실제 데이터 실행은 0건이다.
- 이전 작업 기록상 관련 테스트 35개 통과. 이 문서 생성은 백테스트 재실행이 아니다.
- 실제 Primary validation: DATA_BLOCKED. 검증된 raw/split-only 가격·NYSE 세션·피처 가용시각 입력 필요.
- 기존 일일 운영은 `daily_one_click.py` → 기존 포트폴리오 경로다. 전체 운영 엔진 전환은 미완료.
- Top 5% 공식은 미정(null). 기존 결과의 공식을 새 Core 공식으로 간주하지 않는다.

## 어디서 무엇을 읽는가

- 파일 지도·일지 명세: `docs/strategy_evaluation/FILE_STRUCTURE.md`.
- 이미 시험한 것: `EXPERIMENTS.md` → `experiments/legacy_catalog.csv` / `registry.csv`.
- 미시험/차단 후보: `IDEAS.md`. 세부 결정 대기: `question.md`.
- 구현 계약: `docs/strategy_evaluation/WORKFLOW.md`. 시간순 이력: `progress.md`.

## 다음 작업의 조건

실제 입력 스냅샷 검증 후 Primary를 고정하고 기존 전략 1개를 공통 엔진에 연결·대조한다.
`src/`, `data/raw/`, `data/processed/`는 아직 물리적으로 만들거나 이동하지 않았다.
재실행 전 EXPERIMENTS와 같은 설정/입력 해시를 확인한다. 과거 가설·실행시각을 소급 작성하지 않는다.
