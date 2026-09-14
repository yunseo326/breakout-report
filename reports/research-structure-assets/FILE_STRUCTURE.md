# 2trading 핵심 파일 구조와 연구 기록 (2026-09-14)

소스는 현재 2trading 디렉터리다. 아래는 요청한 구조와 **현재 실제 위치의 대응표**이며,
src/raw/processed로 이동을 끝냈다는 뜻이 아니다. 파일·코드·원장은 기존 위치에 유지했다.
CURRENT_STATE/EXPERIMENTS/IDEAS는 이번에 실제 안내 문서로 추가했다.

## 1. 요청 구조 → 현재 위치

각 행은 파일 또는 폴더 하나의 역할을 설명한다. `*`는 여러 파일에 적용되는 패턴,
`<experiment_id>`·`<run_id>`·`<hash>`는 실행별로 달라지는 경로 자리다.
같은 파일이 여러 요청 역할에 등장하면 역할 연결을 보여주기 위해 반복 표시한 것이다.

| 요청한 역할 | 현재 파일/폴더 | 개별 파일·폴더의 역할 | 상태 |
| --- | --- | --- | --- |
| AGENTS.md | AGENTS.md | AI 작업의 목적·기록 원칙과 현재 안내 문서의 읽는 순서를 지정한다. | 유지 |
| CURRENT_STATE.md | CURRENT_STATE.md | 지금 구현된 기능, 검증 범위, 실데이터 평가의 차단 요인과 다음 작업을 짧게 요약한다. | 이번에 안내 문서 추가 |
| CURRENT_STATE.md | WORKSPACE.md | 주요 문서·실행 명령·폴더로 이동하기 위한 짧은 파일 지도다. | 이번에 안내 문서 추가 |
| CURRENT_STATE.md | experiments/profiles/primary_v1.json | Primary 평가의 기간·자본·포지션 수·체결 모델을 담은 설정 후보다. 현재 DATA_BLOCKED이며 입력 스냅샷은 미지정이다. | 이번에 안내 문서 추가 |
| EXPERIMENTS.md | EXPERIMENTS.md | 시험한 전략의 이름·코드를 빠르게 찾는 목록이다. 상세 실행 조건은 연결된 CSV에서 확인한다. | 이번에 안내 문서 추가 |
| EXPERIMENTS.md | experiments/registry.csv | 새 Core 실행을 run_id별로 조회하는 인덱스다. 실험 식별자·설정·스냅샷·결과 경로를 연결한다. | 이번에 안내 문서 추가 |
| EXPERIMENTS.md | experiments/legacy_catalog.csv | 과거 전략·버전·결과와 미포함 목록을 원본 출처와 함께 모은 색인이다. 동일 조건으로 재계산한 결과표는 아니다. | 이번에 안내 문서 추가 |
| EXPERIMENTS.md | experiments/legacy_ledger_index.csv | 과거 매매 원장을 전략·버전별로 찾기 위한 위치·필터·행 수·해시 목록이다. | 이번에 안내 문서 추가 |
| IDEAS.md | IDEAS.md | 앞으로 검토할 아이디어를 미시험·데이터 차단·정의 불명확 상태로 구분한다. | 이번에 안내 문서 추가 |
| IDEAS.md | question.md | 결정하지 못한 규칙·데이터·지표 질문을 보관한다. 오래된 계획 표시는 후속 실험 기록과 대조한다. | 이번에 안내 문서 추가 |
| results/ | results/ | 기존 연구와 일일 운영의 결과 CSV·Parquet, 후보·피처·매매 원장이 모여 있다. 실행별로 완전히 분리된 구조는 아니다. | 역할 대응; 이동하지 않음 |
| results/ | incoming_experiments/ | 기존·외부 연구에서 받아 표준화한 실험 묶음과 전략 정의·결과를 보관한다. | 역할 대응; 이동하지 않음 |
| results/ | experiments/<experiment_id>/<run_id>/ | 새 Core 실행 한 번의 독립 저장 폴더다. 설정·코드 스냅샷·일지·Scorecard·보고서를 함께 보관한다. | 역할 대응; 이동하지 않음 |
| reports/ | reports/ | 사람이 읽는 HTML·Markdown 보고서와 공개 보고서용 자산을 보관한다. | 역할 대응; 이동하지 않음 |
| reports/ | experiments/<experiment_id>/<run_id>/report.html | 해당 Core 실행 한 건의 결과를 사람이 읽도록 요약한 보고서다. | 역할 대응; 이동하지 않음 |
| reports/ | outputs/ | 기존 분석에서 만든 Excel 등 별도 전달용 산출물을 보관한다. | 역할 대응; 이동하지 않음 |
| reports/ | artifact/ | 기존 스크리너 HTML 등 이전 방식의 화면 산출물을 보관한다. | 역할 대응; 이동하지 않음 |
| data/raw/ | cache/prices/ | 기존 가격 수집 함수가 재사용하는 종목별 가격 캐시다. 가격 조정 방식은 생성 설정을 확인해야 한다. | raw 폴더는 아직 없음 |
| data/raw/ | cache/prices_long/ | 장기간 연구에서 재사용하는 가격 캐시다. 일반 가격 캐시와 기간·내용이 같은지 확인 후 사용한다. | raw 폴더는 아직 없음 |
| data/raw/ | cache/universe/ | 수집한 S&P 500 종목 목록의 스냅샷을 보관한다. 현재 구성종목 목록은 과거 시점별 구성종목을 보장하지 않는다. | raw 폴더는 아직 없음 |
| data/raw/ | cache/earnings/ | 기존 실적 관련 수집 자료를 재사용하기 위한 캐시다. 당시 consensus·수정 이력을 모두 보유한다는 뜻은 아니다. | raw 폴더는 아직 없음 |
| data/raw/ | cache/market_cap/ | 시가총액 관련 수집 자료의 캐시다. 과거 시점에 알 수 있었던 값인지 별도 확인이 필요하다. | raw 폴더는 아직 없음 |
| data/raw/ | data/snapshots/ | 새 Core가 참조하는 고정 입력 묶음을 내용 해시별로 보관한다. 가격·세션·피처와 manifest를 함께 관리한다. | raw 폴더는 아직 없음 |
| data/processed/ | results/events_*.parquet | 기존 이벤트 생성 단계가 계산한 돌파 등 이벤트를 저장하는 파일 패턴이다. | processed 폴더는 아직 없음 |
| data/processed/ | results/features_*.parquet | 기존 피처 생성 단계가 계산한 분석·선별용 피처를 저장하는 파일 패턴이다. | processed 폴더는 아직 없음 |
| data/processed/ | results/*candidates*.csv | 각 연구·운영에서 선별한 후보 목록의 파일 패턴이다. 후보는 실제 체결을 의미하지 않는다. | processed 폴더는 아직 없음 |
| data/processed/ | data/snapshots/<hash>/features.* | 해당 고정 입력 묶음의 피처 관측값·가용시각·버전·원본 이벤트 ID를 담는다. | processed 폴더는 아직 없음 |
| src/engine/ | strategy_evaluation/engine.py | 거래 세션을 순회하며 전략 호출, 청산·진입 처리, 슬롯 제한, 포지션 상태와 일지를 관리하는 공통 Core 엔진이다. | src 폴더는 아직 없음 |
| src/engine/ | strategy_evaluation/config.py | 평가 기간·기준 자본·포지션 크기·동시 보유 한도 등 엔진 공통 설정을 정의하고 검사한다. | src 폴더는 아직 없음 |
| src/engine/ | strategy_evaluation/interfaces.py | 가격·후보·청산 결정·포지션 자료형과 Strategy/DataProvider가 지켜야 할 인터페이스를 정의한다. | src 폴더는 아직 없음 |
| src/data/ | strategy_evaluation/data_provider.py | 엔진에 가격·세션·종목 메타데이터를 제공한다. 메모리 자료와 기존 Parquet 캐시를 읽는 구현을 포함한다. | 역할 대응 |
| src/data/ | strategy_evaluation/snapshot.py | manifest와 입력 파일을 읽고 가격 기준·캘린더·피처 등 스냅샷 계약을 검사해 실행 입력을 구성한다. | 역할 대응 |
| src/data/ | data_fetch.py | 기존 가격 다운로드·컬럼 정리·여러 시간 단위 조회·로컬 캐시 재사용과 증분 갱신을 담당한다. | 역할 대응 |
| src/data/ | universe.py | S&P 500 종목 목록을 수집·캐시하고 데이터 수집에 사용할 티커 형식과 일일 사용 목록을 만든다. | 역할 대응 |
| src/data/ | fetch_daily_prices.py | 일일 가격 갱신을 실행하고 실패 종목 등 가격 품질 결과를 저장하는 명령행 진입점이다. | 역할 대응 |
| src/features/ | features/ | 기존 분석에서 사용하는 개별 피처 계산 모듈을 담는다. 체결·포지션을 관리하는 엔진과 구분된다. | 역할 대응 |
| src/features/ | events/ | 가격에서 돌파 등 이벤트를 감지하는 모듈을 담는다. 이벤트 발생 자체가 포트폴리오 체결을 뜻하지 않는다. | 역할 대응 |
| src/features/ | strategy/research_strategies.py | 잔차·모멘텀·추세선 등 기존 신규 전략 연구의 신호 계산 로직을 모은다. 새 Core 공통 엔진 자체는 아니다. | 역할 대응 |
| src/features/ | strategy_evaluation/combinations.py | 저장된 피처를 JSON의 AND/OR/NOT·임계값 조건으로 결합하고 진입 후보·청산 판단과 조건 평가 기록을 만든다. | 역할 대응 |
| src/results/ | strategy_evaluation/storage.py | 실행 식별자·입력/코드 해시를 기록하고 일지·설정·스키마·보고서를 저장하며 실행 인덱스와 결과표를 갱신한다. | 역할 대응 |
| src/results/ | strategy_evaluation/metrics.py | 완료 거래와 일별 포지션·거절·데이터 예외에서 거래 지표, Scorecard, 연도·국면별 통계를 계산한다. | 역할 대응 |
| src/results/ | strategy_evaluation/regime.py | 전략 평가에 사용하는 시장 국면을 분류한다. 거래의 진입 국면을 기록하고 국면별 진단에 연결한다. | 역할 대응 |
| src/reporting/ | report_design.py | 기존 HTML 보고서가 재사용하는 스타일과 표·표시용 도우미를 제공한다. | 과거 개별 builder 포함 |
| src/reporting/ | publish_report.py | 지정 보고서를 공개 저장소에 복사하고 reports.json과 보고서 목록 index.html을 갱신·발행한다. | 과거 개별 builder 포함 |
| src/reporting/ | strategy_evaluation/documentation.py | 현재 파일 대응표·연구 안내·과거 전략 검색 HTML과 공개용 CSV/JSON/Markdown을 생성한다. 지금 보는 안내의 생성기다. | 과거 개별 builder 포함 |
| src/reporting/ | build_*report.py | 각 분석 주제의 결과를 읽어 해당 HTML 보고서를 만드는 기존 개별 스크립트들의 파일 패턴이다. | 과거 개별 builder 포함 |
| src/validation/ | strategy_evaluation/validation.py | 검증 경계를 넘어 확정되는 학습 라벨을 훈련 자료에서 제외하는 purged_split 함수를 제공한다. 모든 편향 검사를 자동 해결하지는 않는다. | tests는 독립 폴더 유지 권장 |
| src/validation/ | tests/ | 엔진·데이터 처리·운영·보고서 등 기능별 동작을 검증하는 테스트를 보관한다. | tests는 독립 폴더 유지 권장 |
| 추가 핵심: strategy/ | strategy/ | ml_regression_trend·trendlines_with_breaks 등 전략 고유 계산을 보관한다. strategy_evaluation/의 공통 실행·저장과 역할이 다르다. | 유지 |
| 추가 핵심: experiments/ | experiments/examples/ | JSON 조합·파라미터 변형과 합성 입력 등 새 연구 실행 방식을 확인할 예제를 담는다. | 유지 |
| 추가 핵심: experiments/ | experiments/profiles/ | 반복 평가에 사용할 공통 조건 프로필을 보관한다. 각 프로필의 활성·차단 상태를 확인해야 한다. | 유지 |
| 추가 핵심: experiments/ | experiments/registry.csv | 새 Core 실행을 run_id별로 조회하는 인덱스다. 실험 식별자·설정·스냅샷·결과 경로를 연결한다. | 유지 |
| 추가 핵심: history/ | history/README.md | 과거 소스 보관본의 의미와 복원 시 필요한 입력·의존성을 설명한다. | 삭제 대신 보존 |
| 추가 핵심: history/ | history/source_inventory.csv | 보관한 소스의 원래 경로·역할·보관본 위치를 연결하는 목록이다. | 삭제 대신 보존 |
| 추가 핵심: history/ | history/legacy_sources_*.zip | 통합 전 소스를 보존한 압축본의 파일 패턴이다. 같은 이름의 JSON에서 파일 해시를 확인한다. | 삭제 대신 보존 |
| 추가 핵심: docs/ | docs/strategy_evaluation/WORKFLOW.md | 현재 Core 연구의 실행 순서, 입력·전략·출력 계약과 미해결 사항을 설명한다. | 유지 |
| 추가 핵심: docs/ | docs/strategy_evaluation/reference/ | Core Spec·Glossary·Result Output 기준 문서 원문을 보관한다. 구현 설명과 구분해 참조한다. | 유지 |
| 추가 핵심: 실행·운영 | run_research.ps1 | 새 Core 연구 명령을 Python 모듈에 전달하는 PowerShell 실행 진입점이다. | 운영 엔진은 아직 legacy |
| 추가 핵심: 실행·운영 | daily_one_click.py | 기존 일일 가격·이벤트·피처·대시보드 갱신과 품질 검사를 순서대로 실행하는 운영 파이프라인이다. | 운영 엔진은 아직 legacy |
| 추가 핵심: 실행·운영 | portfolio_backtest.py | 기존 거래 원장으로 포트폴리오 배분과 일별 평가·수익을 모의 계산한다. 새 Core의 고정 포지션 연구 엔진과 계산 계약이 다르다. | 운영 엔진은 아직 legacy |
| 추가 핵심: 실행·운영 | logs/ | 기존 실행·예약 작업 등의 로그를 보관해 실패 원인을 추적한다. 정식 실험 인덱스를 대신하지 않는다. | 운영 엔진은 아직 legacy |
| 추가 핵심: 이력·탐색 | progress.md | 수행한 작업과 검증 결과를 시간순으로 누적하는 상세 이력이다. | 전체 로그를 매번 읽지 않음 |
| 추가 핵심: 이력·탐색 | WORKSPACE.md | 주요 문서·실행 명령·폴더로 이동하기 위한 짧은 파일 지도다. | 전체 로그를 매번 읽지 않음 |
| 추가 핵심: 이력·탐색 | .rgignore | 검색 시 대용량·재생성·임시 경로를 제외해 불필요한 검색 결과와 AI 입력량을 줄인다. | 전체 로그를 매번 읽지 않음 |

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
