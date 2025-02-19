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
