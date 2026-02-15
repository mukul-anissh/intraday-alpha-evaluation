# intraday-alpha-evaluation
Cost aware evaluation of short horizon momentum signal using 1 minute bars and realistic turnover assumptions.

## Motivation
Many apparent intraday relationships disappear once trading friction is considered.
This project builds a minimal but disciplined pipeline to test that.

## Research Question
Does recent short term price movement contain information about near future return, and can it survive realistic transaction costs?

## Data
Instrument: BTCUSDT

Venue: Binance

Source: L1 order book updates

Aggregation: resampled into 1 minute bars

Trading day: single 24h period (14th February 2024)

## Feature Construction
### Past Return (predictor)
$$
\text{Past Return}_t = \frac{\text{close}_t - \text{close}_{t-L}}{\text{close}_{t-L}}
$$

### Future Return (outcome)
$$
\text{Future Return}_t = \frac{\text{close}_{t+H} - \text{close}_{t}}{\text{close}_t}
$$

where 
- L = Lookback window (5 minutes)
- H = Horizon window (5 minutes)

All calculations use forward shifts to avoid leakage. 

## Preliminary Evidence
Correlation between past and future returns is positive but small, suggesting weak momentum behavior typical in liquid markets.
Bucketed analysis shows mild directional tilt but significant noise.

## Trading Rule (Test)
- Long when past return in top quantile
- Short when in bottom quantile
- Otherwise flat

Positions are evaluated using close to close returns.

## Cost Model
We assume proportional transaction cost per unit traded (basis point style).
Costs are applied only when position changes, approximating entry/exit friction.

## Results
### Gross (before costs)
A slight positive correlation is present, but cumulative performance is unstable and trends negative over the sample.

### Net (after costs)
Incorporating turnover based costs further degrades performance, confirming the signal lacks economic viability.

Average net return ≈ negative.
Sharpe ≈ near zero (slightly negative).

## Interpretation
While a statistical relationship exists, it is too weak relative to turnover.

## Limitations
- single day
- simple thresholds
- no dynamic sizing
- no impact model
- ignores spread microstructure

## Future Work
- multi-day validation
- alternative horizons
- volatility conditioning
- reduced turnover designs
- combining with other features

## Reproducibility
Run notebooks in order:
1. build_bars.ipynb
2. signal_research.ipynb

## Takeaway
Although a mild predictive relationship exists, realistic trading frictions render the signal non viable, highlighting the gap between correlation and tradability.
