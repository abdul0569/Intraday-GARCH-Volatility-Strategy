# Intraday GARCH Volatility Trading Strategy

A quantitative intraday strategy combining GARCH(1,3) volatility forecasting with RSI/Bollinger Band mean reversion signals on 5-minute data.

## Strategy Overview

**Two-Layer Signal System:**
1. **Daily GARCH Layer** → Predicts volatility to identify market regime (high/low vol)
2. **Intraday Mean Reversion** → RSI + Bollinger Bands for entry/exit timing

**Core Logic:**
- **High volatility regime** + Overbought → **SHORT** (fade the spike)
- **Low volatility regime** + Oversold → **LONG** (buy the dip)

## Key Components

### Volatility Premium Signal
```
Premium = (Predicted Vol - Realized Vol) / Realized Vol

IF premium > 1 std → High vol expected → Short bias
IF premium < -1 std → Low vol expected → Long bias
```

### Signal Combination
| Daily Signal | Intraday Signal | Position |
|--------------|-----------------|----------|
| +1 (High Vol) | +1 (RSI>70, Price>Upper BB) | **SHORT** |
| -1 (Low Vol) | -1 (RSI<30, Price<Lower BB) | **LONG** |
| Other | Any | **FLAT** |

## Methodology

1. **Fit GARCH(1,3)** on 180-day rolling window of daily log returns
2. **Forecast next-day variance** and calculate volatility premium
3. **Generate daily signal** when premium exceeds ±1 std threshold
4. **Calculate intraday signals** using RSI(20) and Bollinger Bands(20) on 5-min bars
5. **Combine signals** and execute trades when both align
6. **Aggregate returns** to daily level


