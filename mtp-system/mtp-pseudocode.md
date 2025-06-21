# JON MTP Universal HTF Trend Following Strategy - Pseudo Code

## OVERVIEW
This is a Universal Higher Timeframe (HTF) trend following system that works across all asset classes (crypto, indices, futures, metals, forex) with configurable timeframes and robust risk management.

## 1. INPUT PARAMETERS & CONFIGURATION

### Robustness Settings (R-0)
- Enable futures mode for proper contract-based position sizing
- Uses contract point value and tick size for accurate risk calculation

### Trend Filter (F-1) 
- Enable/disable 200 EMA trend filter
- Configurable EMA length (default 200)
- Optional reference asset correlation (default: CRYPTOCAP:TOTAL)
- Only trade when both primary asset AND reference asset are above their EMAs

### Breakout Entry System (F-2) - Universal HTF
- Enable/disable breakout entries
- Configurable higher timeframe (default: 1M monthly)
- Age validation rules:
  - HTF high must be 20-200 bars old
  - Must cross above HTF high within 20 bars
- Prevents trading on stale or too-recent highs

### Dip Buy Entry System (F-3) - Universal HTF  
- Enable/disable dip buy entries
- Configurable higher timeframe (default: 1M monthly)
- Touch and recovery logic:
  - Must touch HTF low (within 0.5% buffer)
  - Must recover above HTF low + 1% buffer
  - HTF low must be 15-90 bars old
  - Recovery must happen within 30 bars

### Exit Strategy (F-4) - High Performance
- ATR trailing stop system
- Wide 22.5 ATR trailing distance (key performance feature)
- Touch-high activation: starts trailing immediately on any new high
- Dynamic volatility adjustment based on 14-period ATR comparison

### Risk Management (R-1, R-2)
- Unit-based position sizing (1% risk per unit)
- Maximum 4% total trade risk (4 units max)
- 100-period ATR for stable calculations (vs 14-period noise)
- 5 ATR initial stop loss
- Correlation features for unit coordination

### Pyramiding Engine (R-4)
- Add up to 3 additional units (4 total)
- 1 ATR spacing between unit additions
- Trailing stop adjusts on pyramiding
- Risk validation prevents over-leveraging

## 2. CORE INDICATORS & DATA VALIDATION

### Basic Indicators
```
ATR = 100-period Average True Range (extended for stability)
TREND_EMA = 200-period EMA of close price
REFERENCE_EMA = 200-period EMA of reference asset (if enabled)
```

### Higher Timeframe Data
```
HTF_HIGH = Get high from breakout timeframe (1M default)
HTF_LOW = Get low from dip buy timeframe (1M default)
```

### Comprehensive Data Validation
```
BASIC_DATA_VALID = Check close, high, low are positive and logical
ATR_DATA_VALID = ATR exists and is positive with sufficient history
TREND_DATA_VALID = EMA exists and is positive with sufficient history  
REFERENCE_DATA_VALID = Reference asset data valid (if enabled)
HTF_DATA_VALID = HTF high and low exist and are logical
CONTRACT_VALID = Futures contract data valid (if futures mode)
MATH_SAFETY_VALID = All values positive for calculations
HISTORY_SUFFICIENT = Enough bars for all indicators

IS_DATA_VALID = ALL validation checks must pass
```

## 3. ENTRY TRIGGER LOGIC

### HTF High/Low Age Tracking
```
When HTF_HIGH changes:
    Record bar number of change
    Reset cross-above tracking

When HTF_LOW changes:
    Record bar number of change  
    Reset touch tracking

When price crosses above HTF_HIGH:
    Record bar number of first cross

When price touches HTF_LOW (within buffer):
    Record bar number of first touch
```

### Breakout Entry Conditions
```
BASIC_BREAKOUT = Close > HTF_HIGH
AGE_VALID = HTF_HIGH is 20-200 bars old
CROSS_RECENT = Crossed above HTF_HIGH within last 20 bars
BREAKOUT_TRIGGER = BASIC_BREAKOUT AND AGE_VALID AND CROSS_RECENT
```

### Dip Buy Entry Conditions
```
TOUCHED_LOW = Low touched HTF_LOW (within 0.5% buffer)
RECOVERY_LEVEL = HTF_LOW + 1% buffer
RECOVERY_TRIGGER = TOUCHED_LOW AND Close > RECOVERY_LEVEL
AGE_VALID = HTF_LOW is 15-90 bars old  
RECOVERY_RECENT = Recovery happened within 30 bars of touch
DIP_BUY_TRIGGER = RECOVERY_TRIGGER AND AGE_VALID AND RECOVERY_RECENT
```

### Combined Entry Logic
```
TREND_BULLISH = Close > TREND_EMA (if enabled)
REFERENCE_BULLISH = Reference asset > Reference EMA (if enabled)
TREND_FILTER_PASSED = TREND_BULLISH AND REFERENCE_BULLISH

ENTRY_CONDITION = CONTRACT_VALID AND IS_DATA_VALID AND TREND_FILTER_PASSED AND (BREAKOUT_TRIGGER OR DIP_BUY_TRIGGER)
```

## 4. POSITION SIZING & RISK MANAGEMENT

### Unit Risk Calculation
```
UNIT_RISK_DOLLARS = Account Equity × (Risk Per Unit % ÷ 100)
```

### Dynamic Volatility Adjustment
```
AVERAGE_ATR = 14-period average of ATR
If current ATR > 1.2 × AVERAGE_ATR: Multiply trail distance by 1.3
If current ATR < 0.8 × AVERAGE_ATR: Multiply trail distance by 0.7
Otherwise: Use standard trail distance
```

