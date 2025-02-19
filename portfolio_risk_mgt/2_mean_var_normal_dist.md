Mean, variance, and normal distribution
00:00 - 00:17
Since financial risk can simply be thought of as a measure of uncertainty of future returns, it is good practice to examine the natural distribution of returns in order to better understand the risk of investments.

2. Moments of distributions
00:17 - 00:59
Probability distributions have common properties, known as moments, which can be analyzed and compared to other distributions. The first moment is the mean, or mu, which is essentially the average outcome of a random sample of the distribution. The second moment is the variance, which is simply the standard deviation, or sigma, squared. This is a measure of the variability in outcomes. The third and fourth moments are less commonly used. The third moment, skewness, is a measure of the "tilt" of a distribution, while the fourth moment, kurtosis, is an important measure of the thickness of the tails of a distribution. We'll get to those later.

3. The Normal Distribution
00:59 - 01:10
There are many different types of common distributions. You might be familiar with the Gaussian normal distribution if you've taken a statistics class before.

4. The standard normal distribution
01:10 - 01:19
The standard normal distribution is a special case of the normal distribution when sigma = 1 and mu = 0.

5. Comparing against a normal distribution
01:19 - 01:47
All normal distributions tend to have a skewness near 0 and a kurtosis near 3. Financial returns, on the other hand, tend to have positive skewness and a kurtosis higher than 3. This means financial returns tend to have a higher probability of both outliers and of positive returns than a normal distribution. We'll talk more about kurtosis and skewness later.

6. Comparing against a normal distribution
01:47 - 02:09
Here we compare the daily returns of IBM stock with a sample from the normal distribution with the same mean and standard deviation. There are statistical tests for normality(such as Shapiro-Wilk, Jarque-Bera, but a high kurtosis or large skewness is a simple indicator of non-normal returns.

7. Calculating mean returns in python
02:09 - 02:53
You can use Python to estimate the moments of a return distribution. To calculate the average daily return of an asset, or mu, assuming you have already calculated daily returns, simply use NumPy's mean function to compute the average. Now if you want to annualize that number, you'll first need to add 1 to the decimal before raising the quantity to the power of 252, which is the typical number of trading days in a year. For example, a daily return of just 0-point-03% works out to an annualized return of 7-point-85% when it is compounded every day for 252 trading days in a row.

8. Standard deviation and variance
02:53 - 03:24
The second moment of the distribution, the variance, which is the square of the standard deviation (or volatility), Volatility is one of the most important concepts for risk management in finance. Some traders simply refer to it as "vol" for short. The fundamental takeaway is simple: an investment with higher volatility is viewed as a higher risk investment. Volatility is just another measure of the dispersion of returns, just like variance.

9. Standard deviation and variance in Python
03:24 - 03:42
You can easily calculate the standard deviation of returns using the numpy std() function. If the returns are daily returns, then this function will output the daily standard deviation. In order to calculate the variance, simply square the standard deviation.

10. Scaling volatility
03:42 - 03:58
It's very important to understand that volatility scales with the square root of time. To properly scale the daily volatility of an asset, simply multiply the volatility by the square root of the number of trading days in a year.

11. Scaling volatility in Python
03:58 - 04:06
This is easy to accomplish using NumPy by simply multiplying the standard deviation of daily returns by the square root of 252.

12. Let's practice!
04:06 - 04:12
Time to put this into practice.

# Start

First moment: Mu
You can calculate the average historical return of a stock by using numpy's mean() function.

When you are calculating the average daily return of a stock, you are essentially estimating the first moment ( 
 ) of the historical returns distribution.

But what use are daily return estimates to a long-term investor? You can use the formula below to estimate the average annual return of a stock given the average daily return and the number of trading days in a year (typically there are roughly 252 trading days in a year):

![image](https://github.com/user-attachments/assets/43246ab7-a462-4e69-b06c-0a2c233a50c8)

The StockPrices object from the previous exercise is stored as a variable.

Import numpy as np.
Calculate the mean of the 'Returns' column to estimate the first moment ( 
 ) and set it equal to mean_return_daily.
Use the formula to derive the average annualized return assuming 252 trading days per year. Remember that exponents in Python are calculated using the ** operator.

```
# Import numpy as np
import numpy as np

# Calculate the average daily return of the stock
mean_return_daily = np.mean(StockPrices['Returns'])
print(mean_return_daily)

# Calculate the implied annualized average return
mean_return_annualized = ((1+mean_return_daily)**252)-1
print(mean_return_annualized)
```

output:

```
<script.py> output:
    0.00037777546435757676
    0.09985839482852632
```

# Second moment: Variance

Just like you estimated the first moment of the returns distribution in the last exercise, you can can also estimate the second moment, or variance of a return distribution using numpy.

In this case, you will first need to calculate the daily standard deviation ( 
 ), or volatility of the returns using np.std(). The variance is simply 
.

StockPrices from the previous exercise is available in your workspace, and numpy is imported as np.

Calculate the daily standard deviation of the 'Returns' column and set it equal to sigma_daily.
Derive the daily variance (second moment, 
) by squaring the standard deviation.

```
# Calculate the standard deviation of daily return of the stock
sigma_daily = np.std(StockPrices['Returns'])
print(sigma_daily)

# Calculate the daily variance
variance_daily = sigma_daily**2
print(variance_daily)
```

output:
```
<script.py> output:
    0.019341100408708317
    0.00037407816501973704
```

# Annualizing variance

Annualizing variance
You can't annualize the variance in the same way that you annualized the mean.

In this case, you will need to multiply 
 by the square root of the number of trading days in a year. There are typically 252 trading days in a calendar year. Let's assume this is the case for this exercise.

This will get you the annualized volatility, but to get annualized variance, you'll need to square the annualized volatility just like you did for the daily calculation.

sigma_daily from the previous exercise is available in your workspace, and numpy is imported as np.

Annualize sigma_daily by multiplying by the square root of 252 (the number of trading days in a years).
Once again, square sigma_annualized to derive the annualized variance.

```
# Annualize the standard deviation
sigma_annualized = sigma_daily*np.sqrt(252)
print(sigma_annualized)

# Calculate the annualized variance
variance_annualized = sigma_annualized**2
print(variance_annualized)
```

output:
```
<script.py> output:
    0.3070304505826315
    0.09426769758497373
```
