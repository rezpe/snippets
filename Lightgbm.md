# LightGBM 

This is a fast implementation of Gradient Boost Trees.

```python
import lightgbm as lgb

# Creating the regressor
reg = lgb.LGBMRegressor(n_estimators=self.n_estimators,
                         random_state=2020,
                         max_depth=self.max_depth)

# Creating a validation subset             
ind = X_train.index
ind_val = np.random.choice(ind.values,100,replace=False)
ind_train = ind[~ind.isin(ind_val)]

# Fitting/training with early stopping
reg.fit(X_train.loc[ind_train], y_train.loc[ind_train], 
                    early_stopping_rounds=20,eval_set=[(X_train.loc[ind_val], y_train.loc[ind_val])],
                   verbose=0)
```