### Position Size Calculation
```
Function CALCULATE_UNIT_SIZE(entry_price, stop_price):
    If futures mode enabled:
        Risk per contract = |entry_price - stop_price| × contract_point_value
        Contracts = UNIT_RISK_DOLLARS ÷ risk_per_contract
        Apply margin safety limits
        Return floor(contracts)
    Else:
        Risk per share = |entry_price - stop_price|
        Unit value = UNIT_RISK_DOLLARS ÷ risk_per_share
        Apply equity safety limits  
        Return floor(unit_value)
```

### Initial Stop Loss
```
Function CALCULATE_INITIAL_STOP(entry_price):
    ATR_STOP = entry_price - (ATR × 5.0)
    SAFETY_STOP = entry_price × 0.95
    Return minimum(ATR_STOP, SAFETY_STOP)
```

## 5. EXIT STRATEGY LOGIC

### Touch-High Trailing System
```
If position exists:
    CURRENT_PROFIT = close - position_average_price
    PROFIT_IN_ATR = CURRENT_PROFIT ÷ ATR
    
    If trailing not yet activated:
        If touch_high_mode enabled:
            If high > entry_price: Activate trailing, set trail_high = high
        Else:
            If close >= entry_price + (5 × ATR): Activate trailing
    
    If trailing activated:
        If high > trail_high: Update trail_high = high
        NEW_TRAIL_STOP = trail_high - (22.5 × ATR × volatility_adjustment)
        
        If first trail stop: Set trail_stop = NEW_TRAIL_STOP
        Else: trail_stop = maximum(current_trail_stop, NEW_TRAIL_STOP)
        
        If unit correlation enabled: 
            trail_stop = maximum(trail_stop, current_stop_price)
```

### Exit Conditions
```
STOP_LOSS_HIT = Position exists AND low <= current_stop_price
TREND_FILTER_FAIL = Position exists AND trend filter fails
```

## 6. TRADE EXECUTION LOGIC

### Initial Entry
```
If no position AND entry_condition met:
    Calculate initial_stop = CALCULATE_INITIAL_STOP(close)
    If initial_stop valid:
        Calculate unit_size = CALCULATE_UNIT_SIZE(close, initial_stop)
        If unit_size > 0:
            Determine entry_type = "F2B" (breakout) or "F3D" (dip buy)
            Execute entry with ID "T1-" + entry_type
            Set current_stop_price = initial_stop
            Reset tier tracking
```

### Pyramiding Logic
```
If pyramiding enabled AND position exists AND tiers < max_tiers:
    PROFIT_IN_ATR = (close - average_entry_price) ÷ ATR
    NEXT_TIER_THRESHOLD = (current_tier + 1) × 1.0 ATR
    
    If PROFIT_IN_ATR >= NEXT_TIER_THRESHOLD:
        If price distance from last tier >= 1.0 ATR:
            Calculate current_risk = position_risk_percentage
            If current_risk + unit_risk <= max_total_risk:
                Calculate tier_stop = CALCULATE_INITIAL_STOP(close)
                
                If unit correlation enabled:
                    tier_stop = maximum(tier_stop, current_stop_price)
                
                Calculate tier_size = CALCULATE_UNIT_SIZE(close, tier_stop)
                If tier_size > 0:
                    Execute tier entry with ID "T" + tier_number + "-" + entry_type
                    Update tier tracking
                    Update stop price based on correlation settings
```

### Exit Execution
```
If STOP_LOSS_HIT:
    Close all positions with comment "T" + tier_count + "-F4E"
    
If TREND_FILTER_FAIL:
    Close all positions with comment "T" + tier_count + "-F1E"
    
If position closed:
    Reset all tracking variables
```

## 7. VISUAL ZONE MANAGEMENT

### Active Trade Zones
```
If position exists:
    UPPER_BOUND = maximum(high × 1.05, close × 1.10)
    
    If show_profit_zone enabled:
        Create/update green profit zone from entry_price to UPPER_BOUND
    
    If show_risk_zone enabled:
        Create/update red risk zone from entry_price to current_stop_price
    
    If show_stop_line enabled:
        Create/update red stop loss line at current_stop_price
```

### Historical Zone Preservation
```
When position closes:
    If show_historical_zones enabled:
        Move current zones to historical arrays
        Limit historical arrays to max_zones (default 50)
        Delete oldest zones when limit exceeded
    Else:
        Delete current zones immediately
```

## 8. VISUAL INTERFACE

### Plots and Indicators
```
Plot 200 EMA if enabled (blue line)
Plot entry arrows (blue up arrows) when position opens
Plot exit arrows (orange down arrows) when position closes  
Show position status background (subtle green) when in trade
```

### Debug Information Table
```
If debug enabled:
    Show comprehensive table with:
    - HTF validation status
    - Current timeframe vs HTF compatibility
    - Contract data validity
    - Position details
    - ATR and HTF values
    - Age calculations
    - Reference asset status
    - Trail activation status
    - Data validation results
```

## KEY PERFORMANCE FEATURES

1. **Universal HTF System**: Works with any timeframe combination
2. **Extended ATR Periods**: 100-period ATR reduces noise
3. **Wide Trailing Stops**: 22.5 ATR distance holds major trends
4. **Touch-High Activation**: Immediate trailing on new highs
5. **Age Validation**: Prevents trading stale or too-recent levels
6. **Reference Asset Correlation**: Macro trend confirmation
7. **Robust Data Validation**: Prevents execution on bad data
8. **Futures-Aware Sizing**: Proper contract-based position sizing

## ROBUSTNESS GUARDRAILS

- Comprehensive data validation before any trade execution
- HTF compatibility warnings (HTF must be >= chart timeframe)
- Minimum history requirements (300+ bars recommended)
- Contract data validation for futures
- Mathematical safety checks (no division by zero)
- Memory management for visual zones
- Error handling for all calculations 