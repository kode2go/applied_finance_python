Portfolio composition and backtesting
00:00 - 00:11
Working with individual stocks is relatively simple - but things get a lot more exciting once you begin building your first portfolio.

2. Calculating portfolio returns
00:11 - 00:29
You can think of portfolios as a bundle of individual stocks with different weights for each position. The return of a portfolio is a linear combination of the weights and returns of each position. This is assuming you're using discrete returns instead of log returns.

3. Calculating portfolio returns in Python
00:29 - 01:15
In Python, this entire operation can be performed in a single line if you have an array of portfolio weights and a DataFrame of stock returns. First, call the dot mul() method on StockReturns, pass in the portfolio weights, and set the axis to 1, for rows. This will multiply each portfolio weight by the corresponding returns column assuming they are in proper order. Then, call the dot sum() method, also with axis 1, to sum all the weighted returns in order to get the portfolio return. You'll be performing this operation a lot in this chapter. You can set the portfolio return equal to a new column in StockReturns if you wish to keep track of it easily.

4. Equally weighted portfolios in Python
01:15 - 02:10
Any good strategy should at least ideally outperform an equally weighted portfolio, which holds the same weight in every stock by default. To calculate an equally weighted portfolio, you first want to set numstocks equal to the number of stocks in your portfolio. Use np dot repeat() to set the portfolio weights, passing in 1 divided by the number of stocks as the first argument, and the number of stocks as the second argument. This creates an equally weighted array of weights with the proper length. You can then use the dot mul() and dot sum() methods as in the previous example. In this case, I use dot iloc to select the columns that contain stock returns.

5. Plotting portfolio returns in Python
02:10 - 02:27
Plotting daily stock returns in Python is easy. Assuming you have already computed returns by running the pct_change method on the data frame of adjusted prices, you can grab the column of returns and call plot. But this plot is noisy, right?

6. Plotting portfolio cumulative returns
02:27 - 02:52
Well that's because cumulative returns, or growth over time, is normally what you want to see. All you need to do is add 1 to the daily stock returns, take the cumulative product with the dot cumprod() method finally subtracting 1. In this example, I plotted two cumulative returns columns at once, using the dot plot method on the columns of interest.

7. Market capitalization
02:52 - 03:17
Market capitalization weighted portfolios are extremely common. The commonly quoted S&P 500 index, for example, is modeled after a market-cap weighted portfolio of the top 500 US stocks. While the S&P 500 isn't actually a real traded portfolio, it is a popular benchmarking strategy and many portfolio strategies and funds are based off of it.

8. Market capitalization
03:17 - 03:30
The market capitalization, or market cap, of a company is the total value of all outstanding publicly traded shares of the company at any given time.

9. Market-cap weighted portfolios
03:30 - 03:45
In order to calculate the market-cap weight of a given stock n, divide the market capitalization of that company by the sum of all the market capitalizations of all the stocks in your portfolio.

10. Market-Cap weights in Python
03:45 - 04:12
To do this in Python, first define a numpy array of the market caps of each company in your portfolio, then divide that array by the sum of the array. You can then use the market cap weights to calculate the market-cap weighted portfolio in the same way that you used the equal weights array to calculate the equally weighted portfolio.

11. Let's practice!
04:12 - 04:20
Now let's try some examples.

# Practice

Calculating portfolio returns
In order to build and backtest a portfolio, you have to be comfortable working with the returns of multiple assets in a single object.

In this exercise, you will be using a pandas DataFrame object, already stored as the variable StockReturns, to hold the returns of multiple assets and to calculate the returns of a model portfolio.

The model portfolio is constructed with pre-defined weights for some of the largest companies in the world just before January 2017:

Company Name	Ticker	Portfolio Weight
Apple	AAPL	12%
Microsoft	MSFT	15%
Exxon Mobil	XOM	8%
Johnson & Johnson	JNJ	5%
JP Morgan	JPM	9%
Amazon	AMZN	10%
General Electric	GE	11%
Facebook	FB	14%
AT&T	T	16%
Note that the portfolio weights should sum to 100% in most cases

