# Online SOM-RL Strategy (PineScript)

An educational trading strategy that combines **Self-Organizing Maps (SOM)** with **Reinforcement Learning (Q-learning)** to make automated trading decisions in TradingView.

## ⚠️ Important Disclaimers

**This is an educational learning project only.**

**DO NOT USE THIS SCRIPT WHEN YOU TRADE WITH YOUR REAL MONEY. I DO NOT GUARANTEE THAT THIS STRATEGY IS A PROFITABLE ONE LONG TERM. USE IT ON YOUR OWN RISK.**

## Overview

This PineScript strategy implements a machine learning approach to trading that:

1. **Learns market patterns** using Self-Organizing Maps (neural networks)
2. **Makes decisions** using Q-learning reinforcement learning
3. **Adapts over time** with dynamic learning parameters
4. **Manages risk** through position sizing and volatility penalties

## How It Works

### Core Components

- **SOM Network**: 20 nodes (configurable) that learn to represent different market states
- **Input Features**: 50 past price ticks with distance-function optimized preprocessing
- **Q-Learning**: Each SOM node maintains Q-values for 9 possible trading actions
- **Reward System**: Risk-adjusted returns with penalties for overtrading and volatility

### Learning Process

1. **Delay Phase** (150 bars): No learning occurs
2. **Warmup Phase** (7000 bars): Strategy learns patterns but doesn't trade
3. **Live Trading Phase**: Makes decisions based on learned patterns

### Trading Actions

- **HOLD**: No position change
- **BUY**: INV_500, INV_1000, INV_2000, INV_5000 (investment amounts)
- **SELL**: SELL_500, SELL_1000, SELL_2000, SELL_5000 (sell amounts)

### Risk Management

- Trading penalties to discourage overtrading
- Volatility adjustments based on market conditions
- Position sizing penalties to prevent excessive exposure
- Directional consistency bonuses for aligned trades

## Input Vector Features

The strategy uses a simplified, optimized input vector that focuses on price patterns with preprocessing adapted to the selected distance function for optimal SOM learning performance.

### Vector Structure

The input vector consists of:

1. **Price Features** (M dimensions): Historical price returns processed according to distance function

**Total Vector Size**: `M` (default: 50 dimensions)

### Distance-Optimized Preprocessing

#### For Cosine Distance (Pattern Recognition)

- **Price Processing**: Uses price returns with DC component removal to focus on relative movements
- **Benefit**: Emphasizes market patterns and trends while being scale-invariant, ideal for regime detection

#### For Euclidean Distance (Magnitude-Sensitive)

- **Price Processing**: Z-score normalized price returns (mean=0, std=1) with outlier clipping to [-3, 3]
- **Benefit**: Ensures equal contribution from all features, sensitive to both direction and magnitude of changes

### Feature Components Explained

#### Price Features

- **Purpose**: Capture market price dynamics and trends
- **Preprocessing**: Returns-based (percentage changes) rather than raw prices to achieve stationarity
- **Benefit**: Removes price level bias, focuses on relative movements that are more predictive
- **Why Important**: SOMs can learn different market regimes (trending, ranging, volatile) from price patterns

### Technical Benefits

1. **Scale Consistency**: Price features are normalized to similar ranges for optimal learning
2. **Stationarity**: Price returns are more stationary than raw price values
3. **Outlier Robustness**: Clipping prevents extreme values from distorting SOM learning
4. **Pattern Recognition**: DC removal and normalization enhance the SOM's ability to detect recurring patterns

### Configuration Parameters

- **M**: Number of historical price bars (default: 50)
- **distance_function**: Preprocessing optimization ("cosine" or "euclidean")

## Usage Instructions

The strategy code is in `./online-som-rl-strategy.pinescript`.

1. Open any chart in [TradingView](https://tradingview.com/)
2. Open the Pine Editor
3. Copy & paste the code from `./online-som-rl-strategy.pinescript` into the editor
4. Click "Add to chart" button
5. Adjust parameters in the strategy settings as needed

## Configurable Parameters

### Core Parameters

- **M**: Past price ticks for input vector (default: 50)
- **P**: Future ticks to evaluate reward (default: 20)
- **N**: Number of SOM nodes (default: 20)

### Learning Parameters

- **Initial exploration**: Exploration probability (default: 0.1)
- **Initial beta**: Learning rate (default: 0.6)
- **Initial gamma**: Discount factor (default: 0.9)
- **Sigma factor**: Neighborhood width (default: 0.05)
- **Time decay factor**: Parameter decay rate (default: 0.9992)

### Reward Function Parameters

- **Volatility penalty factor**: Penalty for high volatility (default: 0.1)
- **Volatility penalty cap**: Maximum volatility penalty (default: 0.05)
- **Position penalty factor**: Penalty for large positions (default: 0.001)
- **Directional bonus**: Reward for directional consistency (default: 0.01)
- **Trading penalty**: Cost of trading (default: 0.02)

### Algorithm Parameters

- **Distance function**: Method for SOM similarity calculation (default: "cosine", options: "cosine", "euclidean")

### Phase Control

- **Delay phase**: Bars before learning starts (default: 150)
- **Warmup phase**: Learning-only period (default: 7000)
- **SOM update frequency**: Update SOM every N bars (default: 2)

## How to use claude code in this project

On Windows: A .devcontainer-config is provided, so that you can use claude in a linux-container.

Install claude-cli first:

```bash
npm install -g @anthropic-ai/claude-code
```

Then, run claude-cli:

```bash
claude
```
