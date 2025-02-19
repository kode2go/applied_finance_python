Markowitz portfolios
00:00 - 00:11
Now that you've nailed down the fundamentals of correlation and co-variance, it's time to see how we can use them to build superior portfolios.

2. 100,000 randomly generated portfolios
00:11 - 00:31
Imagine you have 9 assets, and have randomly generated 100 thousand different portfolio examples, each with different weights, and different levels of risk and return. Which one is best?

3. Sharpe ratio
00:31 - 01:00
In order to compare portfolio performance, you'll want to take into account risk-adjusted returns. The Sharpe ratio is a useful metric which is quoted quite frequently in finance for this purpose. The Sharpe ratio measures how much return an investor can be expect for each incremental unit of risk, and thus can be used to compare portfolios with different levels of risk. This formula assumes a risk-free rate of return, but for this chapter we can assume that this value is 0.

4. The efficient frontier
01:00 - 01:41
According to modern portfolio theory, pioneered by Harry Markowitz in 1952, there is an efficient frontier of portfolios, each with the highest expected return for a given level of risk. This is essentially the portfolios on the top left edge when plotted with risk on the x-axis and expected return on the y-axis. You can select the tangency portfolio, which is the portfolio with the highest Sharpe ratio, and crosses the capital allocation line according to the CAPM theory. We'll talk more about CAPM in the next chapter. The tangency portfolio is known as the max sharpe ratio, or MSR portfolio.

5. The Markowitz portfolios
01:41 - 02:23
The GMV, or global minimum volatility portfolio, is the portfolio on the far laft edge of the plot with the lowest volatility. Both the MSR and GMV are known as Markowitz portfolios. The closer you are to the efficient frontier, the green line, the better the portfolio. Now, you can't see it on this chart, but where the capital allocation line hits 0 on the x-axis, that would be the risk free rate.

6. Choosing a portfolio
02:23 - 02:42
Note that any portfolio below the efficient frontier is technically sub-par when compared to other possible portfolios. Choosing a point on the efficient frontier is a matter of personal preference. High return is available at the cost of higher risk.

7. Selecting the MSR in Python
02:42 - 03:44
You will now be given a list of randomly generated portfolios with their risk and return figures. In order to select the MSR, or max Sharpe ratio portfolio, you will first need to calculate the Sharpe ratio for each portfolio in the dataset. You can do this by first subtracting the risk free rate from the returns, and then dividing by the volatility column. In this case, we assume that the risk free rate is 0, so it doesn't matter, but we'll keep it in the formula just in case interest rates shoot up in the future and we need to incorporate them into our analysis. Next, sort the DataFrame by Sharpe ratios using the dot-sort_values() method with ascending = False to get the top Sharpe ratio portfolios. Finally, you can use the dot-iloc accessor to only select the first 5 weight columns if you only have a portfolio with 5 assets.

8. Past performance is not a guarantee of future returns
03:44 - 03:54
Now keep in mind that historical Sharpe ratios aren't an indication of future performance. In fact, Sharpe ratios change quite dramatically over time.

9. Selecting the GMV in Python
03:54 - 04:19
You can find the GMV, or global minimum volatility portfolio by sorting by the Volatility column instead, but this time with ascending equals True to select the portfolio with the lowest volatility. The global minimum volatility portfolio actually tends to do pretty well over time, whereas the MSR portfolio tends to be rather inconsistent.

10. Let's practice!
04:19 - 04:28
Now it's your turn to build some portfolios!

