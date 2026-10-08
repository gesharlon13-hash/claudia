# Momentum Burst MAX + Ghost Machine

| File | What it is |
|---|---|
| `momentum_burst_max_strategy.pine` | **Momentum Burst MAX** strategy (Pine v6), with the Ghost Machine momentum engine added as an opt-in layer |
| `momentum_burst_max.pine` | A separate panel indicator that shows the Ghost Machine engine with burst markers. Use it next to the strategy to see what the Ghost filter sees. |

## The upgrade (strategy)

ChartPrime's **Momentum Ghost Machine** engine (MPL-2.0) is ported into the strategy. It computes:

- **Momentum line:** price minus a Blackman-windowed sinc low-pass filter.
- **MA** of that momentum line.
- **Histogram:** momentum minus MA.
- **Ghost:** the histogram projected 2 bars ahead.

It is used on the **burst leg only**. The dip and engulfing legs are untouched.

| Input (group "Ghost Machine momentum") | Effect |
|---|---|
| Entry filter = `Histogram` | Long bursts need histogram > 0. Short bursts need histogram < 0. |
| Entry filter = `Histogram + ghost` | The histogram and the 2-bar projection must both be on the trade's side. |
| Entry filter = `Momentum + histogram` | The histogram and the momentum line must both be on the trade's side. |
| Ghost exit = `Any` / `Only in profit` | Closes a burst trade at the bar close when the projected histogram crosses zero against it. |
| Mark bursts the Ghost filter blocks | Shows gray triangles where a burst fired but the filter blocked it. |

Rules:

- A burst that the filter blocks still outranks the dip and engulfing legs on that bar. The filter only removes trades and never swaps one trade for another.
- During warm-up, when there isn't enough data yet, the filter passes. The OI filter works the same way.
- With both inputs set to **Off** (the default), every signal, preset, exit and alert is identical to Momentum Burst XE.

## Not validated yet

The container this was built in can't reach exchange data, so none of these options has been backtested. Before switching them on live, run them through the same gate as the other upgrades:

- 7/7 metrics on the TradingView grid and on the half-bar-shifted grid
- At least 5/7 on both held-out grids
- No basket line gets worse

Sweep the 3 filter modes × 3 exit modes on each coin with the default engine settings first, then try the momentum length in steps of 25–50.
