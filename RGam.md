# Non linear relationship in R

## Linear Regression

```R
# Examine the mcycle data frame
head(mcycle)
plot(mcycle)

# Fit a linear model
lm_mod <- lm(accel~times, data = mcycle)

# Visualize the model
termplot(lm_mod, partial.resid = TRUE, se = TRUE)
```

## Fitting a GAM

```R
# Load mgcv
library(mgcv)

# Fit the model
gam_mod <- gam(accel ~ s(times), data = mcycle)

# Plot the results
plot(gam_mod, residuals = TRUE, pch = 1)

# Extract the model coefficients
coef(gam_mod)
```

## Changing the basis function

```R
library(mgcv)

# Fit a GAM with 3 basis functions
gam_mod_k3 <- gam(accel ~ s(times, k = 3), data = mcycle)

# Fit with 20 basis functions
gam_mod_k20 <- gam(accel ~ s(times, k = 20), data = mcycle)

# Visualize the GAMs
par(mfrow = c(1, 2))
plot(gam_mod_k3, residuals = TRUE, pch = 1)
plot(gam_mod_k20, residuals = TRUE, pch = 1)
```

## Changing the smoothness function

```R
library(mgcv)
# Extract the smoothing parameter
gam_mod <- gam(accel ~ s(times), data = mcycle, method = "REML")
gam_mod$sp

# Fix the smoothing parameter at 0.1
gam_mod_s1 <- gam(accel ~ s(times), data = mcycle, sp = 0.1)

# Fix the smoothing parameter at 0.0001
gam_mod_s2 <- gam(accel ~ s(times), data = mcycle, sp = 0.0001)

# Plot both models
par(mfrow = c(2, 1))
plot(gam_mod_s1, residuals = TRUE, pch = 1)
plot(gam_mod_s2, residuals = TRUE, pch = 1)
```
 
 ## Multivariate GAM
 
 ```R
library(mgcv)

# Examine the data
head(mpg)
str(mpg)

# Fit the model
mod_city <- gam(city.mpg ~ s(weight) + s(length) + s(price), 
                data = mpg, method = "REML")

# Plot the model
plot(mod_city, pages = 1)
```

## Categorical Terms

```R
library(mgcv)

# Fit the model
mod_city2 <- gam(city.mpg ~ s(weight) + s(length) + s(price) + fuel + drive + style, 
                 data = mpg, method = "REML")

# Plot the model
plot(mod_city2, all.terms = TRUE, pages = 1)
``` 

## Category-level smooths for different auto types

```R
library(mgcv)
# Fit the model
mod_city3 <- gam(city.mpg ~ s(weight, by = drive) + s(length, by = drive) + s(price, by = drive) + drive,
                 data = mpg, method = "REML")

# Plot the model
plot(mod_city3, pages = 1)
```



