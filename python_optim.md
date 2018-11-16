# Optimization and Speedup tricks in python

## Profiling the code

Profiling lets you find the lines of code that are the most time consuming. In jupyter, you load the extension with

```python
%load_ext line_profiler
``` 

Then you can check the function with

```python
%lprun -f get_vector vectors = [get_vector(obs_node,est_len,obs_len) for obs_node in total_obs_node]
```

You should see an output like this

```
Timer unit: 1e-06 s

Total time: 0.51348 s
File: <ipython-input-14-657307a40a51>
Function: get_vector at line 9

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
     9                                           def get_vector(obs_node,est_len,obs_len):
    10                                           
    11       100      94439.0    944.4     18.4      uniques = np.unique(obs_node)
    12       100     125209.0   1252.1     24.4      res = [np.where(obs_node==unique) for unique in uniques]
    13                                               
    14       100      20974.0    209.7      4.1      w = np.zeros((len(uniques),obs_len)) 
    15      3300       2831.0      0.9      0.6      for i in range(len(uniques)):
    16      3200     269698.0     84.3     52.5          w[i][res[i]] = 1/(len(res[i])*est_len)
    17                                                   
    18       100        329.0      3.3      0.1      return zip(uniques,w)
```

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

## Concurrent Futures

```python
import glob
import os
import cv2
import concurrent.futures


def load_and_resize(image_filename):
  ### Read in the image data
  img = cv2.imread(image_filename)
  
  ### Resize the image
  img = cv2.resize(img, (600, 600)) 
  

### Create a pool of processes. By default, one is created for each CPU in your machine.
with concurrent.futures.ProcessPoolExecutor() as executor:
    ### Get a list of files to process
    image_files = glob.glob("*.jpg")

    ### Process the list of files, but split the work across the process pool to use all CPUs
    ### Loop through all jpg files in the current folder 
    ### Resize each one to size 600x600
    executor.map(load_and_resize, image_files)
```