![image](https://github.com/user-attachments/assets/1ff278e9-65b1-4326-a775-b6581f50c8f1)

![image](https://github.com/user-attachments/assets/342f9912-3c9e-4809-8dc7-c436254870f4)

# Sharpe ratios
The Sharpe ratio is a simple metric of risk adjusted return which was pioneered by William F. Sharpe. Sharpe ratio is useful to determine how much risk is being taken to achieve a certain level of return. In finance, you are always seeking ways to improve your Sharpe ratio, and the measure is very commonly quoted and used to compare investment strategies.

The original 1966 Sharpe ratio calculation is quite simple:

 ![image](https://github.com/user-attachments/assets/11d3f694-88c3-4ca3-bb7b-2eba6c5c851f)


S: Sharpe Ratio
: Asset return
: Risk-free rate of return
: Asset volatility
The randomly generated portfolio is available as RandomPortfolios.

Instructions
100 XP
Assume a risk_free rate of 0 for this exercise.
Calculate the Sharpe ratio for each asset by subtracting the risk free rate from returns and then dividing by volatility.

```
# Risk free rate
risk_free = 0

# Calculate the Sharpe Ratio for each asset
RandomPortfolios['Sharpe'] = (RandomPortfolios['Returns'] - risk_free) / RandomPortfolios['Volatility']

# Print the range of Sharpe ratios
print(RandomPortfolios['Sharpe'].describe()[['min', 'max']])
```

# The MSR portfolio
The maximum Sharpe ratio, or MSR portfolio, which lies at the apex of the efficient frontier, can be constructed by looking for the portfolio with the highest Sharpe ratio.

Unfortunately, the MSR portfolio is often quite erratic. Even though the portfolio had a high historical Sharpe ratio, it doesn't guarantee that the portfolio will have a good Sharpe ratio moving forward.

Instructions
100 XP
Sort RandomPortfolios with the highest Sharpe value, ranking in descending order.
Multiply MSR_weights_array across the rows of StockReturns to get weighted stock returns.
Finally, review the plot of cumulative returns over time.

```
# Sort the portfolios by Sharpe ratio
sorted_portfolios = RandomPortfolios.sort_values(by=['Sharpe'], ascending=False)

# Extract the corresponding weights
MSR_weights = sorted_portfolios.iloc[0, 0:numstocks]

# Cast the MSR weights as a numpy array
MSR_weights_array = np.array(MSR_weights)

# Calculate the MSR portfolio returns
StockReturns['Portfolio_MSR'] = StockReturns.iloc[:, 0:numstocks].mul(MSR_weights_array, axis=1).sum(axis=1)

# Plot the cumulative returns
cumulative_returns_plot(['Portfolio_EW', 'Portfolio_MCap', 'Portfolio_MSR'])
```

![image](https://github.com/user-attachments/assets/cba56680-132b-44c8-b269-22837ea8965a)

# The GMV portfolio
The global minimum volatility portfolio, or GMV portfolio, is the portfolio with the lowest standard deviation (risk) and the highest return for the given risk level.

Returns are very hard to predict, but volatilities and correlations tend to be more stable over time. This means that the GMV portfolio often outperforms the MSR portfolios out of sample even though the MSR would outperform quite significantly in-sample. Of course, out of sample results are what really matters in finance.

Instructions
0 XP
Sort RandomPortfolios with the lowest volatility value, ranking in ascending order.
Multiply GMV_weights_array across the rows of StockReturns to get weighted stock returns.
Finally, review the plot of cumulative returns over time.

```
# Sort the portfolios by volatility
sorted_portfolios = RandomPortfolios.sort_values(by=['Volatility'], ascending=True)

# Extract the corresponding weights
GMV_weights = sorted_portfolios.iloc[0,0:numstocks]

# Cast the GMV weights as a numpy array
GMV_weights_array = np.array(GMV_weights)

# Calculate the GMV portfolio returns
StockReturns['Portfolio_GMV'] = StockReturns.iloc[:, 0:numstocks].mul(GMV_weights_array, axis=1).sum(axis=1)

# Plot the cumulative returns
cumulative_returns_plot(['Portfolio_EW', 'Portfolio_MCap', 'Portfolio_MSR', 'Portfolio_GMV'])
```

![image](https://github.com/user-attachments/assets/c858642b-1d3c-422d-b3fa-c2876eb2968a)

