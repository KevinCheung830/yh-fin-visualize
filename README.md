# Stock Analysis Tool - Technical Analysis Notebook

A comprehensive Python-based stock analysis tool that performs technical analysis using Yahoo Finance data. The tool includes support/resistance detection, order block identification, volume profile analysis, trend analysis, and candlestick pattern recognition.

## 📋 Table of Contents

- [Overview](#overview)
- [Key Steps](#key-steps)
- [Classes and Functions](#classes-and-functions)
- [Code Highlights](#code-highlights)
- [Input/Output Examples](#inputoutput-examples)
- [Installation](#installation)
- [Usage](#usage)

## 🎯 Overview

This notebook implements a complete stock technical analysis system with the following capabilities:

- **Data Fetching**: Retrieves historical stock data from Yahoo Finance
- **Technical Indicators**: Moving averages (20, 50, 200-day)
- **Support/Resistance Levels**: Automatic detection of key price levels
- **Order Blocks**: Identification of institutional trading zones
- **Volume Profile**: Analysis of volume distribution across price levels
- **Trend Analysis**: Multi-timeframe trend identification
- **Pattern Recognition**: Candlestick pattern detection (Doji, Engulfing, etc.)

## 🔑 Key Steps

### Workflow Overview

| Step | Description | Method/Function |
|------|-------------|-----------------|
| 1 | **Initialize Analyzer** | `StockAnalysisTool(symbol, period)` |
| 2 | **Fetch Stock Data** | `fetch_data()` |
| 3 | **Calculate Indicators** | `calculate_moving_averages()` |
| 4 | **Identify Key Levels** | `find_support_resistance()` |
| 5 | **Detect Order Blocks** | `identify_order_blocks()` |
| 6 | **Analyze Volume** | `calculate_volume_profile()` |
| 7 | **Determine Trend** | `analyze_trend()` |
| 8 | **Generate Signals** | `generate_signals()` |
| 9 | **Create Report** | `create_comprehensive_report()` |
| 10 | **Visualize Results** | `plot_candlestick_analysis()` |

### Detailed Process Flow

```
1. Initialize → 2. Fetch Data → 3. Calculate MA → 4. Find S/R Levels
                                                          ↓
10. Plot Analysis ← 9. Generate Report ← 8. Generate Signals ← 7. Analyze Trend
                                                          ↑
6. Volume Profile → 5. Order Blocks → 4. Find S/R Levels
```

## 🏗️ Classes and Functions

### Main Classes

#### `StockAnalysisTool`

The primary class for stock technical analysis.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `__init__` | `symbol: str`, `period: str` | `None` | Initialize analyzer with stock symbol and time period |
| `fetch_data` | `None` | `bool` | Fetch historical data from Yahoo Finance |
| `calculate_moving_averages` | `windows: list` | `DataFrame` | Calculate MA for specified windows (default: [20, 50, 200]) |
| `find_support_resistance` | `window: int`, `threshold: float` | `tuple` | Identify support and resistance levels using local minima/maxima |
| `identify_order_blocks` | `lookback_period: int` | `list` | Detect order blocks (institutional trading zones) |
| `calculate_volume_profile` | `price_bins: int` | `dict` | Calculate volume distribution across price levels |
| `find_high_volume_nodes` | `volume_profile: dict`, `threshold_ratio: float` | `dict` | Identify price levels with high volume concentration |
| `analyze_trend` | `None` | `str` | Determine trend direction (Uptrend/Downtrend/Sideways) |
| `generate_signals` | `None` | `list` | Generate trading signals based on multiple indicators |
| `create_comprehensive_report` | `None` | `dict` | Generate complete analysis report |
| `plot_candlestick_analysis` | `days_to_plot: int` | `None` | Create comprehensive candlestick chart with all indicators |
| `plot_volume_profile` | `price_bins: int` | `None` | Plot volume profile separately |

#### `AdvancedPatterns`

Class for detecting advanced chart patterns.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `__init__` | `stock_analyzer: StockAnalysisTool` | `None` | Initialize with stock analyzer instance |
| `detect_double_top_bottom` | `window: int` | `list` | Detect double top and double bottom patterns |
| `find_breakouts` | `consolidation_period: int`, `breakout_threshold: float` | `list` | Identify price breakouts from consolidation zones |

#### `CandlestickPatterns`

Class for detecting candlestick patterns.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `__init__` | `stock_analyzer: StockAnalysisTool` | `None` | Initialize with stock analyzer instance |
| `detect_doji` | `threshold: float` | `list` | Detect Doji candlestick patterns (indecision) |
| `detect_engulfing` | `None` | `list` | Detect Bullish/Bearish Engulfing patterns |

### Helper Functions

| Function | Parameters | Returns | Description |
|----------|-----------|---------|-------------|
| `main` | `None` | `None` | Example usage function demonstrating the tool |
| `analyze_multiple_stocks` | `symbols: list` | `dict` | Analyze multiple stocks and return results dictionary |

### Private Methods (Internal)

| Method | Description |
|--------|-------------|
| `_plot_candlesticks` | Internal method to plot candlestick chart |
| `_plot_moving_averages` | Internal method to plot moving averages |
| `_plot_support_resistance` | Internal method to plot support/resistance levels |
| `_plot_order_blocks` | Internal method to plot order blocks |
| `_plot_volume` | Internal method to plot volume bars |
| `_format_chart` | Internal method to format chart appearance |

## 💡 Code Highlights

### 1. Support/Resistance Detection Algorithm

```python
def find_support_resistance(self, window=20, threshold=0.02):
    """
    Find support and resistance levels using local minima and maxima
    """
    # Finds local minima (support) by checking if current low is lower
    # than surrounding lows within the window
    # Finds local maxima (resistance) by checking if current high is higher
    # than surrounding highs within the window
    # Filters out levels that are too close together using threshold
```

**Key Features:**
- Uses sliding window approach to identify local extrema
- Filters duplicate levels using percentage threshold
- Returns sorted lists of support (ascending) and resistance (descending) levels

### 2. Order Block Identification

```python
def identify_order_blocks(self, lookback_period=10):
    """
    Identify order blocks - areas where big players enter positions
    """
    # Detects strong momentum candles with:
    # - Body ratio > 60% (strong directional move)
    # - Price breaks previous candle's high/low
    # - Classifies as bullish or bearish order blocks
```

**Key Features:**
- Identifies institutional trading zones
- Uses body-to-range ratio to measure candle strength
- Tracks breakout candles that indicate significant moves

### 3. Volume Profile Calculation

```python
def calculate_volume_profile(self, price_bins=20):
    """
    Calculate volume profile - distribution of volume at different price levels
    """
    # Divides price range into bins
    # Sums volume for all candles that traded within each price bin
    # Returns dictionary mapping price levels to volume
```

**Key Features:**
- Creates price bins across the trading range
- Aggregates volume at each price level
- Identifies High Volume Nodes (HVN) for support/resistance

### 4. Trend Analysis Logic

```python
def analyze_trend(self):
    """
    Simple trend analysis using moving averages
    """
    # Compares current price and moving averages:
    # - Strong Uptrend: Price > MA20 > MA50
    # - Moderate Uptrend: MA20 > MA50 but price may be below MA20
    # - Strong Downtrend: Price < MA20 < MA50
    # - Moderate Downtrend: MA20 < MA50 but price may be above MA20
    # - Sideways: Mixed signals
```

**Key Features:**
- Multi-level trend classification
- Uses relationship between price and multiple MAs
- Provides clear trend direction for trading decisions

### 5. Signal Generation System

```python
def generate_signals(self):
    """
    Generate trading signals based on multiple indicators
    """
    # Combines signals from:
    # - Moving average crossovers
    # - Support/resistance proximity
    # - Volume spikes
    # Returns actionable trading signals
```

**Key Features:**
- Multi-indicator confirmation
- Proximity-based support/resistance signals
- Volume confirmation for signal strength

### 6. Comprehensive Visualization

```python
def plot_candlestick_analysis(self, days_to_plot=90):
    """
    Create comprehensive candlestick chart with all analysis
    """
    # Creates multi-panel chart with:
    # - Candlestick price chart
    # - Moving averages overlay
    # - Support/resistance levels
    # - Order block highlights
    # - Volume bars with MA
```

**Key Features:**
- Professional candlestick visualization
- Multiple indicator overlays
- Color-coded order blocks
- Synchronized volume chart

## 📊 Input/Output Examples

### Input Parameters

| Parameter | Type | Default | Description | Example |
|-----------|------|---------|-------------|---------|
| `symbol` | `str` | Required | Stock ticker symbol | `"AAPL"`, `"TSLA"`, `"GOOGL"` |
| `period` | `str` | `"1y"` | Time period for data | `"1mo"`, `"3mo"`, `"6mo"`, `"1y"`, `"2y"`, `"5y"` |
| `window` | `int` | `20` | Window for support/resistance detection | `10`, `20`, `30` |
| `threshold` | `float` | `0.02` | Threshold for filtering levels (2%) | `0.01`, `0.02`, `0.03` |
| `lookback_period` | `int` | `10` | Period for order block detection | `5`, `10`, `20` |
| `price_bins` | `int` | `20` | Number of bins for volume profile | `10`, `20`, `50` |
| `days_to_plot` | `int` | `90` | Number of days to display in chart | `30`, `60`, `90`, `180` |

### Output Report Structure

| Field | Type | Description | Example Value |
|-------|------|-------------|---------------|
| `symbol` | `str` | Stock ticker symbol | `"AAPL"` |
| `current_price` | `float` | Latest closing price | `268.47` |
| `trend` | `str` | Trend classification | `"Strong Uptrend"` |
| `support_levels` | `list[float]` | Top 5 support levels | `[201.27, 205.50, 210.00]` |
| `resistance_levels` | `list[float]` | Top 5 resistance levels | `[215.98, 220.00, 225.50]` |
| `order_blocks_found` | `int` | Total number of order blocks | `32` |
| `recent_order_blocks` | `list[dict]` | Last 3 order blocks with details | See below |
| `high_volume_nodes` | `list[float]` | Price levels with high volume | `[207.95, 228.97]` |
| `trading_signals` | `list[str]` | Generated trading signals | `["BUY - Price crossed above 20MA"]` |
| `volume_analysis` | `str` | Volume concentration summary | `"High volume concentration at: $207.95, $228.97"` |

### Example Output: AAPL Analysis

```
=== STOCK ANALYSIS REPORT ===
Symbol: AAPL
Current Price: $268.47
Trend: Strong Uptrend

Support Levels: ['$201.27']
Resistance Levels: ['$215.98']

Trading Signals:
  - No strong signals - Wait for confirmation

High volume concentration at: $207.95, $228.97
```

### Example Output: TSLA Analysis

```
=== STOCK ANALYSIS REPORT ===
Symbol: TSLA
Current Price: $429.52
Trend: Moderate Uptrend
Support Levels: []
Resistance Levels: [470.75]
Order Blocks Found: 32
Recent Order Blocks: [
  {
    'date': '2025-10-23',
    'high': 449.40,
    'low': 413.90,
    'type': 'bullish',
    'strength': 0.82
  },
  ...
]
High Volume Nodes: [328.45]
Trading Signals: ['SELL - Price crossed below 20MA']
Volume Analysis: High volume concentration at: $328.45
```

### Order Block Structure

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `date` | `Timestamp` | Date of the order block | `2025-10-23 00:00:00-0400` |
| `high` | `float` | High price of the order block | `449.40` |
| `low` | `float` | Low price of the order block | `413.90` |
| `type` | `str` | Type of order block | `"bullish"` or `"bearish"` |
| `strength` | `float` | Body ratio (0-1) | `0.82` |

### Trading Signals

| Signal Type | Condition | Description |
|-------------|-----------|-------------|
| **BUY - Price crossed above 20MA** | `Close > MA20` (current) and `Close <= MA20` (previous) | Bullish crossover signal |
| **SELL - Price crossed below 20MA** | `Close < MA20` (current) and `Close >= MA20` (previous) | Bearish crossover signal |
| **Potential BUY - Near support level** | Price within 1% of support level | Support bounce opportunity |
| **Potential SELL - Near resistance level** | Price within 1% of resistance level | Resistance rejection opportunity |
| **High volume detected - confirm direction** | Volume > 1.5 × average volume | Significant activity detected |
| **No strong signals - Wait for confirmation** | No signals triggered | Wait for clearer setup |

### Trend Classifications

| Trend | Condition | Description |
|-------|-----------|-------------|
| **Strong Uptrend** | `Price > MA20 > MA50` | Clear bullish momentum |
| **Moderate Uptrend** | `MA20 > MA50` but `Price ≤ MA20` | Bullish but price may be consolidating |
| **Strong Downtrend** | `Price < MA20 < MA50` | Clear bearish momentum |
| **Moderate Downtrend** | `MA20 < MA50` but `Price ≥ MA20` | Bearish but price may be recovering |
| **Sideways/Ranging** | Mixed signals | No clear trend direction |

## 📦 Installation

### Required Packages

```bash
pip install yfinance pandas numpy matplotlib scikit-learn ta-lib
```

### Package Versions

| Package | Purpose | Version |
|---------|---------|---------|
| `yfinance` | Yahoo Finance data fetching | Latest |
| `pandas` | Data manipulation | Latest |
| `numpy` | Numerical computations | Latest |
| `matplotlib` | Data visualization | Latest |
| `scikit-learn` | Machine learning utilities | Latest |
| `ta-lib` | Technical analysis library | Latest |

## 🚀 Usage

### Basic Usage

```python
from stock_analysis import StockAnalysisTool

# Initialize analyzer
analyzer = StockAnalysisTool("AAPL", "6mo")

# Generate comprehensive report
report = analyzer.create_comprehensive_report()

# Print report
print(f"Symbol: {report['symbol']}")
print(f"Current Price: ${report['current_price']:.2f}")
print(f"Trend: {report['trend']}")

# Visualize
analyzer.plot_candlestick_analysis(60)
```

### Advanced Usage

```python
# Multiple stock analysis
symbols = ["AAPL", "GOOGL", "MSFT", "AMZN"]
results = analyze_multiple_stocks(symbols)

# Pattern detection
from stock_analysis import AdvancedPatterns, CandlestickPatterns

patterns = AdvancedPatterns(analyzer)
double_tops = patterns.detect_double_top_bottom()
breakouts = patterns.find_breakouts()

candlestick = CandlestickPatterns(analyzer)
doji = candlestick.detect_doji()
engulfing = candlestick.detect_engulfing()
```

### Customization Options

| Feature | Customization | Example |
|---------|--------------|---------|
| **Moving Averages** | Specify custom windows | `calculate_moving_averages([10, 30, 100])` |
| **Support/Resistance** | Adjust window and threshold | `find_support_resistance(window=30, threshold=0.03)` |
| **Order Blocks** | Change lookback period | `identify_order_blocks(lookback_period=20)` |
| **Volume Profile** | Adjust number of bins | `calculate_volume_profile(price_bins=50)` |
| **Chart Display** | Change days to plot | `plot_candlestick_analysis(days_to_plot=180)` |

## 📈 Visualization Features

### Chart Components

| Component | Description | Color/Style |
|-----------|-------------|-------------|
| **Candlesticks** | Price action visualization | Green (bullish) / Red (bearish) |
| **Moving Averages** | Trend indicators | Blue (MA20), Orange (MA50), Purple (MA200) |
| **Support Levels** | Key support zones | Green dashed lines |
| **Resistance Levels** | Key resistance zones | Red dashed lines |
| **Order Blocks** | Institutional zones | Light green (bullish) / Light coral (bearish) |
| **Volume Bars** | Trading volume | Color-coded by price direction |
| **Volume MA** | Volume trend | Blue line |

## 🔍 Technical Details

### Algorithm Parameters

| Algorithm | Parameter | Default | Impact |
|-----------|-----------|---------|--------|
| **Support/Resistance** | `window` | 20 | Larger = fewer, more significant levels |
| **Support/Resistance** | `threshold` | 0.02 | Larger = fewer duplicate levels |
| **Order Blocks** | `lookback_period` | 10 | Larger = more conservative detection |
| **Volume Profile** | `price_bins` | 20 | Larger = finer granularity |
| **High Volume Nodes** | `threshold_ratio` | 0.7 | Larger = fewer HVN identified |

### Performance Considerations

- **Data Fetching**: First call may take 2-5 seconds depending on period
- **Support/Resistance**: O(n×window) complexity - may be slow for large datasets
- **Order Blocks**: O(n) complexity - efficient for real-time analysis
- **Volume Profile**: O(n×bins) complexity - scales well with data size

## 📝 Notes

- All price levels are in USD
- Dates are in timezone-aware format (America/New_York)
- Volume is in shares traded
- Moving averages use closing prices
- Support/resistance levels are filtered to avoid duplicates within threshold

## 🎯 Use Cases

1. **Day Trading**: Identify intraday support/resistance and order blocks
2. **Swing Trading**: Use trend analysis and pattern detection for entry/exit
3. **Position Trading**: Long-term trend analysis with multiple timeframes
4. **Risk Management**: Identify key levels for stop-loss and take-profit
5. **Market Research**: Analyze multiple stocks for portfolio construction

---



