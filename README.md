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

## Configurable Parameters

Users can adjust the following parameters inside TradingView's settings panel:

- **EMA Length**: Default - 3, 5, 7, 9, 20. Lookback period for the moving average.
- **RSI Length**: Default - 7, 9, 12. Lookback period for RSI
- **Conversion Line Length**: Default - 7. Lookback period for conversion line.

--

## How to Install & Use in TradingView

1. Open any crypto chart (e.g., `BTC/USDT`) on **[TradingView](https://www.tradingview.com/)**.
2. Open the **`Pine Editor`** console at the bottom of the page.
3. Open `indicator.pine` from this repository, copy the source code, and paste it into the editor.
4. Click **`Save`** and then click **`Add to Chart`**.
5. Click the gear icon (`Settings`) on the indicator to adjust parameters as needed.

--

## Key Learnings & Engineering Reflections

1. **The Role of Conversion Line in Ichimoku**
  - I learned that the conversion line shows the trend and local momentum. It is a helpful tool for understanding the chart's behavior over the chosen period. It can be used to filter out spurious signals caused by price spikes.

2. **Data Collecting From Previous Bars**
   - I learned that by comparing the current location of EMAs with the previous location, I can check if it rose or fell. This is useful for checking any crosses between indicators. It could also be used to detect crossovers between different indicators, such as the conversion line and the EMA.
