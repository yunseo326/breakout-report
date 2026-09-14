# 2trading 핵심 파일 구조와 연구 기록 (2026-09-14)

소스는 현재 2trading 디렉터리다. 아래는 요청한 구조와 **현재 실제 위치의 대응표**이며,
src/raw/processed로 이동을 끝냈다는 뜻이 아니다. 파일·코드·원장은 기존 위치에 유지했다.
CURRENT_STATE/EXPERIMENTS/IDEAS는 이번에 실제 안내 문서로 추가했다.

## 1. 요청 구조 → 현재 위치

| 요청한 역할 | 현재 파일/폴더 | 역할 | 상태 |
| --- | --- | --- | --- |
| AGENTS.md | AGENTS.md | 작업 원칙·승인 범위·기록 규칙 | 유지 |
| CURRENT_STATE.md | CURRENT_STATE.md; WORKSPACE.md; experiments/profiles/primary_v1.json | 현재 구현·검증 상태와 다음 차단 요인. WORKSPACE는 파일 지도 | 이번에 안내 문서 추가 |
| EXPERIMENTS.md | EXPERIMENTS.md; experiments/registry.csv; experiments/legacy_catalog.csv; experiments/legacy_ledger_index.csv | 이미 해본 전략과 실행·결과·원장 위치를 찾아 중복 실험 방지 | 이번에 안내 문서 추가 |
| IDEAS.md | IDEAS.md; question.md | 미시험 아이디어·데이터 차단·규칙 불명확 후보. question은 결정 대기 항목 | 이번에 안내 문서 추가 |
| results/ | results/; incoming_experiments/; experiments/<experiment_id>/<run_id>/ | 기존 결과, 이전 표준화 묶음, 새 Core 실행 묶음이 분산. 새 원본은 실행 폴더 | 역할 대응; 이동하지 않음 |
| reports/ | reports/; experiments/<experiment_id>/<run_id>/report.html; outputs/; artifact/ | 사람용 HTML/Markdown, 실행 보고서, Excel, 기존 대시보드 | 역할 대응; 이동하지 않음 |
| data/raw/ | cache/prices/; cache/prices_long/; cache/universe/; cache/earnings/; cache/market_cap/; data/snapshots/ | 가격·유니버스·실적·시총 입력과 고정 스냅샷. 기존 조정 캐시는 검증된 raw 가격과 다름 | raw 폴더는 아직 없음 |
| data/processed/ | results/events_*.parquet; results/features_*.parquet; results/*candidates*.csv; data/snapshots/<hash>/features.* | 재사용 가치가 큰 이벤트·피처·후보. 시점/버전/가격 기준이 같을 때 재사용 | processed 폴더는 아직 없음 |
| src/engine/ | strategy_evaluation/engine.py; strategy_evaluation/config.py; strategy_evaluation/interfaces.py | Core 연구 공통 실행·설정·Strategy/DataProvider 계약 | src 폴더는 아직 없음 |
| src/data/ | strategy_evaluation/data_provider.py; strategy_evaluation/snapshot.py; data_fetch.py; universe.py; fetch_daily_prices.py | 입력 스냅샷 검증 및 기존 가격·유니버스 수집 | 역할 대응 |
| src/features/ | features/; events/; strategy/research_strategies.py; strategy_evaluation/combinations.py | 피처·신호 계산과 JSON 조합/threshold/조건 제거 | 역할 대응 |
| src/results/ | strategy_evaluation/storage.py; strategy_evaluation/metrics.py; strategy_evaluation/regime.py | 원장·Scorecard·일별 점유·국면/연도 집계. 지표 계산은 metrics.py | 역할 대응 |
| src/reporting/ | report_design.py; publish_report.py; strategy_evaluation/documentation.py; build_*report.py | 보고서 생성·공통 스타일·GitHub Pages 발행 | 과거 개별 builder 포함 |
| src/validation/ | strategy_evaluation/validation.py; tests/ | 학습 라벨 분할 경계 제거, 시점 일관성·체결·슬롯·원장 재현 검증 | tests는 독립 폴더 유지 권장 |
| 추가 핵심: strategy/ | strategy/ | 전략별 고유 신호·진입·청산 로직. 공통 엔진과 구분 | 유지 |
| 추가 핵심: experiments/ | experiments/examples/; experiments/profiles/; experiments/registry.csv | 실험 설정, 평가 조건, 실행 인덱스. 결과와 설정의 연결점 | 유지 |
| 추가 핵심: history/ | history/README.md; history/source_inventory.csv; history/legacy_sources_*.zip | 기존 소스 보관·해시·원래 경로. 과거 결과의 재현 근거 | 삭제 대신 보존 |
| 추가 핵심: docs/ | docs/strategy_evaluation/WORKFLOW.md; docs/strategy_evaluation/reference/ | 현재 계약과 Core/Glossary/Output 원문. 원문과 구현 설명 구분 | 유지 |
| 추가 핵심: 실행·운영 | run_research.ps1; daily_one_click.py; portfolio_backtest.py; logs/ | 새 Core CLI와 기존 일일 운영 경로. 현재 엔진 전환은 새 연구에 한정 | 운영 엔진은 아직 legacy |
| 추가 핵심: 이력·탐색 | progress.md; WORKSPACE.md; .rgignore | 상세 시간순 기록·짧은 파일 지도·대용량 검색 제외 | 전체 로그를 매번 읽지 않음 |

## 2. 핵심 흐름과 데이터 저장 위치

입력 스냅샷 → 시점별 피처/원본 신호 → JSON 조건 조합 → 공통 엔진 → 신호/체결/거절/미청산 원장
→ metrics.py → Scorecard → 통합 결과표와 사람용 보고서.

새 실행 묶음은 `experiments/<experiment_id>/<run_id>/`에 있다. 요청 구조의 results 역할을 한다.
`data/snapshots/<내용해시>/`는 실행이 참조하는 고정 입력으로, 원본 가격과 가공 피처가 manifest에 구분돼 있다.
`cache/prices/` 등은 기존 작업 캐시다. 오래된 조정 OHLC를 raw 가격으로 부르지 않는다.
향후 이동한다면 캐시는 data/raw, 피처/이벤트는 data/processed, 실행 결과는 results/runs에 대응할 수 있으나
현재 import·상대경로·운영 의존성을 먼저 점검해야 한다. 이름만 맞추기 위한 중복 복사는 하지 않았다.

## 3. 결과 파일 종류와 매매일지 구성

새 원장·결과의 공통 식별 필드: `source_project, source_family, strategy_code, strategy_name, version, strategy_id, experiment_id, run_id, comparison_key, parameters_hash, variant, parent_run_id, stage, synthetic`.
하나의 lot은 position_id/trade_instance_id, 신호는 signal_id로 연결한다.
같은 실험의 재실행도 run_id가 다르다. parameters_hash는 threshold/조합 변경을 추적한다.

| 파일 | 한 행의 단위 | 주요 정보 | 분석 시 의미 |
| --- | --- | --- | --- |
| features.csv / features.parquet (입력) | 피처 관측 1건 | ticker,date,feature,value,available_at,feature_version,source_event_id | threshold 적용 전 값. 당시 알 수 있는 시각과 원본 event ID 보존 |
| feature_evaluations.parquet | 실험의 ticker-day 1건 | date,ticker,signal_id,condition_passed,eligibility,signal_context | 필터 탈락도 포함. 모의 수익률은 넣지 않음 |
| signal_journal.parquet / .csv | 당일 유효 진입 후보 1건 | date,signal_date,ticker,signal_id,rank_score,entry_rank,signal_context,decision,position_id | FILLED/POSITION_LIMIT/INVALID/데이터상태 구분. 원본의 모든 피처와는 별도 |
| fills.csv | 진입 또는 청산 체결 1건 | date,ticker,position_id,signal_id,side,price_raw,price_split_adj,reason | ENTRY/EXIT 별도 행. broker 실거래가 아닌 백테스트 모의 체결 |
| trades.parquet | 포지션 lot 1건 | position_id,trade_instance_id,signal_id,signal_date,entry_date,exit_date,raw/split 가격,entry_context,entry_regime,status | COMPLETED와 OPEN_AT_END를 함께 보존하는 원본 |
| actual_trade_records.csv | 완료 왕복 거래 1건 | trades 필드 + exit_reason,holding_days,trade_return,holding_day_return | trades.parquet의 COMPLETED만 export. 신호 수익률과 혼합하지 않음 |
| open_positions_at_end.csv | 실제로 진입한 미청산 lot 1건 | position_id,signal_id,진입정보,last_backtest_date,last_valid_price_date,raw/split 최종가격,holding_days_to_end,status,exit_pending_reason | OPEN_AT_END. 완료 수익률 통계 제외, 자본 점유는 유지 |
| rejected_entry_candidates.csv | 슬롯 부족으로 거절된 후보-day 1건 | date,signal_date,ticker,signal_id,rank_score,available_slots,open_positions_count,rejection_reason | 같은 신호가 3일 거절되면 3건. 필터 탈락/invalid와 분리 |
| data_exceptions.csv | ticker-date 데이터 상태 1건 | date,ticker,exception_type,attempt_count,listing_date,listing_date_source,action_taken,note | NOT_YET_LISTED/INSUFFICIENT_HISTORY는 DATA_MISSING 오류 집계에서 제외 |
| engine_exceptions.csv | 계약·불변조건 위반 1건 | date,ticker,signal_id,position_id,exception_type,details,action_taken | 중복/잘못된 후보, 0일 보유 거래 등. 정상 통계와 분리 |
| daily_positions.csv | 시장 거래 세션 1건 | date,open_positions_count,used_capital,peak_same_ticker_positions,peak_ticker_concentration | 청산/진입 후 상태. 거래 없는 세션도 기록 |

### 일지 해석과 결합

- signal_date=신호 발생일, entry_date=엔진이 실제 진입을 모의 체결한 날. 날짜를 합치지 않는다.
- date=그 후보가 평가된 날짜. 지속 신호는 원래 signal_date를 유지하며 여러 date에 등장할 수 있다.
- ATR/RMSE/기간/조건 값은 signal_context 또는 entry_context의 values에, 원본 시각/버전은 origins에 남는다.
- 현재 FeatureCombinationStrategy의 조합 신호는 당일 재평가형이라 signal_date=date이며,
  오래된 원본 피처의 observation_date/source_event_id는 context에서 확인한다.
- 완료되지 않은 신호의 entry/exit 가격·수익률을 만들어 넣지 않는다. 가상 신호 성과 분석을 추가하면 별도 hypothetical 원장으로 둔다.
- run_id+position_id로 fills/positions를 결합하고, 후보-day 중복을 구분하려면 run_id+date+signal_id를 사용한다.
- 미청산만 있는 실행은 완료 거래 수가 0이어도 자본 사용량이 0이 아닐 수 있다.
- 시장 휴장일은 ticker 데이터 오류가 아니다. 상장 전·history 부족·DATA_MISSING을 구분한다.

### 결과표: 실행별 한 행

scorecard.csv가 계산 원본이고 experiments/strategy_results.csv는 같은 행들을 모은 비교표다.
전략·버전이 같아도 다른 run_id/파라미터/기간/데이터는 덮어쓰지 않는다.

| 항목 | 열/정의 |
| --- | --- |
| 거래 빈도 | trade_count, start_date, end_date, trades_per_year, short_backtest_warning |
| 거래 품질 | mean/median_trade_return, mean/median_holding_day_return, win_rate, average_win, average_loss |
| 보유기간 | mean/median_holding_days, holding_days_p25/p75/p90, max_holding_days |
| 꼬리 의존도 | top_5pct_contribution; 현재 공식 미정이므로 null + TOP_5PCT_FORMULA_UNDEFINED |
| 자본/기회 | average/peak_concurrent_positions, average/peak_used_capital, rejected_entry_candidate_events |
| 집중/품질 | peak_same_ticker_positions, peak_ticker_concentration, data_missing_events/tickers, open_positions_at_end |
| 진단표 | regime_metrics.csv(진입 국면), annual_metrics.csv(청산 연도), daily_positions.csv |
| 실행 근거 | definition.json, strategy_definition.md, run_config.json, schema.json, code_snapshot.zip |

trade_return = exit_price_split_adj / entry_price_split_adj - 1.
entry_price/exit_price 별칭은 split-only; 당시 raw 가격은 별도 열이다.
holding_days = 미국 정규장 trading-session distance(entry_date, exit_date), 완료 거래는 >=1.
holding_day_return = (1 + trade_return)^(1/holding_days) - 1.
수익률 저장 단위는 fraction(0.1=10%). 기간 분모는 전체 평가 종료일-시작일의 calendar span /365.25.
0건 카운트는 0, 표본 평균/비율은 null(보고서는 N/A). 승률은 양수 거래/완료 거래이고 0%는 분모에만 포함한다.
average_loss는 음수 거래 평균; 백분위는 선형 보간. 비용·슬리피지는 현재 Core 연구 모드에서 0이다.
CAGR/MDD/Sharpe/복리는 이 Core Scorecard의 평가 항목이 아니다. 이전 포트폴리오 결과와 분리한다.

## 4. 과거 전략·결과를 분류하고 연결한 방식

1. expanded_strategy_results의 401행과 신규 incoming 결과 31행을 원본 순서·출처 해시와 함께 색인했다.
2. 별도 미포함 62행을 같은 카탈로그에 연결했다. 이 중 50행은 미포함, 12행은 사용자 제외이며 모두 미시험은 아니다.
3. 그래서 legacy_catalog는 494행이다. 결과를 다시 계산하거나 새로 494개 전략을 실행한 것이 아니다.
4. 실제 매매일지의 source_project/family/code/name/version으로 그룹화해 94개 원장 묶음을 별도 색인했다.
5. 분산된 증거 파일 448개는 legacy_evidence_index에 원래 경로로 연결했다. 소스 105개는 history zip/해시로 보존했다.
6. canonical_version과 실제 원장 version이 다를 수 있어 equivalent_versions와 원본 필터를 확인한다.
   과거의 중복 제거 결과를 유지했으며, 이번 색인 작업이 모든 거래를 다시 중복 제거한 것은 아니다.
7. 과거 실행 시각이나 사전 가설이 없으면 미상으로 둔다. 파일 수정시각·관측 거래기간을 실행일로 바꾸지 않았다.
8. 구 PLANNED_ONLY 중 모멘텀·잔차 평균회귀·잔차 RMSE/PCA는 후속 실행이 있어 현재 표시를 보충했다.
   원본 CSV는 덮어쓰지 않았다. PEAD는 후속 기록에서도 DATA_BLOCKED다.

429개 결과 행에는 거래가 있고, 3개 PEAD 결과 행은 데이터 차단이다. 307개 Rotation-stop 행은
파라미터/정책 버전 연구로, 307개 독립 전략이 아니다. 여러 기간·비용·유니버스를 섞은 최고 성과 순위는 만들지 않았다.

## 5. AI 기록과 사람용 보고서

AI는 CURRENT_STATE → EXPERIMENTS/IDEAS → 관련 registry/원장만 읽는다. progress 전체나 모든 거래를 매번 읽지 않는다.
사람은 이 보고서의 검색 목록과 요약, 실행별 report.html/CSV/Excel을 본다. 두 경로는 같은 원본에서 나온다.
검색 목록의 source 경로는 프로젝트 내 위치 설명이며, 공개 저장소에 원본 가격·개별 매매 원장을 업로드했다는 뜻은 아니다.
공개 CSV에는 요청한 전략·버전·성과 요약과 상대경로를 담았다. 개별 매매 원장은 로컬 원본에서 찾는다.

## 6. 아직 끝나지 않은 전환

새 Core 엔진의 검증은 합성 데이터 기준이다. 실제 Primary는 raw/split-only 가격과 NYSE 캘린더 입력 문제로 DATA_BLOCKED다.
현재 운영은 기존 Next-Open/복리 포트폴리오 경로다. src 이동·전체 운영 엔진 교체·뉴스/금리 공급자 연결은 완료 상태가 아니다.
모든 비교는 SCREENING ONLY이며 현재 유니버스의 생존편향을 포함한다.
