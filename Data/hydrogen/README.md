### Model of Probability Density Of The Hydrogen Atom Orbital 3d

Dataset originally obtained from [hydrogen.vtk](https://www.it.uu.se/edu/course/homepage/vetvis/ht11/ComputerExercises/data/a1/hydrogen.vtk)

Dataset rotate to be aligned to the Z-axis with:

```py
import pyvista as pv
dataset = pv.read('hydrogen.vtk')
dat = dataset.point_data['probability_density'].reshape(dataset.dimensions, order='F')
dataset['probability_density'] = np.swapaxes(dat, 1, 2).ravel()
dataset.save('hydrogen.vti')
```

See https://github.com/pyvista/pyvista/pull/3811 for more details.
