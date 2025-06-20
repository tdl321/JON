# JON Trading Strategy Systems

## Project Overview
This repository contains two distinct but related trend following strategy systems, each optimized for different use cases and performance objectives.

## 🛡️ System Architecture

```
JON/
├── robust-system/          # Multi-asset robustness focused
│   ├── robust.pine                 # Main robust strategy  
│   ├── robust-experimental.pine    # Alternative version
│   └── README.md                   # Robust system docs
├── mtp-system/             # High-performance monthly system
│   ├── mtp.pine                    # Main MTP strategy
│   ├── config.txt                  # Configuration settings
│   ├── presets.txt                 # TradingView presets
│   ├── hybrid-presets.txt          # Enhanced hybrid presets
│   ├── compatibility.txt           # Compatibility notes
│   └── README.md                   # MTP system docs
├── shared/                 # Shared resources
├── docs/                   # General documentation
├── .cursor/rules/          # Development standards
│   └── pinescript.mdc              # Pine Script coding rules
└── README.md              # This file
```

## 🎯 Choosing Your System

### Use **Robust System** When:
- ✅ Trading **multiple asset classes** (crypto, futures, indices, metals)
- ✅ Need **maximum compatibility** across different timeframes
- ✅ Want **reliable performance** with minimal tweaking
- ✅ Focus on **consistency over peak performance**
- ✅ Trading **futures contracts** (MNK, SIL, MNQ, ES)

### Use **MTP System** When:
- 🎯 Focused primarily on **crypto markets** (especially BTCUSD)
- 🎯 Want **maximum performance** (+17,227% historical results)
- 🎯 Can handle **higher drawdowns** (48% max)
- 🎯 Prefer **monthly resolution** analysis
- 🎯 Want **reference asset correlation** features

## 📊 Performance Comparison

| Metric | Robust System | MTP System |
|--------|---------------|------------|
| **Focus** | Multi-asset reliability | Peak crypto performance |
| **Complexity** | Simple, minimal params | Advanced monthly logic |
| **Drawdown** | Lower, more controlled | Higher but acceptable |
| **Profit Potential** | Steady, consistent | Exceptional (+17K%) |
| **Asset Classes** | All markets | Optimized for crypto |
| **Maintenance** | Set-and-forget | Periodic optimization |

## 🚀 Quick Start

### Robust System
1. Navigate to `robust-system/`
2. Load `robust.pine` in TradingView
3. Set Base TF = "1D", configure for your asset class
4. Enable Futures Mode for futures contracts

### MTP System  
1. Navigate to `mtp-system/`
2. Load `mtp.pine` in TradingView
3. Use provided presets from config files
4. Optimal on BTCUSD with CRYPTOCAP:TOTAL reference

## 🔧 Recent Updates

### Both Systems (January 2025)
- ✅ **Robustness Fixes Applied**: Timeframe independence, futures support, enhanced validation
- ✅ **Pine Script v6 Compliance**: Full syntax and scope compliance
- ✅ **Multi-Asset Support**: Works across all major asset classes
- ✅ **Enhanced Debug Features**: Comprehensive debugging interfaces

## 📖 Documentation

- **Technical Rules**: See `.cursor/rules/pinescript.mdc` for Pine Script standards
- **System-Specific Docs**: Check README.md in each system directory

## 🎭 Philosophy

**Robust System**: *"Automate Everything, Robustness over Optimization"*
**MTP System**: *"Monthly Trends, Maximum Hold"*

Both systems follow the core principle of systematic trend following with automated execution, but with different optimization objectives. 