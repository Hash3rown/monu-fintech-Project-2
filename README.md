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


<br />

## 2. Building Indicators


<br />

### 2.1 Building EMA Indicators
<br />

The construction of an EMA variable is pretty straightforward. Both the `9vs20 EMA` and the `50 Vs 200 EMA` strategies give signals based on the value of the **fast signal** (i.e. 9 or 50) relative to that of the **slow signal** (i.e. 20 or 200). When the 'fast signal' is above the 'slow signal', there's said to be a buying/long opportunity. Inversely, when the slow signal is higher than the fast signal, there's said to be a 'selling/shorting' opportunity.

<br />

From there, it's a simple matter of programming the position signal for both indicators to `1` when there is a long opportunity, and `-1` if there is a short opportunity.

<br />

**EMA9V20**

### 2.2 Bollinger Bands

<br />

### 2.3 MACD

<br />

### 2.2 Relative Strength Index (RSI)

<br />
<br />

***

<br />


## 3. Building Strategies


<br />

### 3.1 Strategy 1: EMA50V200 + Bollinger Bands


<br />

### 3.2 Strategy 2: RSI + MACD


<br />

### 3.3 Strategy 3: EMA50V200 + MACD


<br />

### 3.4 Strategy 4: EMA50V200 + EMA9V20

<br />
<br />

***

<br />

## 4. Machine Learning Model Development