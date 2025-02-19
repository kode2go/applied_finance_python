Skewness and kurtosis
00:00 - 00:14
Daily volatility and mean give you a good indication of daily risk and return, but skewness, kurtosis, and scaled volatility will help you build a more holistic view of risk.

2. Skewness
00:14 - 00:39
The third moment, skewness, can be thought of as a measure of how much a distribution leans to the left or right. Negative skew is a right-leaning curve, while positive skew is a left leaning curve. In finance, you would tend to want positive skewness, with a higher probability of significantly good returns on the right hand side of the distribution, and a compressed, predictable left-hand distribution of negative returns.

3. Skewness in Python
00:39 - 01:01
To calculate the skewness of a return distribution, simply call the skew() function after importing skew from scipy dot stats. Be sure to drop any NA values using the dropna() method. Skewness above 0 indicates possible non-normality, which once again you can expect to find in financial returns.

4. Kurtosis
01:01 - 01:51
Kurtosis is a measure of the thickness of the tails of a distribution, which can be used a proxy for the probability of outliers. If you recall, normal distributions tend to have a kurtosis near 3. Most financial returns are leptokurtic, which simply means that they tend to have positive excess kurtosis, or kurtosis greater than 3. Since kurtosis is so often compared to a normal distribution, many functions in Python will automatically return excess kurtosis, which is essentially the sample kurtosis minus 3, which helps demonstrate whether the probability of outliers is higher or lower than a normal distribution. If excess kurtosis is higher than 0, the kurtosis is higher than a normal distribution.

5. Excess kurtosis in Python
01:51 - 02:38
In SciPy, for example, the kurtosis function is actually computing excess kurtosis. If you wanted to calculate the true sample kurtosis, you would actually need to add 3 to the result. But for most cases, you're going to be interested in excess kurtosis anyways, so this functionality is fine as long as you are aware of it. In finance, high excess kurtosis is an indication of high risk. When large movements in returns happen often, this can be a very bad thing for your portfolio if it moves in the wrong direction. High kurtosis distributions are said to have "thick tails", which means that outliers, such as extreme negative and positive returns, are more common.

6. Testing for normality in Python
02:38 - 03:27
Before we move on, let me briefly add one final tool to your arsenal. If the kurtosis of a distribution is greater than 3, and the skewness is non-zero, the data is most likely non-normal. But what if the values are close to, but not quite normal? You can use the Shapiro-Wilk statistical test to estimate the probability that the data is normally distributed. The null hypothesis of the Shapiro-Wilk test is that the data are normally distributed, and when the p-value returned is less than or equal to 0-point-05, that means you can safely reject the null hypothesis and assume that the data are non-normal.

7. Let's practice!
03:27 - 03:33
Now it's your turn.

# Skewness

Third moment: Skewness
To calculate the third moment, or skewness of a returns distribution in Python, you can use the skew() function from scipy.stats.

Remember that a negative skew is a right-leaning curve, while positive skew is a left-leaning curve. In finance, you would tend to want positive skewness, as this would mean that the probability of large positive returns is unusually high, and the negative returns are more closely clustered and predictable.

StockPrices from the previous exercise is available in your workspace.

Import skew from scipy.stats.
Drop missing values in the 'Returns' column to prevent errors.
Calculate the skewness of clean_returns.

```
# Import skew from scipy.stats
from scipy.stats import skew

# Drop the missing values
clean_returns = StockPrices['Returns'].dropna()

# Calculate the third moment (skewness) of the returns distribution
returns_skewness = skew(clean_returns)
print(returns_skewness)
```

Fourth moment: Kurtosis
Finally, to calculate the fourth moment of a distribution, you can use the kurtosis() function from scipy.stats.

Note that this function actually returns the excess kurtosis, not the 4th moment itself. In order to calculate kurtosis, simply add 3 to the excess kurtosis returned by kurtosis().

clean_returns from the previous exercise is available in your workspace.

Import kurtosis from scipy.stats.
Use clean_returns to calculate the excess_kurtosis.
Derive the fourth_moment from excess_kurtosis.

```
# Import kurtosis from scipy.stats
from scipy.stats import kurtosis

# Calculate the excess kurtosis of the returns distribution
excess_kurtosis = kurtosis(clean_returns)
print(excess_kurtosis)

# Derive the true fourth moment of the returns distribution
fourth_moment = excess_kurtosis + 3
print(fourth_moment)
```

# Statistics

Statistical tests for normality
In order to truly be confident in your judgement of the normality of the stock's return distribution, you will want to use a true statistical test rather than simply examining the kurtosis or skewness.

You can use the shapiro() function from scipy.stats to run a Shapiro-Wilk test of normality on the stock returns. The function will return two values in a list. The first value is the t-stat of the test, and the second value is the p-value. You can use the p-value to make a judgement about the normality of the data. If the p-value is less than or equal to 0.05, you can safely reject the null hypothesis of normality and assume that the data are non-normally distributed.

clean_returns from the previous exercise is available in your workspace.

Import shapiro from scipy.stats.
Run the Shapiro-Wilk test on clean_returns.
Extract the p-value from the shapiro_results tuple.

```
Import shapiro from scipy.stats.
Run the Shapiro-Wilk test on clean_returns.
Extract the p-value from the shapiro_results tuple.
```
