## Identifying Suspicious Trades Among U.S. Senators


# Overview
This README contains a brief overview and background of the project. If you would like to jump straight to the analysis, there are 3 files to read through. They are organized chronologically by number, however most of the interesting analysis is done in Outlier Analysis.ipynb.<br> <br> 1. __Collect Senator Data.ipynb__: Import and quickly analyze senator stock trades. <br><br>   2. __Stocks/Collect Stock Data.ipynb__: Collect different types stock data to be used in gain/loss analysis. Stock pricing data is collected by an R script I (Paul McCabe) made called Stocks/Import-Stocks.Rmd.<br><br> 3. __Outlier Analysis.ipynb__: Analyze senator profits using a combination of graphical representations, basic statistical analysis, and unsupervised machine learning algorithms. 


Alternatively, HTML versions of these jupyter notebooks exist with the same names.


# Motivations


This project by my partner Ankush and I (Paul McCabe) was motivated largely by the news at the time of U.S. senators being investigated for making "suspicious" stock trades (April 2020). Several senators made multi-million dollar sales right before the 2020 stock market plunge, a few directly opposite of the positive sentiment they expressed about the market. We wished to use a data-driven approach to see exactly how unusual these kind of trades were as well as how profiteable they were. <br><br> Since then, some of these senators have been investigated further by the SEC, some noticeable in our analysis (Kelly Loeffler) and others not (Richard Burr). 


# Written Report


