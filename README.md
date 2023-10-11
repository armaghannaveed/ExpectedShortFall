# ExpectedShortFall
Expected Shortfall Summary
Summary:
Expected Shortfall (ES), also known as Conditional Value at Risk (CVaR), is a risk measure that estimates the expected loss in a given time period, given that the loss exceeds a certain threshold (usually a quantile of the loss distribution). It is considered a more coherent risk measure than Value at Risk (VaR) because it takes into account the tail risk of the loss distribution.

Variables:
To calculate Expected Shortfall, you need the following variables:

1. Loss distribution: The distribution of potential losses for your portfolio or investment.
2. Confidence level (α): The probability level at which you want to estimate the risk. Commonly used confidence levels are 95% or 99%.

Formula
Here's the formula to calculate Expected Shortfall:

ES(α) = (1 / (1 - α)) * ∫[α, 1] VaR(p) dp

where:
- ES(α) is the Expected Shortfall at the α confidence level
- VaR(p) is the Value at Risk at probability level p
- ∫[α, 1] denotes the integral from α to 1

To calculate Expected Shortfall, follow these steps:

1. Determine the loss distribution for your portfolio or investment. This can be done using historical data, Monte Carlo simulations, or other methods.
2. Choose a confidence level (α) for which you want to estimate the risk.
3. Calculate the Value at Risk (VaR) for each probability level (p) between α and 1.
4. Integrate the VaR values over the range [α, 1] and divide by (1 - α) to obtain the Expected Shortfall.

Keep in mind that calculating Expected Shortfall can be computationally intensive, especially for large portfolios or complex loss distributions. In practice, many financial institutions and risk managers use software tools or specialized algorithms to compute Expected Shortfall efficiently.

Approaches

Historical Simulation: This method involves collecting historical data and ranking them from worst to best. The ES is then calculated as the average of the worst losses that are beyond the VaR point.


How to Measure the Expected Shortfall of a Stock Investment In Python | by Khuong Lân Cao Thai | DataDrivenInvestor (important)

2. Monte Carlo Simulation: This method involves generating a large number of random portfolio paths, calculating the portfolio loss at the end of each path, and then determining the average loss of the worst-case scenarios.

https://www.investopedia.com/terms/m/montecarlosimulation.asp

3. Parametric Method: This method involves fitting a statistical distribution to the data (often a normal or t-distribution), and then calculating the expected value of the tail of the distribution beyond the VaR point.

4. Delta-Normal Method: This method involves calculating the ES based on the sensitivity of the portfolio's value to changes in the underlying risk factors, combined with the standard deviation of those risk factors.

Explanation: This method is a type of parametric approach that is used when the portfolio consists of linear derivatives. The Delta-Normal Method calculates the ES based on the sensitivity (delta) of the portfolio's value to changes in the underlying risk factors.
The delta of a portfolio is a measure of how much portfolio is expected to change due to a small change in the value of the underlying asset. This method assumes that changes in the value of the underlying asset are normally distributed.
To calculate the ES using the Delta-Normal Method, we first calculate the portfolio's delta. We then use this delta to adjust the mean and standard deviation of the underlying asset's returns. The ES is then calculated as the expected value of the tail of this adjusted distribution beyond the VaR point.


5. Delta-Gamma Method: This method is an extension of the Delta-Normal method that also into account the curvature of the price function of the portfolio. The ES is then calculated based on this adjusted distribution.

Explanation:
This method is an extension of the Delta-Normal method that also takes into account the curvature (gamma) of the price function of the portfolio. The gamma of a portfolio is a measure of how much the delta of the portfolio is expected to change due to a small change in the value of the underlying asset.
The Delta-Gamma Method is used when the portfolio consists of non-linear derivatives, as it can capture the effects of changes in the delta on the value of the portfolio.
To calculate the ES using the Delta-Gamma Method, we first calculate the portfolio's delta and gamma. We then use these to adjust the mean and standard deviation of the underlying asset's returns. The ES is then calculated as the expected value of the tail of this adjusted distribution beyond the VaR point.
This method provides a more accurate estimate of the ES for portfolios with non-linear derivatives, but it is also more complex and computationally intensive than the Delta-Normal Method.

6. Factor Push Method: This method involves changing one factor at a time while keeping all other factors constant to see the effect on the portfolio value. The ES is then calculated as the average of the worst losses resulting from these changes.

7. Cornish-Fisher Expansion: This method involves adjusting the normal distribution used in the Delta-Normal method to account for skewness and kurtosis in the data. The ES is then calculated based on this adjusted distribution.

8. Quadratic Programming: This method involves solving a quadratic programming problem to calculate the ES. This is often used when calculating the ES of a portfolio with a large number of assets. TOO COMPLEX



Parametric Approach Notes

Parametric Model Components

Elements needed:
Estimation of conditional mean (estimated mean return)
Estimation of conditional variance (standard-deviation of return in a given period)
Assumed distribution for standardized residuals

We can use these elements to create a VAR and/or estimated shortfall figure using the following methods:

VAR(t) = MeanReturn(t) + (Z-score(X%Confidence)*Std(returns))
ES = MeanReturn(t) + E(Z-Score | Z-Score < Z-Score(X%confidence))*Std(return)

Conditional Mean
For conditional mean, we could use either a historical figure, or an ARMA process to estimate forward-looking mean return:

