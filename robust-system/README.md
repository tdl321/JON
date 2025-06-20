# JON Robust Strategy System

## Overview
The **JON Robust Strategy** is a multi-asset, multi-timeframe trend following system designed for maximum compatibility and robustness across different markets and chart resolutions.

## Key Features
- ✅ **Multi-Asset Support**: Works on crypto, indices, futures, metals
- ✅ **Multi-Timeframe**: Configurable base timeframe independence
- ✅ **Futures-Aware**: Proper contract point value calculations
- ✅ **Jerry Parker Inspired**: Donchian breakout and dip recovery system
- ✅ **Risk Management**: 1% per unit, max 4% total risk with pyramid scaling

## Files

### Core Strategy
- **`robust.pine`** - Main robust strategy with all 3 robustness fixes
- **`robust-experimental.pine`** - Alternative/experimental version

## Usage Instructions

### For Crypto (BTCUSD, ETHUSD)
```
Base Timeframe: "1D"
Futures Mode: OFF
ATR Length: 100
Trail Distance: 22.5 ATR
```

### For Futures (MNK, SIL, MNQ, ES)
```
Base Timeframe: "1D" 
Futures Mode: ON
ATR Length: 100
Trail Distance: 22.5 ATR
```

### For Indices (QQQ, SPY)
```
Base Timeframe: "1D"
Futures Mode: OFF
ATR Length: 100
Trail Distance: 22.5 ATR
```

## Robustness Fixes Applied
1. **Timeframe Abstraction**: All logic runs on configurable baseTF
2. **Futures Position Sizing**: Contract point value integration
3. **Enhanced Data Validation**: Contract metadata and sufficiency checks

## Philosophy
"Automate Everything, Robustness over Optimization" - Works reliably across all asset classes and timeframes with minimal parameter tweaking required. 