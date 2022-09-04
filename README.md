# Portfolio Analysis Project
### Vikram Achuthan									
### August 2022

## Purpose
All investors analyze the performance of their investments over time to understand how their investments are performing.  Portfolio analysis is an important function that every serious investor does in order to track their performance, help make decisions to allocate resources or understand their tax implications. A portfolio may consist of many financial instruments like stocks, options, real estate, bonds, cds, cash and other combinations of these. A typical set of questions that portfolio analysis tool helps answer are as follows:

1. What are the total gains or losses; how does its performance compare with the performance of a standard index like the S&P index. This comparison is important because many renowned stock advisors recommend that small investors stop picking individual stocks and invest in the broad stock market instead. For instance, if an investor has $1000 to invest, they can choose to invest in an S&P indexed ETF and derive the losses or gains of the stock market in general. The alternative is for the investor to study the market and pick individual stocks in the hope that they can beat the S&P index over the same period of time.
2. What is the best / worst performing financial instrument in the portfolio. This helps the investor in deciding whether to continue investing in a particular stock or to divest of that position and buy other stocks so that they can get better returns.

It is true that many investment trading platforms and brokerages provide these tools and a small investor who has an account with a single broker does not have to rely on a third party to analyze their portfolio. However, any investor who has been investing over a period of time would have opened several accounts. In this case, the investor has to consolidate the records of their financial transactions in one place and rely on a third party tool to perform such an analysis. In my opinion there is no such tool that helps with this challenge that is cheap, user friendly and rich with sophisticated analysis. There is a great need for such a tool that is available to the small retail investor community.

## Requirements


### Overall portfolio

This section will inform the investor about their portfolio as a whole and will have the following details:

| Start Date| End Date | Total Rate of Return (%)|Index Rate of Return (%)|
| ----------- | ----------- | ----------- | ----------- |
| 3/7/2005| 8/15/2022 | 10| 9|


Where:

**Start Date:** This is the start date for the duration for calculating the returns for the portfolio. By default it is the earliest date for actions in the Input table; the design should allow for the investor to enter a custom start date for the performance analysis to be done. The custom date can be only after the earliest transaction date in the Input table

**End Date:** This is the end date for the duration for calculating the returns for the portfolio. By default it will be today’s date. The design should allow for the investor to enter a custom end date that is earlier than today’s date. 

**Total Rate of Return (%):** This is the total loss or gain in percent for the investor in the duration requested by the investor. This is determined by the following formula:

(Total Value of Investments - Total Cost of Investments) / Total Cost of Investments 

Where:

Total Value of Investments = Sum of the total value of stocks remaining at the price on the End Date + Sum of the total value of the sold transactions + Sum of the total dividends earned by the security - one time commission for each transaction

Total Cost of Investments = Sum of the total cost incurred for purchase of the stock








## Code Explanation

### Block 1:

Block 1 reads data from the Google Sheets spreadsheet, where each row represents a ‘Buy’ or ‘Sell’ action for stocks in the user’s portfolio. The columns in the spreadsheet must be in the following order: 

['Type', 'Action', 'Action Date', 'Ticker','Quantity','Cost','Commission']
 
After reading the data, it is sorted into a Pandas DataFrame. 
yFinance is installed and used to download market data from Yahoo! Finance.
 
### Block 2:
total_cost()
Block 2 processes each row that has a sale action by updating unsold quantities in previous rows with buy actions and calculates gains from the sale, according to the FIFO sell method. 

### Block 3:
calculate_cumulative()
This function is called on each row in complete_stock_df to update the cumulative cost and cumulative quantity. This special function was required in order to handle splits, instead of just relying on the built-in cumsum() function. 

### Block 4:
Store dividend and split data for each stock in chosen_stocks in a dictionary. 
 
### Block 5:
User inputs stocks they want analyzed. To analyze the portfolio, input ‘whole!’
 
### Block 6:
Runs analysis on portfolio (driver)
Refer to comments for procedure
 
### Block 7:
Reports final analysis of input, compares rate of return percentage to an S&P Index Fund

Figure 3: Portfolio Analysis Output Statement
 
The rate of return was calculated in the following manner:
Rate of Return = 100 %  Total Value - Outstanding CostTotal Costs, where 
 
Total Value = Cumulative Gains (including dividends) + Value of Outstanding Shares Today (using latest closing price) 
 
Outstanding Cost = Cost of Outstanding Shares Today (not accounted in Cumulative Gains since they were not sold)
 
Total Cost = Cumulative Cost
 
 
### complete_stock_df column description:

Figure 4: Screenshot of complete_stock_df for AAPL

**1. Type:** The kind of investment (eg Stocks, Bonds, Mutual Funds)

**2. Action:** The action that the row represents (eg Sell, Buy, Dividend, Split)

**3. Action Date:** The date on which the action was taken

**4. Ticker:** Ticker of the investment as listed on the stock market

**5. Quantity:** Amount of shares being bought (positive) or sold (negative)

**6. Cost:** The cost at which the shares were bought or the price at which they are being sold

**7. Commission:** The amount paid to the broker for the transaction

**8. Additional Quantity:** The amount of shares being purchased - only applicable to rows

that represent buys (Action = Buy)

**9. Split:** Represents the split multiple. Default is 1 if action is not equal to ‘Split’

**10. Unsold Quantity:** For rows that represent buys, the unsold quantity represents the

amount of shares in that batch that have still not been sold.

**11. Transaction Val:** The amount of shares being purchased multiplied by the cost- only

applicable to rows that represent buys (Action = Buy)

**12. Cumulative Quant:** Cumulative sum of total shares - accounts for sold shares and

splits.

**13. Cumulative Buys:** Cumulative sum of total shares bought - accounts for splits *but not*

*sold shares*.

**14. Cumulative Cost**: Cumulative sum of amount spent to purchase shares.

**15. Gains:** Represents the profit made from a sale, or the income from a dividend.

**16. Cumulative Gains**: Cumulative sum of the gains

**17. Cost Per Share:** Cumulative Cost divided by Cumulative Buys
