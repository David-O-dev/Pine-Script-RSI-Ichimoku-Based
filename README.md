# Pine-Script-RSI-Ichimoku-Based
Indicator based on RSI and conversion line from Ichimoku. Signals entry for long/short in futures for any cryptocurrency on Trading View.  

--

## Chart Preview
![Indicator Preview](RSI+FP-ss.png)

--

## Motivation & Problem
- **Market Noise & False signals**: The previous indicator yielded unstable and inaccurate results due to a lack of ability to check for the complete conversion of the trend.
- **The Core Goal**: To create an indicator that is trustworthy, improving the accuracy of reading the momentum and trend was necessary. The system still implied the **multi-timeframe momentum confirmation** and the merging of different indicators.

--

## Strategy Logic & Architecture
- This indicator avoids false signals and wrong interpretation of the trend by utilizing a **rule-based, multi-factor filtering system**:

### Core Components:
1. **Trend Filter**:
   - Uses multiple moving averages (e.g., EMA 20) to determine macro trend bias.
   - Brings data from previous bars to compare the location of the EMA. Look for the cross on any of the EMAs and set the associated bool to true.
   - Conversion line from Ichimoku used to detect the short-term trend.

2. **Multi-Timeframe RSI**:
   - Instead of relying solely on local RSI, the script pulls RSI data from a higher timeframe using 'request.security()'
   - Confirms that macro momentum supports the local price action.

3. **Execution Rule**
   - **Bullish Signal**: Triggers when one of the small-lengthed EMAs crosses a longer-lengthed EMA upwards in a 3-minute timeframe **and** the conversion line has a slope greater than 0 **and** the RSIs from multi-timeframes are all sorted from least to greatest length above 55.
   - **Bearish Signal**: Triggers when one of the small-lengthed EMAs crosses a longer-lengthed EMA downwards in a 3-minute timeframe **and** the conversion line has a slope less than 0 **and** the RSIs from multi-timeframes are all sorted from greatest to least length below 45.

-- 

## Key Visual Signals & Interpretation
| Visual Component | Chart Display | Technical Interpretation |
| :--- | :--- | :--- |
| **Bullish Entry Signal** | [예: Green Arrow / Label] | Trend aligned with higher-timeframe momentum expansion |
| **Trend Baseline** | [예: Blue / Orange Lines] | Dynamic support/resistance zones based on Moving Averages |
| **Chop / Noise Zone** | [예: Gray candles or Neutral line] | Conflicting signals between trend and RSI; avoid trading |