Finish defining the numpy array of model portfolio_weights with the values according to the table above.
Use the .mul() method to multiply the portfolio_weights across the rows of StockReturns to get weighted stock returns.
Then use the .sum() method across the rows on the WeightedReturns object to calculate the portfolio returns.
Finally, review the plot of cumulative returns over time.

```
# Finish defining the portfolio weights as a numpy array
portfolio_weights = np.array([0.12, 0.15, 0.08, 0.05, 0.09, 0.10, 0.11, 0.14, 0.16])

# Calculate the weighted stock returns
WeightedReturns = StockReturns.mul(portfolio_weights, axis=1)

# Calculate the portfolio returns
StockReturns['Portfolio'] = WeightedReturns.sum(axis=1)

# Plot the cumulative portfolio returns over time
CumulativeReturns = ((1+StockReturns["Portfolio"]).cumprod()-1)
CumulativeReturns.plot()
plt.show()
```

![image](https://github.com/user-attachments/assets/1f0fa6f3-45bf-483a-8c55-0f89b040983f)

# Equal weighted portfolios

When comparing different portfolios, you often want to consider performance versus a naive equally-weighted portfolio. If the portfolio doesn't outperform a simple equally weighted portfolio, you might want to consider another strategy, or simply opt for the naive approach if all else fails. You can expect equally-weighted portfolios to tend to outperform the market when the largest companies are doing poorly. This is because even tiny companies would have the same weight in your equally-weighted portfolio as Apple or Amazon, for example.

To make it easier for you to visualize the cumulative returns of portfolios, we defined the function cumulative_returns_plot() in your workspace.

Set numstocks equal to 9, which is the number of stocks in your portfolio.
Use np.repeat() to set portfolio_weights_ew equal to an array with an equal weights for each of the 9 stocks.
Use the .iloc accessor to select all rows and the first 9 columns when calculating the portfolio return.
Finally, review the plot of cumulative returns over time.

```
# How many stocks are in your portfolio?
numstocks = 9

# Create an array of equal weights across all assets
portfolio_weights_ew = np.repeat(1/numstocks, numstocks)

# Calculate the equally-weighted portfolio returns
StockReturns['Portfolio_EW'] = StockReturns.iloc[:, 0:numstocks].mul(portfolio_weights_ew, axis=1).sum(axis=1)
cumulative_returns_plot(['Portfolio', 'Portfolio_EW'])
```

# Market-cap weighted portfolios
Conversely, when large companies are doing well, market capitalization, or "market cap" weighted portfolios tend to outperform. This is because the largest weights are being assigned to the largest companies, or the companies with the largest market cap.

Below is a table of the market capitalizations of the companies in your portfolio just before January 2017:

Company Name	Ticker	Market Cap ($ Billions)
Apple	AAPL	601.51
Microsoft	MSFT	469.25
Exxon Mobil	XOM	349.5
Johnson & Johnson	JNJ	310.48
JP Morgan	JPM	299.77
Amazon	AMZN	356.94
General Electric	GE	268.88
Facebook	FB	331.57
AT&T	T	246.09

Instructions
100 XP
Finish defining the market_capitalizations array of market capitalizations in billions according to the table above.
Calculate mcap_weights array such that each element is the ratio of market cap of the company to the total market cap of all companies.
Use the .mul() method on the mcap_weights and returns to calculate the market capitalization weighted portfolio returns.
Finally, review the plot of cumulative returns over time.

```
# Create an array of market capitalizations (in billions)
market_capitalizations = np.array([601.51, 469.25, 349.5, 310.48, 299.77, 356.94, 268.88, 331.57, 246.09])

# Calculate the market cap weights
mcap_weights = market_capitalizations / sum(market_capitalizations)

# Calculate the market cap weighted portfolio returns
StockReturns['Portfolio_MCap'] = StockReturns.iloc[:, 0:9].mul(mcap_weights, axis=1).sum(axis=1)
cumulative_returns_plot(['Portfolio', 'Portfolio_EW', 'Portfolio_MCap'])
```

![image](https://github.com/user-attachments/assets/d0b55bf6-c6f7-496a-be9d-ec8f954e9e32)

