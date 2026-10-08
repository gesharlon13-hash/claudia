# Momentum Burst Max [Ghost Engine]

TradingView Pine Script v5 indicator: `momentum_burst_max.pine`.

It runs on ChartPrime's **Momentum Ghost Machine** engine (MPL-2.0), which uses a
windowed-sinc low-pass filter for momentum, an MA, a convergence/divergence
histogram, and a 2-bar "ghost" projection. On top of that it adds burst detection.

## What's added

| Feature | How it works |
|---|---|
| **Burst** | The histogram (`momentum - MA`) is measured in standard deviations of its own recent history (`Burst Lookback`). A burst fires when it moves past `Burst Threshold σ` while still accelerating and lined up with momentum polarity. |
| **MAX Burst** | Fires the first time in a burst leg that the strength beats the strongest reading of the previous `MAX Burst Lookback` bars. |
| **Exhaustion** | Fires on the first bar the histogram turns against a burst that is still extended. |
| **Ghost Cross** | An early warning: the projected histogram crosses zero before the real histogram does. |
| **Ghost Confirmation** | Optional filter. Bursts only count when the projection keeps extending them. |
| **Cooldown** | Sets the minimum number of bars between two same-direction bursts. |
| **Chart signals** | Markers on the price chart (`force_overlay`), burst candle coloring, and burst background shading. |
| **Dashboard** | Shows momentum direction, position vs. MA, burst σ, state, ghost direction, and the last signal. |
| **Alerts** | One `alertcondition` per signal, plus an "Any alert() function call" alert that covers all of them. |

## Engine fixes

- The ghost-projection linefill is now created once and reused. Before, a new one was created every bar.
- The momentum fill is now drawn between the main MA and momentum lines, so it still shows with Glow turned off.

## Usage

Paste `momentum_burst_max.pine` into the TradingView Pine Editor and click **Add to chart**.
Signals confirm on bar close, so set alerts to "Once Per Bar Close".
