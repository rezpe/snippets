## Time Series with R

## Exploring raw time series

```R
# Print the Nile dataset
print(Nile)

# List the number of observations in the Nile dataset
length(Nile)

# Display the first 10 elements of the Nile dataset
head(Nile,10)

# Display the last 12 elements of the Nile dataset
tail(Nile,12)
```

## Analysis of frequency

```R
# Plot AirPassengers
plot(AirPassengers)

# View the start and end dates of AirPassengers
start(AirPassengers)
end(AirPassengers)

# Use time(), deltat(), frequency(), and cycle() with AirPassengers 
time(AirPassengers)
deltat(AirPassengers)
frequency(AirPassengers)
cycle(AirPassengers)
```

## Imputing missing values

```R 
# Plot the AirPassengers data
plot(AirPassengers)

# Compute the mean of AirPassengers
mean(AirPassengers, na.rm = TRUE)

# Impute mean values to NA in AirPassengers
AirPassengers[85:96] <- mean(AirPassengers, na.rm = TRUE)

# Generate another plot of AirPassengers
plot(AirPassengers)

# Add the complete AirPassengers data to your plot
rm(AirPassengers)
points(AirPassengers, type = "l", col = 2, lty = 3)
```

## Plot time Series

```R
plot(tserie)
plot(diff(tserie))
plot(diff(log(tserie)))

# Plot the Nile data
plot(Nile)

# Plot the Nile data with xlab and ylab arguments
plot(Nile,xlab="Year",ylab="River Volume (1e9 m^{3})")

```

## Timeseries R Object

```R
# Use print() and plot() to view data_vector
print(data_vector)
plot(data_vector)

# Convert data_vector to a ts object with start = 2004 and frequency = 4
time_series <- ts(data_vector,start=2004,frequency=4) 

# Use print() and plot() to view time_series
print(time_series)
plot(time_series)

# Check whether data_vector and time_series are ts objects
is.ts(data_vector)
```

## Transformation

```R
# Log rapid_growth
linear_growth <- log(rapid_growth)

# Generate the first difference of z
dz <- diff(z)

# Generate a diff of x with lag = 4. Save this to dx
dx <- diff(x,lag=4)
```

## White noise

```R
# Simulate a WN model with list(order = c(0, 0, 0))
white_noise <- arima.sim(model = list(order = c(0, 0, 0)), n = 100)

# Plot your white_noise data
ts.plot(white_noise)

# Simulate from the WN model with: mean = 100, sd = 10
white_noise_2 <- arima.sim(model = list(order = c(0, 0, 0)), n = 100, mean = 100, sd = 10)

# Plot your white_noise_2 data
ts.plot(white_noise_2)

# Fit the WN model to y using the arima command
arima(y,order=c(0,0,0))

# Calculate the sample mean and sample variance of y
mean(y)
var(y)
```

## Random Walk

```R
# Generate a RW model using arima.sim
random_walk <- arima.sim(model = list(order = c(0, 1, 0)), n = 100 )

# Plot random_walk
ts.plot(random_walk)

# Calculate the first difference series
random_walk_diff <- diff(random_walk)

# Plot random_walk_diff
ts.plot(random_walk_diff)
  
# Generate a RW model with a drift uing arima.sim
rw_drift <- arima.sim(model = list(order = c(0, 1, 0)), n = 100, mean = 1)

# Plot rw_drift
ts.plot(rw_drift)

# Calculate the first difference series
rw_drift_diff <- diff(rw_drift)

# Plot rw_drift_diff
ts.plot(rw_drift_diff)

# Difference your random_walk data
rw_diff <- diff(random_walk)

# Plot rw_diff
ts.plot(rw_diff)

# Now fit the WN model to the differenced data
model_wn <- arima(rw_diff, order = c(0, 0, 0))

# Copy and paste the value of the estimated time trend (intercept) below
int_wn <- model_wn$coef

# Plot the original random_walk data
ts.plot(random_walk)

# Use abline(0, ...) to add time trend to the figure
abline(0, int_wn)


```

## Example: Stocks Assets

