# Identifying Suspicious Trades Among U.S. Senators


## Overview
This README contains a brief overview and background of the project. If you would like to jump straight to the analysis, there are 3 files to read through. They are organized chronologically by number, however most of the interesting analysis is done in Outlier Analysis.ipynb.<br> <br> 1. __Collect Senator Data.ipynb__: Import and quickly analyze senator stock trades. <br><br>   2. __Stocks/Collect Stock Data.ipynb__: Collect different types stock data to be used in gain/loss analysis. Stock pricing data is collected by an R script I (Paul McCabe) made called Stocks/Import-Stocks.Rmd.<br><br> 3. __Outlier Analysis.ipynb__: Analyze senator profits using a combination of graphical representations, basic statistical analysis, and unsupervised machine learning algorithms. 


Alternatively, HTML versions of these jupyter notebooks exist with the same names.


## Motivations


This project by my partner Ankush and I (Paul McCabe) was motivated largely by the news at the time of U.S. senators being investigated for making "suspicious" stock trades (April 2020). Several senators made multi-million dollar sales right before the 2020 stock market plunge, a few directly opposite of the positive sentiment they expressed about the market. We wished to use a data-driven approach to see exactly how unusual these kind of trades were as well as how profiteable they were. <br><br> Since then, some of these senators have been investigated further by the SEC, some noticeable in our analysis (Kelly Loeffler) and others not (Richard Burr). 


## Written Report


![Screen%20Shot%202020-07-08%20at%201.00.33%20PM.png](https://github.com/pkm29/big_data_final_project3/raw/master/picture1.png)


A full report of this project written by Ankur and I is available [here](https://medium.com/@pkm29/identifying-suspicious-trading-among-u-s-senators-91a908432843). (15 minute read) <br> While Ankur's contributions were mostly towards writing the report and generating experimental data for future work, all code that appears in this document was written by me.


## Data Sources


![Artboard%201.png](https://github.com/pkm29/big_data_final_project3/raw/master/Artboard%201.png)


#### Senator Stock Trading Data - senatestockwatcher.com
Senators are required to report their trades either by filing a paper report or an electronic report to efdsearch.senate.gov. Data used in our analysis was compiled by the owner of the website [senatestockwatcher.com](https://senatestockwatcher.com) into a csv file. Inconsistencies and missing information remained an issue, as well as the fact that paper filed reports were not included. Senator Richard Burr does not appear in our analysis for this reason, however recent websites have begun to digitize these reports manually, GovTrades.com being one of them.


#### Stock Price Data - RStudio and the library 'quantmod'
Downloading unlimited free stock pricing data is difficult to come by, however RStudio and the library called quantmod does so, albeit at a different kind of cost. Quantmod pulls from yahoo finance but does so rather slowly and with around a 25% error rate. 


#### Stock Industry Data - NYSE and NASDAQ website
While researching this topic, we found an analysis that compared senator stock trades to the average stock gains of similiar sized companies in the same industry. CSV files with each company's industry and market cap were collected from the NYSE's website and the NASDAQ's webite, then incorporated into our analysis. 


## Credit
- [senatestockwatcher.com](https://senatestockwatcher.com)
- [Quantmod](http://www.quantmod.com)
- NYSE.com and NASDAQ.com
- Practical Statistics for Data Scientists by Peter Bruce & Andrew Bruce (physical book)
- Code found on stackoverflow and Towards Data Science (Sources can be found in the Jupyter Notebooks where they were used)
- Professor Lucian Leahu for his advice during the project


## License
See the [LICENSE](https://github.com/pkm29/big_data_final_project3/blob/master/LICENSE.md) file for license rights and limitations (MIT).
