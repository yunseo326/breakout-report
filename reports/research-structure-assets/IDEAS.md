# IDEAS — 미시험·차단 후보 (2026-09-14)

실행 전 EXPERIMENTS.md와 registry/legacy_catalog에서 중복을 확인한다.
계획·데이터 차단·규칙 불명확을 구분하며, 이 문서가 실행을 자동 승인하지는 않는다.

| 후보 | 현재 상태 | 먼저 필요한 것 |
| --- | --- | --- |
| 가치·수익성·재무건전성 + 가격 모멘텀 | 해당 조합의 정식 실행 근거 미확인 | 기존 QUALITY/PBR_FSCORE/VALUE_MOMENTUM 계열과 중복 확인 후 당시 공시/가용시각 자료와 전략 정의 확보 |
| 뉴스 피처 조합·제거 | 새 피처 예정, 구현된 것은 입력 계약 | 뉴스 원본·발표/가용시각·피처 정의 |
| 금리 피처 조합·제거 | 새 피처 예정, 구현된 것은 입력 계약 | 금리 관측·발표·수정본 시각과 변환 정의 |
| PEAD + analyst revision | DATA_BLOCKED, 3개 버전 기록 존재 | 당시 EPS consensus/추정치 변화 자료. 신규 전략으로 중복 등록하지 않음 |
| LETF 3/76/78/79 | 과거 목록에 계획만, 규칙·실행 근거 미확인 | 원본 정의 확보 후 중복 점검 |

## 이미 시험한 영역

RMSE/ATR/볼린저, Donchian·Darvas·Larry Williams·NR7, RSI/일목/피보나치 조합,
RMSE 패턴, 잔차 평균회귀·RMSE/PCA, 모멘텀/섹터중립, 확정 복수 저점 추세선은
실험 기록이 있다. 새 threshold/조합은 새 run으로 연결하고 새 전략 코드부터 만들지 않는다.
근거: `EXPERIMENTS.md`, 신규 실험의 strategy_definition, legacy_catalog.
구 question.md 및 미포함 목록의 PLANNED_ONLY 표시는 후속 시험보다 오래된 기록일 수 있다.
