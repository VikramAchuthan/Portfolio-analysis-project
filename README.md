# Portfolio-analysis-project
Portfolio Analysis Project
Vikram Achuthan									August 2022
Purpose
All investors analyze the performance of their investments over time to understand how their investments are performing.  Portfolio analysis is an important function that every serious investor does in order to track their performance, help make decisions to allocate resources or understand their tax implications. A portfolio may consist of many financial instruments like stocks, options, real estate, bonds, cds, cash and other combinations of these. A typical set of questions that portfolio analysis tool helps answer are as follows:
What are the total gains or losses; how does its performance compare with the performance of a standard index like the S&P index. This comparison is important because many renowned stock advisors recommend that small investors stop picking individual stocks and invest in the broad stock market instead. For instance, if an investor has $1000 to invest, they can choose to invest in an S&P indexed ETF and derive the losses or gains of the stock market in general. The alternative is for the investor to study the market and pick individual stocks in the hope that they can beat the S&P index over the same period of time.
What is the best / worst performing financial instrument in the portfolio. This helps the investor in deciding whether to continue investing in a particular stock or to divest of that position and buy other stocks so that they can get better returns.
Performance of the portfolio or financial instrument over time. This analysis helps an investor analyze the decisions in a calendar year in order to understand the circumstances under which these investments were made and learn on improving their stock models.

It is true that many investment trading platforms and brokerages provide these tools and a small investor who has an account with a single broker does not have to rely on a third party to analyze their portfolio. However, any investor who has been investing over a period of time would have opened several accounts. In this case, the investor has to consolidate the records of their financial transactions in one place and rely on a third party tool to perform such an analysis. In my opinion there is no such tool that helps with this challenge that is cheap, user friendly and rich with sophisticated analysis. There is a great need for such a tool that is available to the small retail investor community.

Requirements
The analysis of portfolios can be divided into three broad categories. In each of these categories the investor is interested in two metrics. One is the return on investment for the chosen instrument and the second is a comparison with a standard index. 

Overall portfolio: The investor is interested in tracking the performance of their entire portfolio. 
Individual stocks: The investor is interested in tracking the performance of individual stocks	and ranking the winners and losers in the portfolio.
Over time: Performance of the portfolio grouped by calendar years
To understand the performance of the portfolio for specific years
To understand the performance of individual stocks for specific years


The following sections will go into the detailed requirements for the features listed above. For all the features listed below the input will be the same data set that will be provided in a Google spreadsheet as follows:


Type
Action
Action Date
Ticker
Quantity
Unit Cost
Commission
Stock
Buy
3/7/2005
AAPL
40
10.00
7.00
Stock
Buy
3/9/2005
AAPL
30
20.00
7.00
Stock
Div
3/31/2005
AAPL
70
0.50
0.00
Stock
Buy
6/10/2005
WMT
100
50.00
7.00
Stock
Buy
7/1/2005
AAPL
10
50.00
7.00
Stock
Div
7/15/2005
WMT
100
0.25
0.00
Stock
Split
7/20/2005
WMT
100
60.00
7.00
Stock
Div
8/1/2005
AAPL
80
0.50
0.00
Stock
Div
8/23/2005
WMT
100
0.25
0.00
Stock
Sell
9/1/2005
AAPL
50
30.00
7.00
CD
Buy
9/1/2007
fid-123-cd
100
1.00
0.00
CD
Interest
10/5/2007
fid-123-cd
100
0.02
0.00
CD
Sell
12/4/2007
fid-123-cd
100
1.00
0.00
Option
Buy
12/5/2007
pins-2008-call
1
10.00
12.00
Option
Sell
1/5/2008
pins-2008-call
1
12.00
12.00

Where
Type: this is the type of financial instrument that is recorded by the investor and will be one of Stock -- which is traded in any of the US based stock exchanges. Stocks traded in foreign stock exchanges will not be analyzed in this exercise
Action: this is the action taken for the investment. For a Stock it would be one of Buy, Sell, Div (for dividends) or Split. In the case of the split the quantity will reflect the ratio of the split and no additional calculations are required.
Action Date: This is the date on which the Action was taken and performance metrics will be based on this date
Ticker: the symbol of the financial instrument to be reported on
Quantity: This is the amount of the security that was impacted by the action on the Ticker
Unit Cost: The per unit cost of the Ticker for that transaction. Inflow or outflow of cash depends on the action taken on the Ticker. 
Commission: This is the total amount paid for the transaction

Overall portfolio
This section will inform the investor about their portfolio as a whole and will have the following details


Start Date
End Date
Total Rate of Returnh (%)
Index Rate of Return (%)
3/7/2005
8/15/2022
10
9