ARMA(1,1): Return(t) = HistoricalMean + B1(Mean(t-1)) + B2(Error(t-1))


3-year estimation period for training

This would lead to a new overall ES formula:


Where the theta terms in both equations are theoretically equal. Not sure why exactly this is, as x is the residual standard deviation and the sigma is the standard deviation of true return data, I believe. 

Studies have generally used ARMA(1,1), lagging just one time-period.

Conditional Variance
For conditional variance, we may choose to use a GARCH model that auto-regresses based on past variation. As follows:

Variance = B0 + B1 (R(t-1) - MeanReturn)^2 + B2 (Variance(t-1))

Some modified GARCH models account for the leverage effect (Black, 1976)

GJR-GARCH is known to account for leverage effect (Huang, 2014, p. 22), as it is an asymmetric model that separates negative and positive differences from the mean, as negative differences usually have a different impact from positive differences on price volatility (Black, 1976)

GJR-GARCH: 



We could also use an exponentially weighted moving average model (EWMA):

Std = SQRT(B1*Variance(t-1) + (1-B1)*Return(t-1)^2).

EWMA is standard. Can search up general beta online.



Distribution
Potential distributions to model Kurtosis
Normal Distribution
Student T Distribution
Student T with Skewness
General Error Distribution (GED)
Skewed General Error Distribution (SGED)

Most likely to fit normal distribution empirically. Start with normal and go from there

Scaling ES to portfolio is complicated

SGED uses the following formula, with a negative skew fitting data better. This is due to returns data following a trend of more frequent but less extreme positive returns and less frequent but more extreme negative returns.

Which Overall Model is Best:
According to (Huang, 2014), ARMA(1,1)-GJR-GARCH(1,1)-SGED produces the most balanced results for both VAR and ES. 

Resources:
Model summaries and coding - ritvikmath
Research paper that discusses historical, parametric, and semi-parametric approaches: https://dam-oclc.bac-lac.gc.ca/download?is_thesis=1&oclc_number=1356861209&id=48f8e31a-14a4-4ed4-be75-c256f4c38e67&fileName=Huang_Xinxin.pdf
https://lup.lub.lu.se/luur/download?func=downloadFile&recordOId=9018400&fileOId=9018405
https://minerva.it.manchester.ac.uk/~saralees/chap17.pdf - Packages and software functionality that may be useful

Questions:
What timeframe should I use for the conditional mean?
Should I be using daily returns for conditional variance calculations where most models regress on the previous day, or would monthly be more appropriate given we are rebalancing the model monthly
How should I train the model? How far back should I go with historical data, and should I train with monthly or daily data? According to (Huang, 2014), the GARCH formula can be trained using a maximum likelihood estimation, assuming that standardized residuals form a standard normal distribution. Would this be the same as an OLS regression?
Any ideas for how to go about out-of-sample testing? How much data should be used to test vs. train, what time period can be used for testing, etc.
To Do:
Continue to browse literature for best parametric models.
Implement basic functionality of the model including parameter calculation
Test model and revise

Historical Approach Notes

Josh: Made a short Jupyter notebook that plots the portfolio value and VAR as well  as calculates ES in the output. (Example below is NVIDIA inc.)


Explanation of Lambda:
In the context of Exponentially Weighted Moving Average (EWMA), lambda (λ) is a parameter that determines the weighting given to past observations. EWMA is a statistical method used to calculate the moving average of a time series, where more recent observations are given higher weights compared to older observations.

The formula for EWMA is as follows:

\[ \text{EWMA}_{t} = \lambda \times \text{Observation}_{t} + (1-\lambda) \times \text{EWMA}_{t-1} \]

Where:
- \(\text{EWMA}_{t}\) is the EWMA value at time \(t\),
- \(\text{Observation}_{t}\) is the current observation at time \(t\),
- \(\text{EWMA}_{t-1}\) is the EWMA value at the previous time step \(t-1\),
- \(\lambda\) is the smoothing parameter, also known as the decay factor or the weight given to the current observation, and it satisfies \(0 \leq \lambda \leq 1\).

The value of lambda determines the degree of smoothing applied to the time series data. A smaller value of lambda puts more weight on recent observations, resulting in a faster response to changes in the data. Conversely, a larger value of lambda gives more weight to older observations, leading to a smoother average that responds more slowly to changes.

For example:
- If \(\lambda = 0.1\), approximately 91% of the weight is given to the last observation, and only about 9% to the EWMA value from the previous time step.
- If \(\lambda = 0.5\), the weight is equally distributed between the current observation and the EWMA value from the previous time step.
- If \(\lambda = 0.9\), about 91% of the weight is given to the EWMA value from the previous time step, and only around 9% to the current observation.

In summary, the lambda parameter in EWMA allows you to control the trade-off between responsiveness to recent changes and smoothness in the resulting average. Choosing an appropriate value for lambda depends on the characteristics of your data and the specific application. A smaller lambda is suitable for situations where you want to capture rapid changes in the time series, while a larger lambda is better for smoothing out noise and minor fluctuations in the data.

9. Fourier Transform Methods: These methods involve using Fourier transforms to calculate the ES. These are often used when distribution is not known or is difficult to calculate directly.

This approach doesn’t require the calculation of a return distribution. Instead, it uses Fourier transformations and an exponential smoothing (exponential weighted moving average) algorithm to calculate volatility on a time-series basis. It then uses complex mathematics to calculate an expected shortfall.

