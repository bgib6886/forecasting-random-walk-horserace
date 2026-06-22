# Can You Beat a Random Walk? A Forecasting Horse-Race

**TL;DR:** The "random walk beats everything" result from forecasting folklore
(Meese & Rogoff, 1983) only holds when the data really *is* a random walk.
This project runs a controlled simulation showing that when the true process
is stationary (AR, MA, ARMA), correctly specified models beat the random walk
benchmark by **15-30% on average** — and by over **50%** in some setups. The
random walk's reputation, it turns out, has as much to do with the kind of
data economists usually study as with any deep forecasting superiority.

## Why this matters

The random walk model — "tomorrow's value = today's value plus noise" — is
a famously hard benchmark to beat in real-world economic forecasting,
especially for exchange rates. This is often read as evidence that markets
are efficient and unpredictable. But that conclusion is entangled with a
separate fact: a lot of real economic data (like exchange rates) genuinely
behaves like a random walk. So is the random walk winning because it's a
brilliant forecasting strategy, or because the data happens to suit it?

This project answers that by removing the ambiguity: we simulate data where
we **know** the true underlying process, then run a forecasting competition
("horse-race") between the random walk and models that try to exploit the
known structure. If the random walk still wins even when we've stacked the
deck against it, that's real evidence of its strength. If it loses, that
tells us its real-world reputation is more about the data than the method.

## What we did

1. **Simulated six processes** (500 observations each): AR(1), AR(5), MA(1),
   MA(5), ARMA(2,2), and a random walk — covering a spread of realistic
   time-series dynamics used in macro and finance.
2. **Verified the simulations behaved as expected** using ADF and KPSS
   stationarity tests, and ACF/PACF diagnostics — the standard toolkit an
   analyst would use on real data before modelling it.
3. **Ran an eight-model forecasting competition** on each simulated series:
   five fixed ARIMA specifications, the random walk benchmark, an automated
   AIC-based model selector, and a simple model-average ensemble.
4. **Tested robustness across three forecasting designs** — recursive
   (expanding) windows, rolling (fixed-length) windows, and fixed-origin
   (train-once) windows — since how you forecast can matter as much as what
   model you use.
5. **Evaluated everything on RMSE, MAE, and MSE** to check whether
   conclusions were sensitive to how forecast errors are penalised.

Full code and output: [`arma_vs_random_walk_horserace.ipynb`](./arma_vs_random_walk_horserace.ipynb)

## Key results

| Process type | Best non-RW model beats RW by | Notes |
|---|---|---|
| AR(1) | 6-34% | Modest gains; closer to RW when persistence is low |
| AR(5) | 21-23% | Bigger gains — RW ignores more lag structure to exploit |
| MA(1) | 18-51% | Largest single improvement, especially in fixed-origin design |
| MA(5) | 11-22% | Consistent, moderate gains |
| ARMA(2,2) | 0-20% | Near-zero gains under fixed-origin at longer horizons |
| **Random walk (true RW data)** | **~0%** | **No model meaningfully beats RW when the data really is a random walk** |

**The headline result:** when the underlying process is genuinely
unpredictable (a true random walk), nothing beats it — confirming the
classic result. But the moment there's *any* real structure in the data
(even subtle, higher-order AR/MA dynamics), models that exploit that
structure reliably and substantially outperform the "just guess yesterday's
value" approach. The random walk's real-world dominance says more about
the kind of data it's usually tested on (genuinely close to a random walk)
than about forecasting superiority in general.

A secondary finding: **forecasting design matters as much as model choice**
— recursive and rolling re-estimation consistently beat a "fit once and
extrapolate" approach, especially as forecast errors compound over longer
unrevised horizons.

## Methodology notes

- All data is **simulated** within the notebook itself (via `statsmodels`'
  `ArmaProcess`) — there's no external dataset dependency, so the notebook
  runs end-to-end with no setup beyond installing the listed packages.
- Models compared: AR(1), AR(5), MA(1), MA(5), ARMA(2,2) (fixed
  specifications), an AIC-selected automated ARIMA, a simple equal-weight
  model average, and the random walk benchmark.
- Forecast accuracy evaluated via RMSE, MAE, and MSE across all three
  forecasting window designs.

## Tools

Python — `statsmodels` (ARIMA, ArmaProcess, ADF/KPSS tests), `pmdarima`
(automated ARIMA selection), `pandas`, `numpy`, `matplotlib`, `seaborn`.

## About this project

Completed as a group project for ECON5119 (MSc Data Analytics for Economics
and Finance, University of Glasgow) with group members 3091587L and
3118182D. Published here with their agreement; cleaned up and documented
for portfolio purposes.

Full written report (methodology and results in academic format) available
on request.

---
*Benjamin Gibson — [LinkedIn](#) — [email](#)*
