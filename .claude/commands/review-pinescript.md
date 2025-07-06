# PineScript v6 Strategy Code Review

**Command**: Review the pinescript strategy. Focus first on code quality issues.

## Comprehensive Analysis Framework

### 1. PineScript v6 Syntax Compliance
- Verify proper `//@version=6` declaration
- Check for deprecated v5 syntax usage
- Validate type system usage (built-in types, user-defined types)
- Ensure proper function syntax and parameter handling
- Review array and matrix operations for v6 compatibility

### 2. Code Structure & Organization
- Evaluate logical separation of concerns (inputs, calculations, logic, outputs)
- Check for appropriate use of user-defined types and functions
- Review variable and function naming conventions
- Assess code modularity and reusability
- Verify proper commenting and documentation

### 3. Performance & Efficiency
- Identify unnecessary calculations or redundant operations
- Check for proper use of `var` declarations for persistent state
- Review loop efficiency and array operations
- Evaluate memory usage with large data structures
- Check for proper caching of expensive calculations

### 4. Error Handling & Edge Cases
- Verify input validation and bounds checking
- Check for proper NA value handling
- Review division by zero protection
- Validate array bounds and size checks
- Ensure proper handling of edge cases in mathematical operations

### 5. Trading Logic & Risk Management
- Review strategy entry/exit conditions
- Check for proper position sizing and risk controls
- Validate stop-loss and take-profit implementations
- Assess overtrading prevention measures
- Review backtesting considerations

### 6. PineScript Best Practices
- Check for proper use of series vs simple values
- Review security function usage if applicable
- Validate plot and display functions
- Check for proper use of built-in functions and variables
- Review execution model understanding (bar-by-bar vs real-time)

### 7. Mathematical & Algorithmic Correctness
- Verify mathematical formula implementations
- Check algorithm logic and flow
- Review statistical calculations and normalizations
- Validate machine learning implementations
- Check for numerical stability issues

### 8. Code Quality Metrics
- Function complexity and length
- Variable scope and lifetime management
- Code duplication and repetition
- Magic numbers and hardcoded values
- Maintainability and readability

### 9. Security & Safety
- Check for potential infinite loops or excessive calculations
- Review resource usage implications
- Validate input sanitization
- Check for proper error propagation

### 10. Documentation & Maintainability
- Review inline comments and documentation
- Check for clear variable and function names
- Evaluate code self-documentation
- Assess ease of future modifications

## Analysis Output Format
- Provide specific line number references
- Categorize issues by severity (Critical, High, Medium, Low)
- Suggest specific improvements with code examples
- Prioritize fixes based on impact and complexity
- Include PineScript v6 specific recommendations

## Target Files
- Primary: `online-som-rl-strategy.txt` (main strategy implementation)
- Secondary: Related configuration and documentation files

## Expected Deliverables
1. Comprehensive code quality assessment
2. Prioritized list of issues with specific fixes
3. PineScript v6 compliance verification
4. Performance optimization recommendations
5. Best practices alignment suggestions