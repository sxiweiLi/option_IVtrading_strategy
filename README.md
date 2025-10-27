Here is what the three metrics mean (avg_volume, iv30_rv30, and ts_slope_0_45) in the code in order to filter out low quality trades:

1. Term Structure Slope (ts_slope_0_45):
   
What is it? - THe slope of the IV curve across different option expiration dates.
slope = ((IV at 45 days) - (IV at expiration)) / ((45) - (DTE))
What does this mean?
- Negative slope --> short term IV is higher than long-term IV --> indicates near-term uncertainty (favorable)
- Positive slope --> long-term IV higher --> market expects uncertainty later
Generally, more negative ts_slope = better

2. IV/RV ratio (iv30_rv30)

What is it? - Describes market's expectation of future volatility (from option prices) versus historical volatility calculated from the stock's price movements
What does this mean?
- Greater than 1 --> options are expensive relative to historical volatilty --> market expects more volatility than past
- Less than 1 --> options are cheap relative to past volatilty --> market expects calmer conditions
Generally, higher IV / RV ratio = better

3. 30-Day Average Volume (avg_volume)
What is it? - Average daily trading volume of the stock for the past 30 days. Ensures liquidity for trading options.
High volume = easy to execute trades + low bid/ask spread

Term Structure Slope Threshold (ts_slope_0_45 <= -0.00406)
- Checks if IV curve is downward sloping
- Historical backtesting: slopes steeper than -0.004 are more predictive of near-term volatility events
- Catch subtle backwardation

IV / RV ratio threshold (iv30_rv30 >= 1.25)
- Checks whether IV is significantly higher than RV
- Market expects 25% more volatility than historical volatility - shows short term volatility
- Identify overpriced options

30 Day average volume threshold (avg_volume >= 1,500,000)
- Ensures options are liquid enough to trade without huge bid-ask spreads
- Stocks with lower volume may have wide spreads or gapped fills making options harder to trade

Trade Execution:

Enter 1 short call (nearest DTE) + 1 long position call (>45 days out)
Calendar Spread Logic:
- Sell near-term option (premium high due to backwardation/near-term IV spike)
- Buy long-term option (premium lower relative to potential movement)
- Stock stays ATM --> short-term option decays faster than long-term option, short option loses value = profit
- Short is deep ITM --> short contract loses value
- Short is deep OTM --> long contract loses value (able to rebound due to lower theta decay)
