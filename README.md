# Trader Behavior Insights — Performance vs Bitcoin Market Sentiment

Analysis of the relationship between trader performance (Hyperliquid historical trade data) and Bitcoin market sentiment (Fear & Greed Index), with patterns and recommendations for smarter trading strategies.

**Candidate:** Rhushikesh Sangle

## Repository structure

```
ds_rhushikesh/
├── notebook_1.ipynb      # Full analysis (executed, with all outputs visible)
├── csv_files/
│   ├── historical_trader_data.csv   # Hyperliquid trades (211,224 rows, 32 accounts, May 2023 – May 2025)
│   └── fear_greed_index.csv         # Daily Bitcoin Fear & Greed Index (2018 – 2025)
├── outputs/
│   ├── dashboard.png                # 4-panel regime comparison
│   ├── timeline.png                 # Cumulative PnL shaded by sentiment regime
│   ├── perf_by_sentiment.csv        # Aggregated performance metrics per regime
│   └── daily_pnl_sentiment.csv      # Daily PnL joined with sentiment
├── ds_report.pdf         # Written report: methodology, findings, strategy implications
└── README.md
```

## Method

Each trade is merged to the Fear & Greed classification of its calendar day (211,218 / 211,224 trades matched). Performance metrics (PnL, win rate, payoff ratio, efficiency) use closing trades (Closed PnL ≠ 0, n = 104,402); behavioral metrics (volume, positioning, sizing) use all trades. Headline differences are validated with Mann-Whitney U, two-proportion z, and Spearman tests.

## Key findings

1. **Profits live at the sentiment extremes, not in the middle.** PnL per active day: Extreme Fear $53.8k > Fear $45.4k > Extreme Greed $25.1k ≈ Neutral $23.5k > Greed $12.9k. The relationship is U-shaped — daily PnL has ~zero linear correlation with the raw index value.
2. **Extreme Greed is the highest-quality regime** (89% win rate, payoff ratio 1.34, $46.8 PnL per $1k traded — 3x any other regime). **Extreme Fear is the highest-risk regime** (76% win rate, average loss 1.5x average win); its big totals are brute-forced by 7.5x higher daily volume.
3. **The long/short edge flips with the regime.** Shorts earn most in Fear ($208/trade, 86% win); longs dominate Extreme Greed ($176/trade, 91% win) while shorts there collapse to $29/trade. Worst configuration: shorting plain-Greed days.
4. **These traders are contrarian — and it paid.** Long share of opening volume falls monotonically from 75% (Extreme Fear) to 53% (Extreme Greed). The cohort was flat through Greed-heavy 2024 and earned nearly all of its $10.3M during the Fear-heavy Dec 2024 – May 2025 stretch.
5. **Regime-change days are the money days:** ~2x the PnL of stable days; days flipping into Fear average $60k.
6. **Win rate differs significantly between Fear and Greed days (p ≈ 1e-14); mean per-trade PnL does not (p = 0.53)** — regimes differ in loss-tail shape, not average outcome.

## Strategy implications

- Trade the extremes, fade the middle (plain Greed days are the weakest regime by every metric).
- Match direction to regime: short into Fear, ride longs in Extreme Greed, never short the melt-up.
- Risk-manage Extreme Fear specifically — it is the only regime where losses outsize wins.
- Watch sentiment-regime flips, especially into Fear: the most profitable single days.

## Reproducing

```bash
git clone https://github.com/rhushi8/ds_prime.git
cd ds_prime
pip install pandas numpy scipy matplotlib
jupyter notebook notebook_1.ipynb   # paths are relative to the repo root
```

Or open `notebook_1.ipynb` in Google Colab and uncomment the `git clone` lines in the first cell.

## Caveats

32 accounts; top 3 hold 45.7% of PnL (regime pattern survives their exclusion). Coin mix is confounded with time/regime. Sentiment is BTC-based but applied to all coins. Closed PnL excludes fees (~3% of PnL). No leverage column in this export.
