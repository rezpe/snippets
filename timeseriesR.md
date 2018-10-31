## Time Series with R

## Plot time Series

```R
plot(tserie)
plot(diff(tserie))
plot(diff(log(tserie)))
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

