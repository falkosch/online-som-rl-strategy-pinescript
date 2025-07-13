# Debugging Invalid Distance Calculations in SOM/Neural Networks

## Problem Symptoms
- Distance functions always return the same value (e.g., always 1.0, NA, or infinity)
- SOM always selects the same node regardless of input
- All input vectors have zero magnitude
- Cosine similarity returns NA or constant values
- Learning algorithm appears "stuck" or non-responsive

## Root Cause Analysis Checklist

### 1. Input Vector Validation
**Check for zero vectors:**
```
input_norm = 0.0
for i in vector:
    input_norm += i * i
if input_norm == 0:
    # Problem: All-zero vector
```

**Common causes of zero vectors:**
- DC removal on similar values (prices: 100.1, 100.2 → after mean subtraction: ~0, ~0)
- Overly aggressive normalization
- Conditional logic preventing feature assignment
- Wrong variable assignment syntax (= vs :=)

### 2. Feature Generation Review
**Look for these anti-patterns:**
```
# BAD: Can result in zero after normalization
feature = raw_price * scale
feature = feature - mean(all_features)  # If prices similar → ~0

# BAD: Complex conditions that may never trigger
if complex_condition_that_fails:
    feature = meaningful_value
# feature remains 0.0

# BAD: Wrong assignment in some languages
feature = calculate_value()  # May not persist in function scope
```

**Better approaches:**
```
# GOOD: Use differences/returns instead of raw values
feature = (current_price - prev_price) / prev_price * scale

# GOOD: Ensure at least one non-zero element
features[0] = small_nonzero_baseline  # e.g., 0.1

# GOOD: Simple, robust feature engineering
feature = log_return * large_scale_factor
```

### 3. Distance Function Validation
**Check for division by zero:**
```
# Cosine distance
norm_a = sqrt(sum(a[i]^2))
norm_b = sqrt(sum(b[i]^2))
if norm_a == 0 or norm_b == 0:
    return max_distance  # e.g., 1.0
return 1.0 - dot_product / (norm_a * norm_b)
```

**Euclidean distance is more robust:**
```
# Usually safe even with zero vectors
return sqrt(sum((a[i] - b[i])^2))
```

### 4. SOM Node Initialization
**Verify nodes start with non-zero values:**
```
# Initialize with small random values, not zeros
for node in som_nodes:
    node.weights = random_vector(-0.1, 0.1)  # Small but non-zero
```

## Systematic Debugging Steps

### Step 1: Add Diagnostic Logging
```
# Log vector statistics
vector_norm = sqrt(sum(x[i]^2))
zero_count = count(x[i] == 0.0 for i in x)
non_zero_count = len(x) - zero_count

log("Vector norm: {}, zeros: {}, non-zeros: {}".format(
    vector_norm, zero_count, non_zero_count))

# Log sample values
log("Sample features: [{}, {}, {}]".format(x[0], x[1], x[10]))
```

### Step 2: Test Feature Generation Isolation
```
# Create minimal test case
test_input = create_simple_test_vector()  # Known good values
test_distance = distance(test_input, som_node)
if test_distance is invalid:
    # Problem is in distance function
else:
    # Problem is in feature generation
```

### Step 3: Validate Distance Functions
```
# Test with known vectors
vec_a = [1.0, 0.0, 0.0]
vec_b = [0.0, 1.0, 0.0]
expected_cosine_distance = 1.0  # Orthogonal vectors
expected_euclidean_distance = sqrt(2)

actual_cosine = cosine_distance(vec_a, vec_b)
actual_euclidean = euclidean_distance(vec_a, vec_b)
```

### Step 4: Fix Feature Engineering
**Principle: Ensure meaningful variance in features**

```
# Instead of raw values that might be similar
features = [price1, price2, price3]  # 100.1, 100.2, 100.15

# Use relative changes and diverse feature types
features = [
    baseline_value,  # e.g., 0.1 (prevents zero vector)
    price_returns * scale,  # (price_t - price_t-1) / price_t-1
    volume_changes * scale,  # normalized volume differences
    volatility_measures,    # rolling standard deviation
    momentum_indicators,    # RSI, MACD, etc.
    technical_patterns,     # support/resistance levels
    market_regime_features  # trend/range classification
]
```

