1. Financial returns
00:00 - 00:11
Hello, and welcome to this course on Portfolio Risk Management in Python! I'm Dakota Wixom, and I'll be your instructor for this course.

2. Course overview
00:11 - 00:35
In this course, you'll learn how to analyze return distributions, build portfolios, identify factors driving portfolio returns, and even estimate potential losses. But before we get into portfolio investing, factor analysis, or risk management, it is essential to understand the fundamental properties of financial returns, which is what this chapter is all about.

3. Investment risk
00:35 - 00:59
To kick things off, let's first define the term "risk" itself. Financial risk can simply be thought of as a measure of the uncertainty of future returns. When analyzing returns, we can say that risk is essentially the dispersion or variance of those returns.

4. Financial risk
00:59 - 01:09
What I'm getting at is that financial risk has many different definitions, but they all hinge on two fundamental concepts: Returns and Probability.

5. A tale of two returns
01:09 - 01:48
Before we get ahead of ourselves, let's do a quick refresher on financial returns. Financial returns are generally derived from stock prices, and are expressed as percentages in decimal form. There are two common types of financial returns: discrete returns (also called simple returns), and log returns (also called continuous returns). For this course, you will be using mostly discrete returns at the daily frequency. Log returns are used for some advanced formulas and financial models in finance.

6. Calculating stock returns
01:48 - 02:04
There is a very simple formula to calculate simple returns, which is simply today's price minus yesterday's price, divided by yesterday's price. This gives you a percentage gain or loss for the day.

7. Calculating log returns
02:04 - 02:31
Log returns are actually pretty easy to calculate, too, but we can ignore them for the sake of this course. What is important to understand though is that log returns aggregate across time, while discrete returns aggregate across assets. Since you are building portfolios with multiple assets, discrete returns make the most sense to work with for this course.

8. Calculating stock returns in Python
02:31 - 03:02
In order to calculate returns, you're first going to need stock price data to work with. We will provide you with CSV files containing stock prices, and it's your job to read in that data and process it into a DataFrame. To accomplish all of this, you'll need to work with the Python module Pandas, which will come in handy for this course. Make sure to parse the dates, sort the values by date, and then set the index to the 'Date' column.

9. Calculating stock Returns in Python
03:02 - 03:31
Now that you've wrangled the data, calculating daily returns is as easy as calling the dot pct_change() method on the proper column. If you call the dot head() method on your returns, you'll notice that the very first period has an invalid return entry. This is because in the first period, there was no previous price to derive the return from.

10. Visualizing return distributions
03:31 - 04:14
In order to visualize the distribution of your daily returns, you're going to need to first import the matplotlib dot pyplot module. Remember how the first period had no return? You will need to call the dot dropna() method on your returns before passing them into the hist() function. Setting bins equal to a large number will spread out your plot, but a low number will cause a lack of resolution. If you set density equal to False, you will plot the frequency of returns rather than the probability density on the y-axis. Finally, call plt dot show() to display the plot.

11. Let's practice!
04:14 - 04:22
Got everything? Great, now let's try some examples.

# Financial timeseries data

Financial timeseries data
In finance, it is common to be working with a CSV (comma-separated-values) "flat" file of a timeseries of many different assets with their prices, returns, or other data over time. Sometimes the data is stored in databases, but more often than not, even large banks still use spreadsheets.

In this exercise, you have been given a timeseries of trading data for Microsoft stock as a .csv file stored at the url fpath_csv. When you finish the exercise, take note of the various types of data stored in each column.

You will be using pandas to read in the CSV data as a DataFrame.

```
# Import pandas as pd
import pandas as pd

# Read in the csv file and parse dates
StockPrices = pd.read_csv(fpath_csv, parse_dates=['Date'])

# Ensure the prices are sorted by Date
StockPrices = StockPrices.sort_values(by='Date')

# Print only the first five rows of StockPrices
print(StockPrices.head())
```

Output:

```
<script.py> output:
            Date    Open    High     Low   Close    Volume  Adjusted
    0 2000-01-03  88.777  89.722  84.712  58.281  53228400    38.528
    1 2000-01-04  85.893  88.588  84.901  56.312  54119000    37.226
    2 2000-01-05  84.050  88.021  82.726  56.906  64059600    37.619
    3 2000-01-06  84.853  86.130  81.970  55.000  54976600    36.359
    4 2000-01-07  82.159  84.901  81.166  55.719  62013600    36.834
```

# Calculating financial returns

The file you loaded in the previous exercise included daily Open, High, Low, Close, Adjusted Close, and Volume data, often referred to as OHLCV data.

The Adjusted Close column is the most important. It is normalized for stock splits, dividends, and other corporate actions, and is a true reflection of the return of the stock over time. You will be using the adjusted close price to calculate the returns of the stock in this exercise.

StockPrices from the previous exercise is available in your workspace, and matplotlib.pyplot is imported as plt.

```
# Calculate the daily returns of the adjusted close price
StockPrices['Returns'] = StockPrices['Adjusted'].pct_change()

# Check the first five rows of StockPrices
print(StockPrices.head())

# Plot the returns column over time
StockPrices['Returns'].plot()
plt.show()
```

![image](https://github.com/user-attachments/assets/7283e5b2-63cc-4aab-93d4-19caa5df85ec)

# Return distributions
In order to analyze the probability of outliers in returns, it is helpful to visualize the historical returns of a stock using a histogram.

You can use the histogram to show the historical density or frequency of a given range of returns. Note the outliers on the left tail of the return distribution are what you often want to avoid, as they represent large negative daily returns. Outliers on the right hand side of the distribution are normally particularly good events for the stock such as a positive earnings surprise.

StockPrices from the previous exercise is available in your workspace, and matplotlib.pyplot is imported as plt.

```
# Convert the decimal returns into percentage returns
percent_return = StockPrices['Returns']*100

# Drop the missing values
returns_plot = percent_return.dropna()

# Plot the returns histogram
plt.hist(returns_plot, bins=75)
plt.show()
```

![image](https://github.com/user-attachments/assets/f5c1b021-61bf-4e29-ac1d-a4ba206cd5b6)

