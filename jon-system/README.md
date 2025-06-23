# JON MTP System - Universal Higher Timeframe Strategy

## Overview

The MTP (Monthly Trend Performance) system has been upgraded to a **Universal Higher Timeframe (HTF) System** that works across ALL asset classes and timeframes. No longer locked to monthly data, you can now configure any higher timeframe for swing level detection.

## Key Features

✅ **Universal Compatibility**: Works with crypto, indices, futures, metals, forex  
✅ **Configurable HTF**: Separate breakout and dip timeframes (1D, 1W, 1M, etc.)  
✅ **Age Validation**: HTF levels must be properly aged (configurable min/max bars)  
✅ **Smart Guardrails**: Warns if HTF < baseTF or insufficient history  
✅ **Reference Asset**: Optional macro trend confirmation  
✅ **Extended ATR**: 100-period ATR for stability  
✅ **Wide Trailing**: 22.5 ATR distance for major trend capture  

## HTF Preset Recommendations

### Crypto Assets (24/7 Markets)
```pinescript
// Recommended Settings for BTC, ETH, etc.
Breakout HTF: "1M" (Monthly)
Dip HTF: "1M" (Monthly)
Base TF: "1D" (Daily chart)
Min Age: 20 bars, Max Age: 200 bars
```
**Rationale**: Crypto volatility requires monthly resolution to avoid noise

### US Equities & ETFs (SPY, QQQ, etc.)
```pinescript
// Recommended Settings for Traditional Markets
Breakout HTF: "1W" (Weekly)
Dip HTF: "1W" (Weekly)  
Base TF: "1D" (Daily chart)
Min Age: 15 bars, Max Age: 150 bars
```
**Rationale**: Traditional markets have lower volatility, weekly levels are significant

### Futures (NQ, ES, CL, etc.)
```pinescript
// Recommended Settings for Futures Contracts
Breakout HTF: "1D" (Daily) for intraday, "1W" for position
Dip HTF: "1D" (Daily) for intraday, "1W" for position
Base TF: "1h" or "4h" for intraday, "1D" for position
Min Age: 10 bars, Max Age: 100 bars
```
**Rationale**: Futures allow flexible HTF based on trading style

### Metals (Gold, Silver, etc.)
```pinescript
// Recommended Settings for Precious Metals
Breakout HTF: "1W" (Weekly)
Dip HTF: "1W" (Weekly)
Base TF: "1D" (Daily chart)  
Min Age: 15 bars, Max Age: 120 bars
```
**Rationale**: Metals have longer-term trends, weekly resolution captures structure

### Forex (EUR/USD, GBP/USD, etc.)
```pinescript
// Recommended Settings for Currency Pairs
Breakout HTF: "4H" (4-Hour) for intraday, "1D" for swing
Dip HTF: "4H" (4-Hour) for intraday, "1D" for swing
Base TF: "15m" or "1h" for intraday, "4h" for swing
Min Age: 12 bars, Max Age: 80 bars
```
**Rationale**: Forex allows multiple timeframes based on session trading

## Compatibility Matrix

| Asset Class | Chart TF | HTF Setting | Expected Result |
|-------------|----------|-------------|-----------------|
| BTCUSD | 4h | 1D | ✅ Works |
| BTCUSD | 1D | 1M | ✅ Works (Original MTP) |
| SPY | 1h | 1W | ✅ Works |
| NQ Futures | 30m | 1D | ✅ Works |
| XAUUSD | 1D | 1W | ✅ Works |
| EURUSD | 15m | 4H | ✅ Works |

## Critical Guardrails

⚠️ **HTF Validation**: The system validates that HTF ≥ baseTF  
⚠️ **History Check**: Minimum 300 bars required for reliable age validation  
⚠️ **Debug Warnings**: Red header in debug table when guardrails are violated  

## Migration from Original MTP

The original monthly-locked MTP system automatically migrates:
- `breakoutTimeframe="1M"` → `breakoutHTF="1M"`
- `dipBuyTimeframe="1M"` → `dipBuyHTF="1M"`  
- All age parameters now specified in "HTF bars" instead of "days"

## Usage Examples

### Conservative Setup (Lower Volatility)
```pinescript
HTF: 1W (Weekly levels)
Age: Min 15, Max 120 bars
Trail: 15-20 ATR distance
```

### Aggressive Setup (Higher Volatility)
```pinescript
HTF: 1M (Monthly levels)  
Age: Min 20, Max 200 bars
Trail: 22.5+ ATR distance
```

### Intraday Setup (Active Trading)
```pinescript
HTF: 1D or 4H (Daily/4H levels)
Age: Min 10, Max 80 bars  
Trail: 10-15 ATR distance
```

## Files

- `mtp.pine` - Main strategy file (Universal HTF version)
- `compatibility.txt` - Implementation notes and improvements
- `config.txt` - Parameter reference and validation rules
- `presets.txt` - Asset-specific configuration templates

## Performance Notes

The original MTP performance (+1,722% with 48% max drawdown) was achieved using:
- **1M timeframe** for both breakout and dip detection
- **20/200 bar age validation** for monthly levels  
- **22.5 ATR trailing** distance
- **Touch-high activation** mode

These settings remain the default but can now be adjusted for any asset class or trading style. 