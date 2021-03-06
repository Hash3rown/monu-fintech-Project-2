# Project 2 - Monash Fintech <br />
## Team: Wen G-Wagon <br />
### Sreeni, Briar, Tas & Alex <br />
#### [Please see Project_2_Wen_G_Wagon.pdf for project scope guidelines]
<br />
<br />
<br />

# Leveraging Technical Analysis Indicators and Machine Learning to Develop Passive Trading Strategies 
<!-- ![Leo](/images/Leololinbed.jpeg "Leo") -->
<img src='images/Leololinbed.jpeg' width=400><br>

## 1. Idea Development

We developed our idea by combining our week 15 learnings (on algorithmic trading) with key trading strategy logic popularly promoted by day traders on Youtube. There's a general consensus amongst the trading community that a good trading strategy should comprise of at least **three technical indicators.** 

**Technical Indicators We Explored**
* [EMA 50 vs 200](https://www.orbex.com/blog/en/2014/11/trading-200-50-ema-h4-time-frame-trading-strategy-2) 
* [EMA 9 vs 20](https://www.forexfactory.com/thread/89346-how-to-use-the-920-ema-setup-effectively)
* [MACD](https://www.investopedia.com/terms/m/macd.asp)
* [RSI](https://www.investopedia.com/terms/r/rsi.asp)
* [Bollinger Bands®](https://www.investopedia.com/terms/b/bollingerbands.asp)

<br />

After exploring several free APIs for sales data, we hit an early roadblock in that few sites provided more than 720 periods of data. On our first attempt to program a three-criteria trading strategy, there were very few instances in which you had 3x 'buy' or 3x 'sell' signals. As such, we agreed to **cull each strategy back to just two technical indicators.**

<br />

To make things fun, we each put forth our own ideas for trading strategies comprising of two technical indicators each:

| Strategy   | Owner    | Indicator 1     | Indicator 2     |
| ---------- |:--------:| ---------------:| ---------------:|
| 1          | Tas      | EMA50 vs EMA200 | Bollinger Bands |
| 2          | Briar    | RSI             | MACD            |
| 3          | Sreeni   | EMA50 vs EMA200 | MACD            |
| 4          | Alex     | EMA50 vs EMA200 | EMA9 vs EMA20   |


<br />

Unlike the week 15 activities, we also wanted our strategies to be able to **enter long positions as well as short positions**. This was a further nod to the popularly expressed opinion amongst the crypto day-trading community that you should only ever:
* Enter a **LONG** position (1) during a **BULL MARKET**
* Enter a **SHORT** position (-1) during a **BEAR MARKET**

<br />

After building and exploring all of our strategies, the question was asked whether we could leverage the indicators we had already built, to train some sort of **Machine Learning Trading Model**, and whether or not that model would outperform either of our strategies [To be explored at end].


<br />
<br />

***

***

***


<br />

## 2. Building Indicators

<br />

---

<br />

### 2.1 Building EMA Indicators
<br />

The construction of an EMA variable is pretty straightforward. Both the `9vs20 EMA` and the `50 Vs 200 EMA` strategies give signals based on the value of the **fast signal** (i.e. 9 or 50) relative to that of the **slow signal** (i.e. 20 or 200). When the 'fast signal' is above the 'slow signal', there's said to be a buying/long opportunity. Inversely, when the slow signal is higher than the fast signal, there's said to be a 'selling/shorting' opportunity. From there, it's a simple matter of programming the position signal for both indicators to `1` when there is a long opportunity, and `-1` if there is a short opportunity.

<br />

**Outcome of the EMA9V20**

<img src='images/2.1_indicator_9v20_ema.png' width=800><br>
<br />

**Outcome of the EMA50V200**

<img src='images/2.1_indicator_50v200_ema.png' width=800><br>

<br />

As you can see, the EMA50vs200 chart showed that in each of the past 720 periods, the slow 200EMA was above that of the 50EMA. This indicates that for the strategies utilising this indicator, only short / sell opportunities would exist. By contrast, the 9vs20EMA shows a lot more buy and sell signals.

<br />

---

<br />

### 2.2 Bollinger Bands

<br />

With Bollinger Bands, you get a 'buy' signal the second the price action drops over one standard deviation below the current 20-period rolling mean closing price. Similarly, 'buy' signals appear when the price drops below one standard deviation below. 

However, after backtesting this signal a few times, we realised that entering a trade the instant price moves outside the 1stdev range is a recipe for trouble. Often, it is simply the first of 3 to 5 periods in which the price continues to rise/fall outside of the range. The implication here being, if you enter a lopng period on the first signal, price is likely to continue to fall, costing your bottom line.

As such, we updated our 'signals' code to only provide a buy/sell signal on the first candle in which price returns back into the `MEAN +- 1 STD` range. Furthermore, as buy/sell signals disappear quickly, the 'signal / position'.


<br />
<img src='images/2.2_bollinger.png' width=800><br>

<br />

---

<br />


### 2.3 MACD

<br />

The MACD is renowned for being amongst the most popular and widely-used trading indicators. The MACD value/line itself is derived from taking the 26-period EMA from the 12-period EMA. This can be seen as the 'fast signal'. 

Importantly, the macd is plotted in relation to the 9-period EMA 'Signal' line (Read: slow signal). When the MACD line is above the signal line, a buying opportunity exists. When it is below, a selling opportunity exists.

Importantly, many day-traders utilise the MACD for **confirmation.**. I.e. If they believe, based on other indicators, that a surge/drop is coming, they will WAIT for the relevant macd crossover prior to entering the trade.

<br />

<img src='images/2.3_macd.png' width=800><br>

<br />

---

<br />

### 2.4 Relative Strength Index (RSI)

<br />

The RSI is a popular momentum indicator used to determine when a market is overbought or oversold. The exact mathematics behind our code can be viewed [here.](https://www.investopedia.com/terms/r/rsi.asp). Generally speaking though, when the RSI hits a value below 30, it is deemed to be 'oversold' and thus 'undervalued'. Hence, a long/buy opportunity may exist in anticipation of an upcoming boost in price. Conversely, when the RSI is above 70, it is deemed to be 'overbought' and hence the price is 'overvalued'. As such, a short/sell opportunity may exist in expectation of an upcoming correction (price drop).

From a programming perspective, the RSI can be viewed as being quite similar to Bollinger bands. For example, it isn't recommended to enter a long position the instant the RSI drops below 30, as the trend which pushed it below may continue for several more periods, pushing price down further and further. This would lead to instantaenous unrealised losses. If the RSI was being used solely as an 'entry signal', it would be wiser to time the entry on the first period in which the RSI closes above 30 once again.

However, in this instance, we aim to use the RSI in place of a 'macro trend indicator' only for strategy 2 (in combination with the MACD). Many day traders use the RSI as an initial suggestion that a smart trade may be coming up, before waiting for a **confirmation** to actually enter the trade. As an example, if the RSI breaches 70, this might suggest to a trader that a short/sell opportunity may arise soon. They would then wait for a confirmation in the short few periods following. In our case, the `macd signal line` crossing above the `macd`. 


<img src='images/2.4_macd_rsi_strategy_example.jpeg' width=800><br>


As such, we programmed the RSI, such that buy/sell signals would linger for several periods following the RSI re-entering a 'fair' price range. The results can be seen in as follows:


<br />
<br />

<img src='images/2.4_rsi_graphs.png' width=800><br>


<br />
<br />

***

***

***

<br />
<br />

## 3. Building Strategies

After building the signals, the next task was to build the strategies. A quick refresher on what they were:


| Strategy   | Owner    | Indicator 1     | Indicator 2     |
| ---------- |:--------:| ---------------:| ---------------:|
| 1          | Tas      | EMA50 vs EMA200 | Bollinger Bands |
| 2          | Briar    | RSI             | MACD            |
| 3          | Sreeni   | EMA50 vs EMA200 | MACD            |
| 4          | Alex     | EMA50 vs EMA200 | EMA9 vs EMA20   |


<br />

***

<br />


### 3.1 Strategy 1: EMA50V200 + Bollinger Bands

<br />

#### **3.1i) Strategy 1: Building Logic**
 
 Combining the EMA50V200 indicator with the Bollinger bands wasn't as straightforward as it sounds. Comparing the current Bollinger position to the previous (1-period shifted) value was paramount in terms of determining the logic for the strategy.

|#   | EMA50v200 | BOLL | BOLLSHIFT       | INTERPRETATION                               |
|----| ----------|:----:| ---------------:|---------------------------------------------:|
|1   | -1        |  1   |   1             |  HOLD NO POSITION                            |
|2   | -1        |  1   |   0             |  HOLD NO POSITION / CLOSE SHORT POSITION     |
|3   | -1        |  1   |   -1            |  CLOSE SHORT POSITION                        |
|4   | -1        |  0   |   1             |  HOLD NO POSITION                            |
|5   | -1        |  0   |   0             |  HOLD POSITION                               |
|6   | -1        |  0   |   -1            |  HOLD SHORT POSITION                         |
|7   | -1        |  -1  |    1            |  CLOSE LONG, ENTER SHORT                     |
|8   | -1        |  -1  |    0            |  ENTER SHORT                                 |
|9   | -1        |  -1  |  -1             |  HOLD SHORT                                  |
|10  | 1         | 1    | 1               |  HOLD LONG                                   |
|11  | 1         | 1    | 0               |  ENTER LONG                                  |
|12  | 1         | 1    | -1              |  CLOSE SHORT, ENTER LONG                     |
|13  | 1         | 0    | 1               |  HOLD LONG                                   |
|14  | 1         | 0    | 0               |  HOLD POSITION                               |
|15  | 1         | 0    | -1              |  HOLD POSITION                               |
|16  | 1         | -1   |   1             |  CLOSE LONG                                  |
|17  | 1         | -1   |   0             |  HOLD POSITION / CLOSE LONG                  |
|18  | 1         | -1   |   -1            |  HOLD NO POSITION                            |

<br />

Fortunately, in 11 of the above 18 possible scenarios, the interpretation is to hold. This was easily accounted for with an 'else' statement (i.e. else: position = previous position). This meant that only scenarios 3,7,8,11, 12 and 16 required specific programming.

<br />

#### **3.1ii) Strategy 1: Visualisation**

The below graph showcases the EMA50v200 signals, the bollinger signals, as well as the positions taken based on the two signals coinciding.


<br />


<img src='images/3.1_ema50v200_boll_strategy.png' width=800><br>


<br />

As you can see above, as the EMA50v200 line was negative for most of the time period, and there was only a brief window at the start when long opportunities were said to have existed. Thus, for the rest of the time period, only shorting opportunities arose.

#### **3.1iii) Strategy 1: Backtesting**

<br />

<img src='images/Tas_charts/Strat1.png' width=800><br>

[*** Sreeni, this is where you talk about backtesting]

<br />

---

<br />

### 3.2 Strategy 2: RSI + MACD

<br />

#### **3.2i) Strategy 2: Building Logic**

<br />

The MACD + RSI Strategy was similarly straightforward to put together. 

<br />

|#   | RSI | MACD | MACDSHIFT(-1)    | INTERPRETATION                               |
|----| ----|:----:| ----------------:|---------------------------------------------:|
|1   | -1  |  1   |    1             |  HOLD NO POSITION                            |
|2   | -1  |  1   |   -1             |  CLOSE SHORT                                 |
|3   | -1  |  -1  |    1             |  ENTER SHORT                                 |
|4   | -1  |  -1  |   -1             |  HOLD SHORT                                  |
|5   |  0  |  1   |    1             |  HOLD POSITION                               |
|6   |  0  |  1   |   -1             |  NO POSITION OR CLOSE SHORT                  |
|7   |  0  |  -1  |    1             |  NO POSITION OR CLOSE LONG                   |
|8   |  0  |  -1  |   -1             |  HOLD POSITION                               |
|9   |  1  |  1   |    1             |  HOLD LONG                                   |
|10  |  1  |  1   |   -1             |  ENTER LONG                                  |
|11  |  1  |  -1  |    1             |  CLOSE LONG                                  |
|12  |  1  |  -1  |   -1             |  HOLD NO POSITION                            |


<br />

Similarly to the scenario prior, of the total possible scenarios, 6 scenarios required simply holding the pre-existing position (#1, #4, #5, #8, #9 and #12) - easily coded into the 'else' of the if section. As such, we only had to program 6 other unique scenarios.

<br />

#### **3.2i) Strategy 2: Visualisations**

<br />

The below graph showcases the RSI signals, the macd signals, as well as the positions taken based on the two signals coinciding.


<br />


<img src='images/3.2_rsi_macd_strategy.jpeg' width=800><br>

#### **3.2i) Strategy 2: Backtesting**

<img src='images/Tas_charts/Strat2.png' width=800><br>
<br />


<br />

---

<br />



### 3.3 Strategy 3: EMA50V200 + MACD

<br />

#### **3.3i) Strategy 3 Building Logic**

<br />


|#   | 50v200 | MACD | MACDSHIFT(-1)    | INTERPRETATION                        |
|----| -------|:----:| ----------------:|--------------------------------------:|
|1   | 1      |  1   |    1             |  HOLD LONG POSITION                   |
|2   | 1      |  1   |    0             |  ENTER LONG POSITION                  |
|3   | 1      |  1   |    -1            |  CLOSE SHORT, ENTER LONG POSITION     |
|4   | 1      |  0   |    1             |  HOLD POSITION                        | 
|5   | 1      |  0   |    0             |  HOLD POSITION                        |
|6   | 1      |  0   |    -1            |  HOLD POSITION                        |
|7   | 1      | -1   |    1             |  NO POSITION / CLOSE LONG             |
|8   | 1      | -1   |    0             |  NO POSITION / CLOSE LONG             |
|9   | 1      | -1   |    -1            |  HOLD NO POSITION                     |
|10  | -1      |  1   |    1            |  HOLD NO POSITION                    |
|11  | -1      |  1   |    0            |  NO POSITION / CLOSE SHORT           |
|12  | -1      |  1   |    -1           |  NO POSITION / CLOSE SHORT           |
|13  | -1      |  0   |    1            |  HOLD POSITION                       |
|14  | -1      |  0   |    0            |  HOLD POSITION                       |
|15  | -1      |  0   |    -1           |  HOLD POSITION                       |
|16  | -1      | -1   |    1            |  CLOSE LONG, ENTER SHORT POSITION    |
|17  | -1      | -1   |    0            |  ENTER SHORT POSITION                |
|18  | -1      | -1   |    -1           |  HOLD SHORT POSITION                 |

<br />

As you can see, 8 scenarios (#1, #4, #5, #6, #9, #10, #13, #14, #15, #18) required simply holding onto the existing position. So, we only had to program 8 additional scenarios.

<br />

#### **3.3i) Strategy 3: Visualisation**

<br />

The below graph showcases the EMA50v200 signals, the macd signals, as well as the positions taken based on the two signals coinciding.


<br />


<img src='images/3.3_ema50v200_macd_Strategy.png' width=800><br>

<br />

#### **3.3i) Strategy 3: Backtesting**

<br />

<img src='images/Tas_charts/Strat3.png' width=800><br>
---

<br />

### 3.4 Strategy 4: EMA50V200 + EMA9V20

<br />

#### **3.3i) Strategy 4 Building Logic**

<br />

|#   | 50v200 | 9V20 | 9V20SHIFT    | INTERPRETATION                       |
|----| -------|:----:| ------------:|-------------------------------------:|
|1   | 1      |  1   |    -1        |  ENTER LONG POSITION                |
|1   | 1      |  1   |    1         |  HOLD LONG POSITION                 |
|1   | 1      |  -1  |   -1         |  HOLD NO POSITION                   |
|1   | 1      |  -1  |   1          |  CLOSE LONG POSITION                |
|1   | -1     |  1   |    -1        |  CLOSE SHORT POSITION               |
|1   | -1     |  1   |    1         |  HOLD NO POSITION                   |
|1   | -1     |  -1  |   -1         |  HOLD SHORT POSITION                |
|1   | -1     |  -1  |   1          |  ENTER SHORT POSITION               |

Strategy four had the easiest logic to program, as both signals only have two possible values (-1 and 1). As such, outside of the 'hold' scenarios (position='existing position'), only four scenarios had to be built.

<br />

<br />


#### **3.3i) Strategy 4 Visualisation**

<br />

The below graph showcases the EMA50v200 signals, the EMA9V20 signals, as well as the positions taken based on the two signals coinciding.

<br />


<img src='images/3.4_ema50200+ema920_strategy.png' width=800><br>

<br />


#### **3.3i) Strategy 4 Backtesting**

<br />

<img src='images/Tas_charts/STrat4.png' width=800><br>

***

<br />

## 4. Machine Learning Model Development

We elected to pit machine learning against traditional algo trading to see if a more profitable model would result.

Initially attempted to feed our buy/sell indicators (-1, 0, 1) into a random forest classifier, attempting to predict positive hourly returns, essentially signalling a buy when a positive return was predicted and a sell when a negative return predicted. The Random Forest model performed poorly against test data (heavily weighted toward positive return.

From there we changed our approach and used our raw indicator data (EMA/MACD/hourly volume etc) and had a much better result. We created a column signalling  1 for an hourly return > 0 and shifted the column by one row in order to train the model to predict when the NEXT period would see a price increase. 

![image](https://user-images.githubusercontent.com/77593180/121496777-1f6d0480-ca1e-11eb-9ce7-0c6ad65a39e5.png)


We tried multiple combinations of indicators as our input data and found the best performance came from using the full set of exponential moving averages. The model accuracy when tested against the actual signal data was .50 (weighted average).

A Support Vector Machine (SVM) model was then used with the same input data and returned a better result (.57 weighted accuracy)

The predictions from the best performing model were then used to calculate an hourly return simulating the result of purchasing only when the model predicted a positive return. This compared well to the actual hourly return on LINK.

<img src='images/Tas_charts/RFcumret.png' width=800><br>

When backtested using shorts and longs on Chainlink PA over 720 periods the model made 13 trades and was in profit (approx 3%). This was a good result given the asset under test was in a macro bearish downtrend. From a risk management point of view it was great to see that we minimised loss and outperformed the simple process of buying and holding.

<img src='images/Tas_charts/StratML.png' width=800><br>

It did not however outperform our most profitable algorithmic trading strategies.

<img src='images/Tas_charts/StratPerf.png' width=800><br>

Given the model could only be applied to a test dataset (30% of the overall data), it will be interesting to see how it continues to perform into the future.