Where:
Start Date: This is the start date for the duration for calculating the returns for the portfolio. By default it is the earliest date for actions in the Input table; the design should allow for the investor to enter a custom start date for the performance analysis to be done. The custom date can be only after the earliest transaction date in the Input table
End Date: This is the end date for the duration for calculating the returns for the portfolio. By default it will be today’s date. The design should allow for the investor to enter a custom end date that is earlier than today’s date. 
Total Rate of Return (%): This is the total loss or gain in percent for the investor in the duration requested by the investor. This is determined by the following formula:

Total Value of Investments - Total Cost of Investments  Total Cost of Investments 

Where:
Total Value of Investments = Sum of the total value of stocks remaining at the price on the End Date + Sum of the total value of the sold transactions + Sum of the total dividends earned by the security - one time commission for each transaction
Total Cost of Investments = Sum of the total cost incurred for purchase of the stock		
Index Rate of Return (%):  This is the total change in percentage of the value of S&P for the same period for which the investor’s Total Return was calculated as determined by the following formula:

	Starting Value of Index - Ending value of IndexStarting Value of Index

Where :
	Starting Value of Index is the value of S&P index on the start date requested by user
	Ending Value of Index is the value of S&P index on the end date requested by user












Code Explanation

Block 1:

Block 1 reads data from the Google Sheets spreadsheet, where each row represents a ‘Buy’ or ‘Sell’ action for stocks in the user’s portfolio. The columns in the spreadsheet must be in the following order: 

['Type', 'Action', 'Action Date', 'Ticker','Quantity','Cost','Commission']
 
After reading the data, it is sorted into a Pandas DataFrame. 
yFinance is installed and used to download market data from Yahoo! Finance.
 
Block 2:
total_cost()
Block 2 processes each row that has a sale action by updating unsold quantities in previous rows with buy actions and calculates gains from the sale, according to the FIFO sell method. 

Block 3:
calculate_cumulative()
This function is called on each row in complete_stock_df to update the cumulative cost and cumulative quantity. This special function was required in order to handle splits, instead of just relying on the built-in cumsum() function. 
[1]: Yahoo! Finance’s historical dividend data accounts for splits by retroactively multiplying the dividend by the split multiple. 

Figure 1: Apple Dividend History from investor.apple.com

Figure 2: Apple Dividend History from Yahoo! Finance
For example, the two tables show dividend history for AAPL from May, 2020 to February, 2021. The data from Apple’s website shows that after the 4-for-1 stock split on July 30, 2020, the next dividend amount ($.205) was 4 times less than the previous dividend amount ($.82). On the other hand, the data provided by Yahoo! Finance is split adjusted to show a $.205 dividend for each dividend in that period. For our analysis, using split adjusted data to calculate dividends would not accurately reflect total gains. This line finds previous dividend amounts and multiples the cost by the split multiple each time each time a split occurs. 

Block 4:
Store dividend and split data for each stock in chosen_stocks in a dictionary. 
 
Block 5:
User inputs stocks they want analyzed. To analyze the portfolio, input ‘whole!’
 
Block 6:
Runs analysis on portfolio (driver)
Refer to comments for procedure
 
Block 7:
Reports final analysis of input, compares rate of return percentage to an S&P Index Fund

Figure 3: Portfolio Analysis Output Statement
 
The rate of return was calculated in the following manner:
Rate of Return = 100 %  Total Value - Outstanding CostTotal Costs, where 
 
Total Value = Cumulative Gains (including dividends) + Value of Outstanding Shares Today (using latest closing price) 
 
Outstanding Cost = Cost of Outstanding Shares Today (not accounted in Cumulative Gains since they were not sold)
 
Total Cost = Cumulative Cost
 
 
complete_stock_df column description:

Figure 4: Screenshot of complete_stock_df for AAPL

Type: The kind of investment (eg Stocks, Bonds, Mutual Funds)
Action: The action that the row represents (eg Sell, Buy, Dividend, Split)
Action Date: The date on which the action was taken
Ticker: Ticker of the investment as listed on the stock market
Quantity: Amount of shares being bought (positive) or sold (negative)
Cost: The cost at which the shares were bought or the price at which they are being sold
Commission: The amount paid to the broker for the transaction
Additional Quantity: The amount of shares being purchased - only applicable to rows that represent buys (Action = Buy)
Split: Represents the split multiple. Default is 1 if action is not equal to ‘Split’
Unsold Quantity: For rows that represent buys, the unsold quantity represents the amount of shares in that batch that have still not been sold. 
Transaction Val: The amount of shares being purchased multiplied by the cost- only applicable to rows that represent buys (Action = Buy)
Cumulative Quant: Cumulative sum of total shares - accounts for sold shares and splits. 
Cumulative Buys: Cumulative sum of total shares bought - accounts for splits but not sold shares.
Cumulative Cost: Cumulative sum of amount spent to purchase shares. 
Gains: Represents the profit made from a sale, or the income from a dividend. 
Cumulative Gains: Cumulative sum of the gains
Cost Per Share: Cumulative Cost divided by Cumulative Buys