![Screen%20Shot%202020-07-08%20at%201.00.33%20PM.png](https://github.com/pkm29/big_data_final_project3/raw/master/picture1.png)


A full report of this project written by Ankur and I is available [here](https://medium.com/@pkm29/identifying-suspicious-trading-among-u-s-senators-91a908432843). (15 minute read) <br> While Ankur's contributions were mostly towards writing the report and generating experimental data for future work, all code that appears in this document was written by me.


# Data Sources


![Artboard%201.png](https://github.com/pkm29/big_data_final_project3/raw/master/Artboard%201.png)


#### Senator Stock Trading Data - senatestockwatcher.com
Senators are required to report their trades either by filing a paper report or an electronic report to efdsearch.senate.gov. Data used in our analysis was compiled by the owner of the website [senatestockwatcher.com](https://senatestockwatcher.com) into a csv file. Inconsistencies and missing information remained an issue, as well as the fact that paper filed reports were not included. Senator Richard Burr does not appear in our analysis for this reason, however recent websites have begun to digitize these reports manually, GovTrades.com being one of them.


#### Stock Price Data - RStudio and the library 'quantmod'
Downloading unlimited free stock pricing data is difficult to come by, however RStudio and the library called quantmod does so, albeit at a different kind of cost. Quantmod pulls from yahoo finance but does so rather slowly and with around a 25% error rate. 


#### Stock Industry Data - NYSE and NASDAQ website
While researching this topic, we found an analysis that compared senator stock trades to the average stock gains of similiar sized companies in the same industry. CSV files with each company's industry and market cap were collected from the NYSE's website and the NASDAQ's webite, then incorporated into our analysis. 


# Quick Summary of Results (same content as Medium.com article)
## Box Plot
Our analysis centers upon relative returns (percentage returns) as opposed to absolute (dollar value) returns in order to ensure comparability across a wide range of trade sizes. The dollar amount per trade is far from irrelevant though and will likely be highly important for future analysis. <br> <br>
Recall that when measuring returns, we will investigate the future gains of stock purchases, and the future avoided losses of stock sales. This means if a stock plummets after being sold, it needs to be recorded as a large positive percentage return, and vice versa. In order to achieve this, percentage movements following a sale are multiplied by -1.


We define six different variables in order to capture percentage returns across periods of varying length, with multiple variables per period (by using different prices for the start price and finish price). This is because every stock hits four key prices each weekday on the stock exchange — the high, low, open, and close. Our six variables are as follows:<br><br>
__high_low_day__: the % change between the highest and lowest price of the transaction day.<br>
__open_close_day__: the % change between the open price and the close price on the transaction day.<br>
__open_open_week__: the % change between the open price on the transaction date and the open price a week later. <br>
__close_close_week__: the % change between the closing price on the transaction date and the closing price a week later. <br>
__open_open_month__: the % change between the open price on the transaction date and the open price a month later. <br>
__close_close_month__: the % change between the closing price on the transaction date and the closing price a month later. <br>
Below, we summarize the returns for each of these variables across all the senator trades we are analyzing (4474 entries).


![picture.png](image2.png)


Here we can see that as time passes, the percent gains and losses of a stock increase. Below are the precise returns values for various summary statistics:
![](image3.png)


Interestingly enough, the median is significantly lower than mean for the month long stock duration, indicating the distribution is strongly skewed to higher returns.
These box plots show us that the range of transaction returns varies greatly from the mean and IQR values. One potential measure of suspiciousness is if a senator makes many trades in the 4th quartile range (if many of their trades strongly outperform the average senator’s trade). Before that though, let’s quickly see which senators are making the top trades percentage wise for our 3 different time intervals.
![](image4.png)


From these 3 tables, we notice that David A Perdue Jr. has many of the highest return trades on transaction day, but Pat Roberts, Sheldon Whitehouse, Susan M Collins, and Kelly Loeffler have many high return trades after a week and a month. Note that David A Perdue has the highest number of transactions by far, so his activity is perhaps not unusual. Below are each of their total transactions: <br> <br>
David A Perdue , Jr: 1726 <br>
Pat Roberts: 249 <br>
Sheldon Whitehouse: 524 <br>
Susan M Collins: 389 <br>
Kelly Loeffler: 83 <br> <br>
As you can see, everyone else’s number of trades are much lower than David’s. Kelly Loeffler in particular has nearly 15% of her trades in the top 2.2% of all senator trades.


## Categorizing 4th Quartile Trades
Here the terminalogy will be slightly confusing. Essentially we are taking the 6 measures of trade profit from before, 1 day 1 week and 1 month with 2 measures for each time duration, and counting the number of times a Senator makes a trade in the top quartile. Then we compare that to their overall number of trades. As a baseline, we would expect that on average, 25% of trades for each senator would be in the top quartile. 


Instead of looking at the whole data frame though, let's examine how our senators perform for the 1 day, 1 week, and 1 month trade returns.
![](transactionday.png)
To reiterate, this table shows the percentage of trades that senators make that are in the top 25% of all senator trades, along with their total number of trades made. Initially we were going to remove senators from the visualization step with only one trade, as they are too low to be considered statistically significant. These entries remained however because we believe that insider trading can occur only a few times when the information is available, thus potentially being all of a senators trades.


![](transactionweekmonth.png)
Some names that stand out to us are Kelly Loeffler with 60% of her trades being top quartile, Angus S King, Jr. with 52.6% of his trades being top quartile, as well as William Cassidy and Thomas R Carper who all appear multiple times in the list. This is far from convincing though, as perhaps these traders are especially skilled or are making riskier trades overall. It could be that these same senators also have many trades in the bottom quartile of percent returns.


## Unsupervised Machine Learning Techniques
With a dataset like this, it is more difficult to use supervised machine learning techniques, as there is no clear response variable. In this section, we will use several unsupervised machine learning methods in an attempt to notice patterns through clustering. The three methods we use are hierarchical clustering (including dendrogram), isolation forest, and correlation matrix and dendrogram heatmap.


Below is the visualization of hierarchical clustering, the dendrogram, using the ward method. (The list of columns used and method for numerical categorization of string columns can be found on our github.) The ward method is one that clusters the data so as to minimize unexplained variance. The vertical distances represent the distance, or variance, between clusters and in order to capture the strongest divide, we will set a threshold in a way that cuts the tallest vertical line. Marking it near the bottom of the left blue line, this will give us 3 clusters.

![fasdf](dendrogram.png)


Hopefully these three clusters will help visualize different groups in the data. Below is a plot with the labeled clusters of 1 week percentage gain vs 1 month percentage gain. Not very helpful is it though, perhaps besides showing the variance of the two.
![](cluster.png)


# Credit
- [senatestockwatcher.com](https://senatestockwatcher.com)
- [Quantmod](http://www.quantmod.com)
- NYSE.com and NASDAQ.com
- Practical Statistics for Data Scientists by Peter Bruce & Andrew Bruce (physical book)
- Code found on stackoverflow and Towards Data Science (Sources can be found in the Jupyter Notebooks where they were used)
- Professor Lucian Leahu for his advice during the project


# License
See the [LICENSE](https://github.com/pkm29/big_data_final_project3/blob/master/LICENSE.md) file for license rights and limitations (MIT).
