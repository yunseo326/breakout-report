# EXPERIMENTS — 중복 실험을 피하기 위한 입구 (2026-09-14)

새 실험을 시작하기 전에 전략명뿐 아니라 source_project/family, code, version, 파라미터,
기간·유니버스·가격·체결 모델과 원장 근거를 비교한다. 같은 이름이 같은 실험을 뜻하지 않는다.

- 기존 결과표 401행 + 신규 잔차/추세선/모멘텀 31행 = 결과 기록 432행.
- 432행 중 거래가 있는 기록 429행, PEAD DATA_BLOCKED 기록 3행. 432개 독립 전략이 아니다.
- 결과표 미포함 목록 62행 = 미포함 50 + 사용자 제외 12. 이 중 TESTED 48, PARTIALLY_TESTED 4,
  PLANNED_ONLY 10은 원본 목록의 상태다. 전부 미시험 전략이 아니다.
- 구 계획 목록의 모멘텀/잔차 평균회귀/잔차 RMSE/PCA RMSE는 후속 시험 기록이 있어 IDEAS로 다시 넣지 않는다.
- 전체 494행의 원본 연결은 `experiments/legacy_catalog.csv`; 현재 상태 보충은 `reports/research-structure-assets/strategy-catalog.csv`.
- 실제 완료 원장의 정확한 전략·버전별 필터/행 수/해시: `experiments/legacy_ledger_index.csv` (94묶음).
- 새 Core 실행: `experiments/registry.csv`. 현재 6건은 합성 검증이며 전략 수익 검증이 아니다.

## 결과가 기록된 전략 식별자

아래는 원본 표기를 유지한 탐색 목록이다. 버전·기간·선택정책은 CSV에서 확인한다.
동일 code라도 프로젝트가 다르면 병합하지 않는다. 다른 이름/버전의 실제 동등성도 자동 추정하지 않는다.

| 프로젝트 | 코드 | 전략명 | 결과 행 |
| --- | --- | --- | --- |
| 2trading |  | Above Continuation + ATR Target | 1 |
| 2trading |  | Below Mean Reversion + ATR Fixed Stop | 1 |
| 2trading |  | Bollinger Above Continuation | 1 |
| 2trading |  | Bollinger Below Mean Reversion | 1 |
| 2trading |  | Channel AND Bollinger | 4 |
| 2trading |  | RANSAC/ATR Channel Above Continuation | 1 |
| 2trading |  | RANSAC/ATR Channel Below Mean Reversion | 1 |
| 2trading |  | RMSE Touch / Pullback / Double Pattern | 6 |
| 2trading |  | RMSE×2 + leading indicators | 4 |
| 2trading |  | Top5 × Channel AND/OR | 6 |
| 2trading | PCA_MR | PCA Residual Mean Reversion | 3 |
| 2trading | PCA_RMSE2 | PCA Residual RMSE2 | 3 |
| 2trading | PEAD_REV | PEAD + Estimate Revision | 3 |
| 2trading | RES_MOM | Residual Momentum | 3 |
| 2trading | RMSE2 | RMSE×2 하단 평균회귀 | 63 |
| 2trading | S13 | S13 NR7 돌파 | 81 |
| 2trading | S3 | S3 Darvas Box 돌파 | 72 |
| 2trading | S8 | S8 Larry Williams 변동성 돌파 | 73 |
| 2trading | S8 | S8 continuous exit walk-forward | 1 |
| 2trading | SN_MOM | 12-1 Sector-neutral Momentum | 3 |
| 2trading | SR_MR | Sector-factor Residual Mean Reversion | 2 |
| 2trading | SR_REV | Sector Residual Reversal Ranking | 3 |
| 2trading | SR_RMSE2 | Sector Residual RMSE2 | 2 |
| 2trading | TKFIB | 전환선·기준선 + 피보나치 (TKFIB) | 51 |
| 2trading | TRENDBREAK_REVERSE | Trendlines with Breaks 역추세 | 1 |
| 2trading | TRENDBREAK_TREND | Trendlines with Breaks 정추세 | 1 |
| 2trading | TREND_RET | Multiple-low Trend Retouch | 6 |
| 2trading | XS_MOM | Short Cross-sectional Momentum | 3 |
| imported_trading_project | A1 | A1_volatility_breakout | 1 |
| imported_trading_project | A2 | A2_timeseries_momentum | 1 |
| imported_trading_project | A3 | A3_cross_sectional_momentum | 1 |
| imported_trading_project | C1 | C1_mean_reversion | 1 |
| imported_trading_project | C2 | C2_support_resistance | 1 |
| imported_trading_project | C3 | C3_low_volatility | 1 |
| imported_trading_project | S1 | S1_gap_and_go | 1 |
| imported_trading_project | S10 | S10_turtle_soup_reversal | 1 |
| imported_trading_project | S11 | S11_golden_cross | 1 |
| imported_trading_project | S12 | S12_episodic_pivot | 1 |
| imported_trading_project | S13 | S13_nr7_breakout | 1 |
| imported_trading_project | S14 | S14_adx_trend_breakout | 1 |
| imported_trading_project | S15 | S15_week52_low_reversal | 1 |
| imported_trading_project | S16 | S16_turn_of_month | 1 |
| imported_trading_project | S17 | S17_fibonacci_retracement_bounce | 1 |
| imported_trading_project | S18 | S18_pre_holiday_effect | 1 |
| imported_trading_project | S19 | S19_double_bottom_breakout | 1 |
| imported_trading_project | S2 | S2_vcp_breakout | 1 |
| imported_trading_project | S20 | S20_inverse_head_and_shoulders | 1 |
| imported_trading_project | S21 | S21_rsi2_sma200_connors_exit | 1 |
| imported_trading_project | S22 | S22_rsi_adx_ibs | 1 |
| imported_trading_project | S23 | S23_momentum_12_1 | 1 |
| imported_trading_project | S24 | S24_week52_high_proximity | 1 |
| imported_trading_project | S25 | S25_macd_rsi_sma200 | 1 |
| imported_trading_project | S26 | S26_rsi14_sma200_dip | 1 |
| imported_trading_project | S3 | S3_darvas_box | 1 |
| imported_trading_project | S4 | S4_rsi2_reversion | 1 |
| imported_trading_project | S5 | S5_high_tight_flag | 1 |
| imported_trading_project | S6 | S6_new_high_momentum | 1 |
| imported_trading_project | S7 | S7_ttm_squeeze | 1 |
| imported_trading_project | S8 | S8_lw_volatility_breakout | 1 |
| imported_trading_project | S9 | S9_ema_pullback_continuation | 1 |