```R
# Plot eu_stocks
plot(eu_stocks)

# Use this code to convert prices to returns
returns <- eu_stocks[-1,] / eu_stocks[-1860,] - 1

# Convert returns to ts
returns <- ts(returns, start = c(1991, 130), frequency = 260)

# Plot returns
plot(returns)

# Use this code to convert prices to log returns
logreturns <- diff(log(eu_stocks))

# Plot logreturns
plot(logreturns)

# Generate means from eu_percentreturns
colMeans(eu_percentreturns)

# Use apply to calculate sample variance from eu_percentreturns
apply(eu_percentreturns, MARGIN = 2, FUN = var)

# Make a scatterplot of DAX and FTSE
plot(DAX, FTSE)

# Make a scatterplot matrix of eu_stocks
pairs(eu_stocks)

# Convert eu_stocks to log returns
logreturns <- diff(log(eu_stocks))

# Plot logreturns
plot(logreturns)

# Make a scatterplot matrix of logreturns
pairs(logreturns)

# Use cov() with DAX_logreturns and FTSE_logreturns
cov(DAX_logreturns, FTSE_logreturns)

# Use cov() with logreturns
cov(logreturns)

# Use cor() with DAX_logreturns and FTSE_logreturns
cor(DAX_logreturns, FTSE_logreturns)

# Use cor() with logreturns
cor(logreturns)
```

## Autocorrelation

```R 
# Define x_t0 as x[-1]
x_t0 <- x[-1]

# Define x_t1 as x[-n]
x_t1 <- x[-n]

# Confirm that x_t0 and x_t1 are (x[t], x[t-1]) pairs  
head(cbind(x_t0, x_t1))
  
# Plot x_t0 and x_t1
plot(x_t0, x_t1)

# View the correlation between x_t0 and x_t1
cor(x_t0, x_t1)

# Use acf with x
acf(x, lag.max = 1, plot = FALSE)

# Confirm that difference factor is (n-1)/n
cor(x_t1, x_t0) * (n-1)/n

# View the ACF of x
acf(x)
```

## AR Model

```R
# Fit the AR model to x
arima(x, order = c(1, 0, 0))

# Copy and paste the slope (ar1) estimate
0.8575

# Copy and paste the slope mean (intercept) estimate
-0.0948

# Copy and paste the innovation variance (sigma^2) estimate
1.022

# Fit the AR model to AirPassengers
AR <- arima(AirPassengers, order = c(1, 0, 0))
print(AR)

# Run the following commands to plot the series and fitted values
ts.plot(AirPassengers)
AR_fitted <- AirPassengers - residuals(AR)
points(AR_fitted, type = "l", col = 2, lty = 2)
```

## MA Model

```R 
# Fit the MA model to x
arima(x, order = c(0,0,1))

# Paste the slope (ma1) estimate below
0.7928

# Paste the slope mean (intercept) estimate below
0.1589

# Paste the innovation variance (sigma^2) estimate below
0.9576

# Fit the MA model to Nile
MA <- arima(Nile, order =  c(0,0,1))
print(MA)

# Plot Nile and MA_fit 
ts.plot(Nile)
MA_fit <- Nile - resid(MA)
points(MA_fit, type = "l", col = 2, lty = 2)

# Make a 1-step forecast based on MA
predict_MA <- predict(MA)

# Obtain the 1-step forecast using $pred[1]
predict_MA$pred[1]

# Make a 1-step through 10-step forecast based on MA
predict(MA,n.ahead=10)

# Plot the Nile series plus the forecast and 95% prediction intervals
ts.plot(Nile, xlim = c(1871, 1980))
MA_forecasts <- predict(MA, n.ahead = 10)$pred
MA_forecast_se <- predict(MA, n.ahead = 10)$se
points(MA_forecasts, type = "l", col = 2)
points(MA_forecasts - 2*MA_forecast_se, type = "l", col = 2, lty = 2)
points(MA_forecasts + 2*MA_forecast_se, type = "l", col = 2, lty = 2)
```

## Arma Model

The arima.sim() command requires that you specify a particular model and a number of observations (n) to simulate from that model. To simulate a MA(1) parameter, specify the MA argument within your model argument. For example, the MA(1) with parameter .9 can be simulated by setting model equal to list(order = c(0, 0, 1), ma = .9). A similar procedure can be used to generate the AR(2) model by creating a vector that specifies the ar coefficients.

```R
# Generate and plot white noise
WN <- arima.sim(model = list(order = c(0, 0, 0)), n = 200)
plot(WN)

# Generate and plot an MA(1) with parameter .9 by filtering the noise
MA <- arima.sim(model = list(order = c(0, 0, 1), ma = .9), n = 200)  
plot(MA)

# Generate and plot an AR(1) with parameters 1.5 and -.75
AR <- arima.sim(model = list(order = c(2, 0, 0), ar = c(1.5, -.75)), n = 200) 
plot(AR)
```

