 ## Estadistica descriptiva
 
 ```python
 # Numero que resume
 media = np.mean(datos)
 varianza = np.var(datos)
 
 # Histograma
 plt.hist(datos,bins=50)
 # Histograma de densidades
 plt.hist(datos,bins=50,density=True)
 ```
 
 ## Probabilidad Y Variable aleatoria
 
 ```python
 # P(Evento) = Num Casos Favorables / Num Casos Totales
 len(data[(data>4)&(data<6)])/len(data)
 
 # Variable aleatoria
 from scipy import stats
 mi_dist = stats.norm(3,2)
 mi_dist = stats.expon(scale=1/4)
 mi_dist = stats.uniform(loc=3,scale=6-3)
 
 # Prob
 # P(4<X<6)
 mi_dist.cdf(6)-mi_dist.cdf(4)
 ```
 
 ## Regresion Lineal
 
 ```Python
 # Calcular
 plt.scatter(area,precios)
 
 # Correlación
 np.corrcoef(area,precios)[0][1]
 
 result = stats.linregress(area,precios)
 
 # Plotear
 plt.plot(area,area*result.slope+result.intercept)
 ``` 
 
 ## Inferencia
 
 ```Python
 # Metodo momentos
 
 mu_est = np.mean(data)
 s_est = np.std(data)
 norm_est = stats.norm(mu_est,s_est)
 
 # Metodo de maxima verosimilitud
 import tensorflow as tf
import tensorflow_probability as tfp
tfd = tfp.distributions

optimizer = tf.keras.optimizers.Adam(learning_rate=0.01)

concentration = tf.Variable(1.0)
rate=tf.Variable(1.0)
d = tfd.Gamma(concentration=concentration, rate=rate)

for i in range(1000):
  with tf.GradientTape() as tape:
    log_vero = tf.reduce_sum(d.log_prob(data))
    loss = -log_vero
    gradients = tape.gradient(loss,d.trainable_variables)     
    optimizer.apply_gradients(zip(gradients, d.trainable_variables))
```
 
## Muestreo

```Python
# Tamano de la muestra y los parametros de la pob original --> Distribucion de la media muestral
# Ej: n=10 mu=5 sigma=2
n=10
mi_t = stats.t(n-1)
#P(Xm<7)
mu=5
s=2
mi_t.cdf((7-mu)/s/np.sqrt(n))
``` 

## Test

```Python
# Test de Normalidad - P-valor < 0.05 --> Rechazas que es normal
stats.shapiro(data)

# Test de medias
# Ej: Misma varianza en las poblaciones o misma poblacion
stats.ttest_rel(muestra1,muestra2)
```
 
 
 
 
 
 
 
 
 
 
 
