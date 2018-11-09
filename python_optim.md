# Optimization and Speedup tricks in python

## Parallel with multiprocessing

```python
import collections
import multiprocessing

Scientist = collections.namedtuple('Scientist', [
    'name',
    'born',
])

scientists = (
    Scientist(name='Ada Lovelace', born=1815),
    Scientist(name='Emmy Noether', born=1882),
    Scientist(name='Marie Curie', born=1867),
    Scientist(name='Tu Youyou', born=1930),
    Scientist(name='Ada Yonath', born=1939),
    Scientist(name='Vera Rubin', born=1928),
    Scientist(name='Sally Ride', born=1951),
)

def process_item(item):
    return {
        'name': item.name,
        'age': 2017 - item.born
    }

pool = multiprocessing.Pool(multiprocessing.cpu_count())
result = pool.map(process_item, scientists)

print(tuple(result))
```

## Parallel with joblib

```python
#https://joblib.readthedocs.io/en/latest/parallel.html
from joblib import Parallel, delayed
import multiprocessing
     
# what are your inputs, and what operation do you want to 
# perform on each input. For example...
inputs = range(10) 
def processInput(i):
    return i * i
 
num_cores = multiprocessing.cpu_count()
     
results = Parallel(n_jobs=num_cores)(delayed(processInput)(i) for i in inputs)
```

## Numexpr

```python
>>> import numpy as np
>>> import numexpr as ne
>>> a = np.arange(10)
>>> b = np.arange(0, 20, 2)
>>> c = ne.evaluate("2*a+3*b")
>>> c
array([ 0,  8, 16, 24, 32, 40, 48, 56, 64, 72])
```

## Numba

```python
#https://numexpr.readthedocs.io/en/latest/user_guide.html
#https://jakevdp.github.io/blog/2015/02/24/optimizing-python-with-numpy-and-numba/
@numba.jit(nopython=True)
def get_sort(obs_node,est_len,obs_len):
    w=[]
    n=[]
    for node in np.unique(obs_node):
        n.append(node)
        t=np.zeros(obs_len)
        t[obs_node==node]=1/(est_len*sum(obs_node==node))
        w.append(t)
    return w,n

%%time
#https://stackoverflow.com/questions/30003068/get-a-list-of-all-indices-of-repeated-elements-in-a-numpy-array
quantiles = np.arange(1,10)/10.0 
est_len = len(reg.estimators_)
obs_len = len(y_train)

def get_vector(estimator,X_train,y_train):
    obs_node = estimator.apply(X_train)
    return get_sort(obs_node,est_len,obs_len)
    
vectors = [get_vector(estimator,X_train,y_train) for estimator in reg.estimators_]
```

## Cython

In a separate cell, we create the cython optimized function

```python
%%cython -a
import numpy as np
def get_sort(obs_node,est_len,obs_len):
    w = {}
    idx_sort = np.argsort(obs_node)
    sorted_records_array = obs_node[idx_sort]
    uniques, idx_start, counts = np.unique(sorted_records_array, return_counts=True,return_index=True)
    res = np.split(idx_sort, idx_start[1:])
    for i in range(len(uniques)):
        node=uniques[i]
        w[node]=np.zeros(obs_len)
        w[node][res[i]] = 1/(counts[i]*est_len)
    return w
```

Then we can invoke the function from any other cell

```python
%%time
#https://stackoverflow.com/questions/30003068/get-a-list-of-all-indices-of-repeated-elements-in-a-numpy-array
quantiles = np.arange(1,10)/10.0 
est_len = len(reg.estimators_)
obs_len = len(y_train)

def get_vector(estimator,X_train,y_train):
    obs_node = estimator.apply(X_train)
    return get_sort(obs_node,est_len,obs_len)
    
vectors = [get_vector(estimator,X_train,y_train) for estimator in reg.estimators_]
```

