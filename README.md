# Algorithmic_Bitcoin_Trading
Capstone Project, DSI-5 SF

[INSERT PHOTO]

## Bitcoin: An Introduction 
At bottom, Bitcoin is a “[digital asset](https://www.techopedia.com/definition/23367/digital-asset).” Specifically, it is a “virtual currency” that uses cryptography for security. For this reason, you may hear Bitcoin referred to as "[cryptocurrency](http://www.investopedia.com/terms/c/cryptocurrency.asp).” Compared to traditional “fiat” currency that is controlled by a central authority (i.e., government), Bitcoin’s peer-to-peer system provides for [virtually untraceable](https://bitcoinmagazine.com/articles/how-bitcoin-users-reclaim-their-privacy-through-its-anonymous-sibling-monero-1472761633/) and untaxable transactions. 

The idea for Bitcoin first emerged in November 2008 in a paper entitled “Bitcoin: A Peer-to-Peer Electronic Cash System.” The identity of the purported author, Satoshi Nakamoto, is still unknown as of this writing. (See, e.g., *[Who is Satoshi Nakamoto?](http://www.coindesk.com/information/who-is-satoshi-nakamoto/)*). 

In this now famous paper, Satoshi addresses “the inherent weaknesses” of our current financial system. ([Bitcoin: A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf).) Specifically, Satoshi points out that our current system “rel[ies] almost exclusively on financial institutions serving as trusted third parties to process electronic payments.” In Satoshi’s opinion, “[w]hat is needed is an electronic payment system based on cryptographic proof instead of trust.” To this end, Satoshi outlines a currency system that affords its users more security and privacy than any system to come before it.

All Bitcoin transactions take place on a global financial ledger know as the “blockchain.” The blockchain is maintained and updated by a network of thousands of computers around the world. What is more, this design also effectively solves the [double-spending problem](http://www.investopedia.com/terms/d/doublespending.asp) -- a primary concern with any digital currency --  by “using [this] peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions.” This global network “is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.” Simply put, so long as there are more good guys than bad guys supporting the Bitcoin blockchain, your virtual currency is safe. 

Even more, the blockchain design set forth by Satoshi can oversee the exchange of not only cash, but anything that holds value, including stocks, bonds, and futures as well as houses and car titles. The blockchain has been adopted by the New York Stock Exchange, the Nasdaq stock exchange, IBM, and online retail giant Overstock.com to name a few. These companies and many more are currently building systems that can use the blockchain in this way.(*[10 Stock and Commodities Exchanges Investigating Blockchain Tech](http://www.coindesk.com/10-stock-exchanges-blockchain/)*, *[Bitcoin and the Future of the Blockchain in International Payment Systems](https://www.americanexpress.com/us/content/foreign-exchange/articles/bitcoin-future-of-blockchain-in-international-payments-systems/)*, *[IBM Launches effort to expand blockchain technology to supply chains](http://www.marketwatch.com/story/ibm-launches-effort-to-expand-blockchain-technology-to-supply-chains-2016-07-14)*, *[Blockchain: Look Toward Blue-Chip Technology Companies for Investment](https://seekingalpha.com/article/4042182-blockchain-look-toward-blue-chip-technology-companies-investment)*.)

**Bitcoins are not printed** as in the case of traditional, fiat money. Instead, **Bitcoins are “mined.”** Mining is nothing more than solving a large number of cryptographic hash functions (math problems) very quickly. This mining process facilitates actual Bitcoin transactions, and as a reward for supporting the Bitcoin blockchain, successful miners are rewarded with -- wait for it -- Bitcoin! 

This process also affords more security against fraud than our traditional bank-based electronic payment system. Since each new blockchain hash contains information about all previous Bitcoin transactions, altering a transaction -- i.e., **cheating at Bitcoin** -- would require re-computing the transaction to be altered and all subsequent transactions before the next block of transactions in the blockchain is computed -- **a virtually impossible task**.

## Bitcoin Trading

Perhaps due to the virtual anonymity and reliable security provided by Bitcoin, since 2016, the price of Bitcoin has exploded. The price in April 2017 is nearly [three times greater](http://www.coindesk.com/price/) than the price less than one year ago. Somewhat famously, Laszlo Hanyecz, a programmer, paid a fellow BitcoinTalk forum user 10,000 Bitcoins for two Papa John’s pizzas in November of 2010. Back then – when the technology was just over a year old – that amounted to roughly $25. As of this writing in April 2017, those two pizzas cost more than $13 million dollars.

With this explosion in Bitcoin’s price, naturally, [Bitcoin trading](http://www.forexnews.com/bitcoin-trading/) is on the rise. At present, there are more than 130 cryptocurrency exchanges around the globe with a total 24-hour volume of roughly 143,521.06 in Bitcoin (over $179 million in USD). New products and services are emerging every day including margin trading, futures, derivatives, lending, and more.
 
For these reasons, I selected algorithmic Bitcoin trading as the subject of my analysis.

### Step One: Predict the Price of Bitcoin

#### Feature Selection: Market and Mining Data

In an attempt to find data that may serve as reliable predictors of Bitcoin’s price, I first looked to [Bitcoin market data](https://www.quandl.com/collections/markets/bitcoin-data) such as the total number of Bitcoins in circulation, the Bitcoin market capitalization (total number of Bitcoins multiplied by the price of Bitcoin), the number of Bitcoin addresses, the Bitcoin transaction volume, and more. I gathered historic data via Quandl, and that data is updated daily via web scrapers scheduled to run up to every 15 minutes. All data is written to a MySQL database, from which it is later queried for backtesting and analysis via Python, SQLAlchemy, and Pandas.  

##### Figure 1. Bitcoin Market Data
 
| **FEATURE**                                        | **DESCRIPTION**                                                    |
|----------------------------------------------------|--------------------------------------------------------------------|
| Total BTCs                                         | Total Bitcoins mined (in circulation)                              |
| BTC Market Cap                                     | Total Bitcoins in circulation multiplied by Bitcoin price in USD   |
| BTC Addresses                                      | Number of unique bitcoin addresses used per day                    |
| BTC Estimated Transaction Volume                   | Similar to  total output vol.; more accurate reflection of Tx vol  |
| BTC Exchange Trade Volume                          | USD trade volume from the top exchanges                            |
| Number Bitcoin Transactions                        | Total number of unique Bitcoin transactions per day                |
| Total Bitcoin Transactions                         | Total volume of bitcoin transactions                               |
| Total Transactions Less the Most Popular Addresses | Total number of unique bitcoin Txs/day excluding top 100 pop addrs | 
| Transactions Per Block                             | The average number of transactions per block                       |
| Average Transaction Confirmation Time              | Daily median time take for Txs to be accepted into a block         |
| Total Transaction Fees BTC                         | The total value of transaction fees miners earn per day in BTC     |
| Total Transaction Fees USD                         | Total Tx fees miners earn per day in $USD                          |
| Cost Per Transaction                               | Miners revenue divided by the number of transactions               |
| Cost as Percent Volume                             | Miners revenue as as percentage of the transaction volume          |

The correlation between these Bitcoin market features and the price of Bitcoin can be seen the heatmap below.

##### Figure 2. Bitcoin Market Data Correlation

[INSERT HEATMAP]

After studying Bitcoin market data, next, I explored the predictive power of Bitcoin mining data. Again turning to Quandl, I used the features set forth below together with the market features listed above.

##### Figure 3. Bitcoin Mining Data

| **Feature**                                        | **Description**                                                    |
|----------------------------------------------------|--------------------------------------------------------------------|
| Total Blockchain Size                              | Total size of all block headers and transactions                   |
| Average Block Size                                 | The average block size in MB                                       |
| Total Output Volume                                | Total value of all Tx outputs per day including coins returned     |
| Hash Rate BTC                                      | # of giga hashes/sec  BTC network is performing (billions)         |
| Miners Revenue                                     | (Bitcions mined per day + TX fees) * Bitcoin market price          |
| Difficulty                                         | How difficult it is to find a hash below a given target            |

The correlation between these Bitcoin market features can be seen the heatmap below:

##### Figure 4. Bitcoin Mining Data Correlation 

[INSERT HEATMAP]

In light of the strong correlation between much of the market data and mining data, it came as no surprise that regularized regression produced a more accurate prediction that ordinary least squares regression.

### Feature Selection: Non-Market Data

Beyond the Bitcoin market and mining data discussed above, I also explored the relationship of the Bitcoin price to the daily number of tweets that mention Bitcoin, the sentiment of tweets mentioning Bitcoin, the number of searches on Google for the term “Bitcoin”, and the number of web page views for the Bitcoin page on Wikipedia.

As you can see by the somewhat parallel lines below, there appears to be a strong correlation between the price of Bitcoin and the volume of Google searches involving the word “Bitcoin.”

##### Figure 5. Number of Google Searches for “Bitcoin” and Bitcoin Price Correlation

[INSERT PLOT]

The correlation between the number of page views for the “Bitcoin” Wikipedia page and the Bitcoin price is significantly weaker. 

##### Figure 6. Page Views for Wikipedia’s “Bitcoin” Page and Bitcoin Price Correlation

[INSERT PLOT]

In the end, adding the Wikipedia page view trend to the feature set did not produce more accurate predictions.

Finally, I explored the relationship between daily Twitter activity and the Bitcoin price. First, I measured the relationship between the number of tweets mentioning “Bitcoin” each day and the Bitcoin price. 

##### Figure 7. Daily Non-Unique Tweets Mentioning “Bitcoin” and Bitcoin Price

[INSERT PLOT]

##### Figure 8. Daily Non-Unique Tweets Mentioning “Bitcoin” and Bitcoin Price

[INSERT PLOT]

As you can see, there is very little difference between the correlation of unique tweets to Bitcoin price and the correlation between non-unique tweets to Bitcoin price. In both cases, the correlation is negligible. Not surprisingly, neither feature added any accuracy to the model’s accuracy.

Finally, I examined whether there was a correlation between the sentiment of tweets and the price of Bitcoin -- that is, whether positive and negative tweets regarding Bitcoin cause the price of Bitcoin to rise or fall. To this end, I used a natural language processing (NLP) library specifically designed for use with Twitter data; i.e., text strings that contain slang and emoticons, and other casual conventions popular on Twitter.

##### Figure 7. Overall Tweet Sentiment and Bitcoin Price

[INSERT PLOT]

As the plot above demonstrates, there was very little correlation between the overall sentiment of daily tweets and the price of Bitcoin. Next, I analyzed positive tweet sentiment.

##### Figure 8. Positive Tweet Sentiment and Bitcoin Price

[INSERT PLOT]

##### Figure 9. Negative Tweet Sentiment and Bitcoin Price

[INSERT PLOT]

Ultimately, I found no significant correlation between the volume of tweets and Bitcoin price nor between the sentiment of those tweets and the Bitcoin price. In the end, the Twitter features were not added to the Bitcoin market and mining features discussed above.

#### Prediction Accuracy

In selecting a model or ensemble of models to use in predicting the price of Bitcoin, I first turned to Facebook’s latest creation, [Prophet](https://facebookincubator.github.io/prophet/static/prophet_paper_20170113.pdf). Prophet is a time-series forecasting module, that, according to Facebook, “has been a key piece to improving Facebook’s ability to create a large number of trustworthy forecasts used for decision-making and product features.”

At its core, Prophet ostensibly provides a number of benefits beyond simple linear regression including options to model weekly and yearly seasonality (e.g., school vs. vacation schedules), 
account for important holidays, specify changepoints where changes in time series are expected (e.g., new product launch), and more. Further, Prophet can handle “a reasonable number” of missing values and large outliers, as well as nonlinear growth curves in which a trends hits a natural limit or saturates. Although Prophet looks promising, I was unable to tune the Prophet model to predict with better accuracy than the regression models available from SciKit-Learn.

#### Figure 10. Prophet Prediction Accuracy Compared to LassoCV from SciKit-Learn

[INSERT PLOTS]

Using the Bitcoin market and mining features discussed above, I trained five regression models using the SciKit-Learn API. The [R-squared scores](https://en.wikipedia.org/wiki/Coefficient_of_determination) (or the “coefficient of determination”) indicate the proportion of the variance in the Bitcoin price that is predictable from the Bitcoin market and mining data. The R-squared scores returned during model training and evaluation ranged from 0.75 for Random Forests Regression to 0.96 for LassoCV.

##### Figure 11. R-Squared Scores From Model Training/Evaluation

[INSERT BAR CHART]

This accuracy carried over to actual backtesting as shown in the plots below. (The closer the dots are to the line, the more accurate the prediction of Bitcoin’s price.)

This accuracy carried over to actual backtesting as shown in the plots below. (The closer the dots are to the line, the more accurate the prediction of Bitcoin’s price.)

[INSERT PLOTS]

As show in the plots above, the most accurate model throughout backtesting was LassoCV. Thus, during backtesting LassoCV served as the Bitcoin price prediction that is factored into the trading algorithm.

### Step Two: Develop a Trading Algorithm

Because I do not have a background in finance or algorithmic trading, I selected a simple metric known as an “exponential moving average crossover” to identify buy and sell opportunities. An [exponential moving average](http://www.investopedia.com/terms/s/sma.asp#ixzz4fsDKnMGb) (EMA) is akin to a simple moving average (SMA) yet it gives more weight to recent prices. This Bitcoin market is [notoriously volatile](https://www.buybitcoinworldwide.com/volatility-index/). Thus, by using an EMA crossover, our price calculations [“hug” the price]( http://www.investopedia.com/terms/e/ema.asp) action tighter than a simple moving average, and therefore react quicker to market shifts.

##### Figure 12. Exponential Moving Averages in Bitcoin Market: Jan. 1, 2016 - April 30, 2017

[INSERT PLOTS]

As you can see from the plot above, when the short-term EMA (blue) moves below the long-term EMA (green), the yellow line representing Bitcoin’s price also drops. This indicates a sell opportunity. The opposite is also true. When the short-term EMA move above the long-term EMA, the yellow line representing Bitcoin’s price rises. With this simple strategy in place, I now had a trigger for buy and sell opportunities. 

Next, I sought to incorporate the predicted price of Bitcoin into the algorithm. Again, to keep things simple, I decided to buy only if both the short-term EMA is higher than the long-term EMA and the predicted price of Bitcoin at the closing bell is higher than the predicted price for the previous day. Accordingly, I sell only if both the short-term EMA is lower than the long-term EMA, and the predicted price of Bitcoin is lower than the previous day’s prediction.

With a strategy of when to buy or sell in hand, I next needed a rubric for determining how much to buy or sell. With this in mind, I decided to examine the degree of EMA crossover -- i.e., the degree to which the EMA short window crosses over the EMA long window -- and the trading volume. Measuring the degree of EMA crossover is simple, and it should indicate the extent to which the short term average is rising above or dropping below the long-term average thereby providing some guidance on whether this is a mediocre, good, or great opportunity.

Market volume also informs this quantity decision. The rationale is simple: a price drop (or rise) on little volume is not a strong signal; however, a price drop (or rise) on large volume is a stronger signal that something in the currency has fundamentally changed. Indeed, Bitcoin market volume is subject to big swings.

##### Figure 13. Bitcoin Exchange Trade Volume

[INSERT PLOTS]

In sum, the final trading algorithm works like so: if there is an EMA crossover, buy 10% to 100% of the user’s maximum trade -- an arbitrary number set by the user during backtesting -- based on 1) the degree of EMA crossover, and 2) [market volume](http://www.investopedia.com/articles/technical/02/010702.asp) compared to the average market volume for the past week and month.

### Step Three: Build a Backtesting Module

When researching how to best backtest my algorithm, I found a number of options for backtesting stock trading strategies such as [Quantopian](https://www.quantopian.com/posts/new-feature-comprehensive-backtest-analysis) and [PyAlgoTrade](https://pypi.python.org/pypi/PyAlgoTrade). However, I could not find a suitable option for backtesting Bitcoin trading strategies against historic Bitcoin prices. For this reason, I created a simple function to backtest my algorithm.

Aptly named “ema_backtest,” the function I created permits the user to set the duration for the EMA short and long windows, the backtest start date, the size of the portfolio in Bitcoins and $USD at the start of trading, whether to trade on margin, the maximum trade (e.g., 10 Bitcoins), and whether to print verbose output for subsequent analysis. After the user’s preferences are entered, the backtesting module, trains several regression models on historic Bitcoin data starting in 2013 up to the day before backtesting. Each iteration in the function’s main loop represents a day of trading, and for each day, the algorithm must decide to buy, sell, or hold. Predictive models are re-trained each day thereby adding an additional row of data to the training data as each day passes in an effort to increase prediction accuracy. The user’s portfolio is updated after every trade, and transactions fees are deducted in accordance the fees on the largest Bitcoin exchange, [GDAX](https://www.gdax.com/).

At the end of the backtesting, a scorecard is printed to the screen, and several DataFrames are returned for subsequent analysis and display. On average, the results generated from backtesting my algorithm have been exponentially better than simple buy-and-hold investing. Giving myself massive starting capital, and free reign to trade on margin produced returns more than 40 times better than buy-and-hold, and slightly more realistic starting values produced return 10 times greater than buy-and-hold. If starting with $10,000 USD and 1 Bitcoin, far more realistic values for most novice traders, the results were more than 3.5 times better than buy-and-hold investing.

##### Figure 14. Sample Scorecard

[INSERT IMAGE]

The figure below displays the points at which Bitcoins were purchased throughout backtesting. It affords the user an easy check on whether the algorithm is “buying low and selling high.”

##### Figure 15. Buy and Sell Orders and Bitcoin Price

[INSERT PLOT]

## Conclusions and Next Steps

Because this project pushed me out of my comfort zone into the world of time series forecasting and algorithmic trading, I found it both challenging and rewarding. Indeed, I enjoyed the project so much that I plan on continuing to develop my algorithm and trading strategies in my free time. To this end, I will add XGBoost and LSTM to see if either approach improves the accuracy of the Bitcoin price predictions. I will also continue to tune Prophet models to improve the accuracy of both short-term and long-term forecasts. After that, my goal is to plug in my algorithm to a sandbox account on an existing exchange (e.g., GDAX) to trade at a higher frequency using live-streaming order-book data.







