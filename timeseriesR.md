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

