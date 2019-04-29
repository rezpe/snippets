# Overview

Astropy is a astrophysics package for Python.

# Representation of stars

```python
from astropy.coordinates import SkyCoord  # High-level coordinates
import astropy.units as u

c = SkyCoord(ra=df[0].values*u.degree, dec=df[1].values*u.degree, frame='icrs')
ra_rad = c.ra.wrap_at(180 * u.deg).radian
dec_rad = c.dec.radian

plt.figure(figsize=(15,10))
plt.subplot(111, projection="aitoff")
plt.grid(True)
plt.scatter(ra_rad, dec_rad, alpha=0.3,
         c=[ ["r","g","b"][labels[i]] for i in range(len(labels))])
plt.show()
```
