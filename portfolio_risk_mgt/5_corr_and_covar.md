Correlation and co-variance
00:00 - 00:18
Modern portfolio theory teaches that you can actually build a portfolio of assets which has less risk than any of the underlying assets alone. How is this possible? In short - it's all about correlation and co-variance.

2. Pearson correlation
00:18 - 00:41
Pearson correlation is a measure of the linear correlation between two variables, in our case, between the returns of two stocks. Pearson correlation ranges from -1 to 1 where 1 is perfect positive correlation, -1 would be perfect negative correlation, and 0 would be no correlation.

3. Pearson correlation
00:41 - 00:57
You can calculate the correlation coefficient for every pair of stocks in your portfolio, and this results in a matrix of correlations. I won't bore you with the formula, since you're simply going to be using Python to easily calculate the pearson correlation matrix.

4. Correlation matrix in Python
00:57 - 01:18
Assuming you have a pandas DataFrame of stock returns, all you need to do is call the dot corr() method on the DataFrame. You'll notice that the diagonals of the matrix are always equal to 1. This is because any variable or asset will always be perfectly correlated with itself, of course.

5. Portfolio standard deviation
01:18 - 01:35
Now we can use the correlation in a formula to calculate the portfolio standard deviation for a two asset portfolio. But if you have more assets in your portfolio, this formula will get very complicated very quickly. This is where the covariance matrix will come in handy.

6. The Co-variance matrix
01:35 - 01:56
Correlation is actually a normalized measure of the co-variance. Co-variance measures the joint variability of two random variables, and in finance, the co-variance matrix is often used for portfolio optimization and risk management purposes. There are many different ways to derive a co-variance matrix, but of course for this course we'll just use a handy Python function!

7. The Co-variance matrix in Python
01:56 - 02:11
All you need to do is call the dot cov() method on the DataFrame of daily stock returns.

8. Annualizing the covariance matrix
02:11 - 02:32
Now, you might recall that the variance of a stock scales linearly with time, or the volatility with the square root of time. In this case, since it is a variance-covariance matrix, all you need to do to annualize the covariance matrix is multiply it by the number of trading days in a year, that is 252.

9. Portfolio standard deviation using covariance
02:32 - 02:47
Remember that nasty formula for portfolio standard deviation using correlation? Well, we can collapse the formula into a much simpler one using the covariance matrix, but there's a few new concepts to introduce as well.

10. Matrix transpose
02:47 - 03:00
The first is the transpose operation, represented as the capital T. This simply means you flip the dimensions of a matrix, as if you are rotating it by 90 degrees.

11. Dot product
03:00 - 03:16
The second is the dot product operator. Also known as the inner product, it is quite common in linear algebra. Don't let it scare you, it's simply a shorthand for the sum of the products of the elements of both matrices.

12. Portfolio standard deviation using Python
03:16 - 04:04
Don't worry, this is easy to do in Python as long as you keep track of everything. Assuming you have already calculated portfolio weights and the covariance matrix, follow the formula from the previous slide to calculate portfolio standard deviation. If you pass in the daily covariance matrix, the result is daily portfolio volatility. Likewise if you pass in an annualized matrix, the portfolio volatility will be annualized as well. The dot T operator in numpy does the transpose of the weights array, and the np dot dot() and np dot sqrt() functions allow you to compute the dot product and square root, respectively.

13. Let's practice!
04:04 - 04:13
Now let's try some examples.

# The correlation matrix
The correlation matrix can be used to estimate the linear historical relationship between the returns of multiple assets. You can use the built-in .corr() method on a pandas DataFrame to easily calculate the correlation matrix.

Correlation ranges from -1 to 1. The diagonal of the correlation matrix is always 1, because a stock always has a perfect correlation with itself. The matrix is symmetric, which means that the lower triangle and upper triangle of the matrix are simply reflections of each other since correlation is a bi-directional measurement.

In this exercise, you will use the seaborn library to generate a heatmap.

Instructions 1/2
50 XP
1
2
Calculate the correlation_matrix of the StockReturns DataFrame.

```
# Calculate the correlation matrix
correlation_matrix = StockReturns.corr()

# Print the correlation matrix
print(correlation_matrix)
```

Import seaborn as sns.
Use seaborn's heatmap() function to create a heatmap map correlation_matrix

```
# Import seaborn as sns
import seaborn as sns

# Create a heatmap
sns.heatmap(correlation_matrix,
            annot=True,
            cmap="YlGnBu", 
            linewidths=0.3,
            annot_kws={"size": 8})

# Plot aesthetics
plt.xticks(rotation=90)
plt.yticks(rotation=0) 
plt.show()
```

![image](https://github.com/user-attachments/assets/a4e7feab-f903-44c0-a179-3f3396c8e88a)

# The co-variance matrix
You can easily compute the co-variance matrix of a DataFrame of returns using the .cov() method.

The correlation matrix doesn't really tell you anything about the variance of the underlying assets, only the linear relationships between assets. The co-variance (a.k.a. variance-covariance) matrix, on the other hand, contains all of this information, and is very useful for portfolio optimization and risk management purposes.

Instructions
100 XP
Calculate the co-variance matrix of the StockReturns DataFrame.
Annualize the co-variance matrix by multiplying it with 252, the number of trading days in a year.

```
# Calculate the covariance matrix
cov_mat = StockReturns.cov()

# Annualize the co-variance matrix
cov_mat_annual = cov_mat*252

# Print the annualized co-variance matrix
print(cov_mat_annual)
```

# Portfolio standard deviation
In order to calculate portfolio volatility, you will need the covariance matrix, the portfolio weights, and knowledge of the transpose operation. The transpose of a numpy array can be calculated using the .T attribute. The np.dot() function is the dot-product of two arrays.

The formula for portfolio volatility is:


: Portfolio volatility
: Covariance matrix of returns
w: Portfolio weights (
 is transposed portfolio weights)
 The dot-multiplication operator
portfolio_weights and cov_mat_annual are available in your workspace.

![image](https://github.com/user-attachments/assets/fbf215e0-5bd7-4e1b-9032-83faef6c2150)

Calculate the portfolio volatility assuming you use the portfolio_weights by following the formula above.

```
# Import numpy as np
import numpy as np

# Calculate the portfolio standard deviation
portfolio_volatility = np.sqrt(np.dot(portfolio_weights.T, np.dot(cov_mat_annual, portfolio_weights)))
print(portfolio_volatility)  
```
