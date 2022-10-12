# Halo Point Cloud from Findus23

These datasets were originally downloaded from [pyvista/pyvista
#2576](https://github.com/pyvista/pyvista/discussions/2576):

> The particles correspond to (same mass) particles in a cosmological
> simulation and filtered to only show one resulting dark matter halo.

There are two arrays in this directory were compressed via:

```py
import numpy as np

array = np.genfromtxt('halo_high_res.csv', skip_header=1, delimiter=',')
np.save('/tmp/halo_low_res.npy', array.astype(np.float32))
array = np.genfromtxt('halo_low_res.csv', skip_header=1, delimiter=',')
np.save('/tmp/halo_high_res.npy', array.astype(np.float32))
```

And are acessible within pyvista with:

```py
>>> from pyvista import examples
>>> low_res = examples.download_cloud_dark_matter()
>>> hig_res = examples.download_cloud_dark_matter_dense()
>>> hig_res

```
