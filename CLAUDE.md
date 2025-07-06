# Claude Instructions for Online SOM-RL Strategy Project

## Project Overview

This is an educational PineScript trading strategy that combines Self-Organizing Maps (SOM) with Reinforcement Learning (Q-learning) for TradingView. The main strategy code is in `online-som-rl-strategy.pinescript`.

## Key Files

- `online-som-rl-strategy.pinescript` - Main PineScript strategy file (219 lines)
- `README.md` - Project documentation
- `package.json` - Project metadata and Claude Code integration

## Strategy Architecture

### Core Components

1. **SOM Network**: Neural network with N nodes (default 20) that learn market state representations
2. **Q-Learning**: Each SOM node maintains Q-values for 8 trading actions
3. **Input Features**: M past price ticks (default 50) with normalized price and log-normalized volume
4. **Reward System**: Risk-adjusted returns with trading penalties

### Learning Phases

1. **Delay Phase**: 150 bars - no learning
2. **Warmup Phase**: 7000 bars - learns patterns, no trading
3. **Live Trading**: Makes decisions based on learned patterns

### Trading Actions

- HOLD, INV_500, INV_1000, INV_2000, INV_5000 (buy amounts)
- SELL_500, SELL_1000, SELL_2000, SELL_5000 (sell amounts)

## Development Guidelines

### When Working with Strategy Code

- **File**: Always work with `online-som-rl-strategy.pinescript`
- **Language**: PineScript v6 syntax
- **Testing**: Cannot run automated tests - this is PineScript for TradingView
- **Validation**: Code must be manually tested in TradingView Pine Editor

### Code Structure Key Functions

- `SOMNode` type definition (lines 5-7)
- `get_input_vector()` - Creates input features (lines 59-74)
- `find_best_node()` - SOM winner selection (lines 117-129)
- `find_best_q()` - Q-value action selection (lines 132-145)
- `update_som_with_neighbors()` - SOM learning (lines 154-164)
- `reward()` - Reward calculation (lines 167-182)
- Main learning loop (lines 184-217)

### Important Parameters

- `M` = 50: Input vector length (past ticks)
- `P` = 20: Future ticks for reward evaluation
- `N` = 20: Number of SOM nodes
- Learning rates: exploration, beta, gamma (all decay over time)
- Phase durations: delay_phase, warmup_phase

### Risk Management Features

- Trading penalties to discourage overtrading
- Volatility adjustments
- Position sizing penalties
- Directional consistency bonuses

## Common Tasks

### Parameter Tuning

When adjusting parameters, focus on:

- Learning rates (beta, gamma, exploration)
- Network size (N - number of SOM nodes)
- Input size (M - past ticks)
- Phase durations (delay, warmup)
- Trading penalties and rewards

### Performance Analysis

- Monitor `risk_adjusted_return` plot
- Check `t_decay` for learning parameter decay
- Review log outputs for trading decisions
- Analyze `strategy.opentrades.capital_held` for position tracking

### Code Improvements

- Focus on reward function refinement
- SOM learning efficiency
- Action selection strategies
- Risk management enhancements

## Testing Instructions

Since this is PineScript:

1. Copy code from `online-som-rl-strategy.pinescript`
2. Paste into TradingView Pine Editor
3. Check for syntax errors
4. Add to chart and monitor performance
5. Adjust parameters through strategy settings

## Important Notes

- **Educational Purpose Only**: Strong disclaimers about real money trading
- **No Automated Testing**: Manual validation required in TradingView
- **Performance Dependent**: Results vary significantly based on market conditions
- **Parameter Sensitivity**: Small changes can have large impacts on performance

## Development Workflow

1. Make changes to `online-som-rl-strategy.pinescript`
2. Test in TradingView Pine Editor
3. Document parameter changes in commit messages
4. **Always update README.md** to reflect any changes made to the project
5. Maintain educational disclaimers

## Documentation Maintenance

**CRITICAL**: The `README.md` must always be kept current and accurate:

- Update documentation for ANY changes to the strategy, parameters, features, or functionality
- Ensure all sections reflect the current state of the implementation
- Maintain consistency between the strategy code and its documentation
- Keep usage instructions, parameter descriptions, and feature explanations up to date
- Document any modifications to the learning process, trading logic, or risk management

The README serves as the primary source of truth for users - it must always accurately represent the current project state.

## Security Considerations

- No API keys or sensitive data in code
- Educational use only - not for production trading
- All trading decisions logged for transparency