### Step 5: Robust Distance Implementation
```
def safe_cosine_distance(a, b, epsilon=1e-6):
    norm_a = sqrt(sum(x*x for x in a))
    norm_b = sqrt(sum(x*x for x in b))
    
    # Handle zero vectors
    if norm_a < epsilon or norm_b < epsilon:
        return 1.0  # Maximum distance
    
    dot_product = sum(a[i] * b[i] for i in range(len(a)))
    return 1.0 - dot_product / (norm_a * norm_b)
```

## Multi-Feature Vector Considerations

### Feature Scaling Issues
**Problem:** Different feature types have vastly different scales
```
# BAD: Mixed scales
features = [
    price_return,      # ~0.001 (0.1%)
    volume,           # ~1000000
    rsi_indicator     # ~50
]
```

**Solution:** Normalize each feature type appropriately
```
# GOOD: Consistent scaling
features = [
    price_return * 10000,     # ~10 (0.1% becomes 10)
    log(volume / avg_volume), # ~±2 (log ratio)
    (rsi - 50) / 50          # ~±1 (centered and scaled)
]
```

### Feature Selection Guidelines
1. **Include diverse feature types**: Price, volume, volatility, momentum, sentiment
2. **Use relative measures**: Returns, ratios, z-scores instead of absolute values
3. **Ensure temporal consistency**: All features should represent the same time period
4. **Add baseline elements**: Include known non-zero values to prevent zero vectors
5. **Test feature ranges**: Verify each feature type produces meaningful variance

### Common Multi-Feature Pitfalls
- **Correlation issues**: Highly correlated features don't add information
- **Missing data**: NA values can propagate through calculations
- **Temporal misalignment**: Features from different time periods
- **Scale domination**: One feature type overwhelming others due to magnitude

## Prevention Strategies

### 1. Input Validation
- Always check vector norms before distance calculations
- Log vector statistics during development
- Include at least one guaranteed non-zero element
- Validate feature ranges and distributions

### 2. Robust Feature Engineering
- Use relative measures (returns, ratios) instead of absolute values
- Apply appropriate scaling for each feature type
- Avoid overly complex conditional feature generation
- Test with edge cases (market gaps, low volume, extreme volatility)

### 3. Defensive Distance Functions
- Handle edge cases (zero vectors, identical vectors)
- Use epsilon comparisons instead of exact zero checks
- Consider fallback distance measures
- Implement bounds checking on results

### 4. Testing Framework
- Create unit tests with known vector pairs
- Test edge cases (zero vectors, identical vectors, orthogonal vectors)
- Validate that different market conditions produce different distances
- Monitor distance distribution over time

## Language-Specific Notes

### PineScript
- Use `:=` for variable assignment in functions, not `=`
- `nz()` function helps handle NA values
- Series access like `source[1]` looks backward in time
- `ta.` functions may return NA for insufficient data

### Python/NumPy
- Use `np.linalg.norm()` for vector norms
- Check for `np.isnan()` and `np.isinf()` in results
- Consider using `scipy.spatial.distance` functions
- Handle pandas DataFrame NA values with `fillna()`

### General Debugging
- Floating-point precision issues can cause unexpected zeros
- Large scale differences between features can cause numerical issues
- Always validate assumptions about data ranges and distributions
- Use version control to track when distance calculations stopped working

## Quick Fix Checklist

1. ✅ **Log vector norms** - Are they > 0?
2. ✅ **Check feature generation** - Do individual features have variance?
3. ✅ **Validate distance function** - Test with known vectors
4. ✅ **Review variable assignment** - Correct syntax for the language?
5. ✅ **Add baseline non-zero element** - Prevent zero vectors
6. ✅ **Scale features appropriately** - Avoid one feature dominating
7. ✅ **Test edge cases** - What happens with identical/zero inputs?