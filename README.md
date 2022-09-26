## PyVista Example Data Respository

This data was originally cloned from git://vtk.org/VTKData.git - these are used for examples in both PyVista and PVGeo.

Feel free to open a [Pull Request](https://github.com/pyvista/vtk-data/pulls) with any data you would like to add. Please create a new directory for any new datasets and add a README with license information so other users can reuse the examples within the limits of the license.

For this data to be useful, you'll need to be able to consume these files.

### Consuming these files

If you're using ``pyvista>=0.37.0`` you can download these files with:

```py
from pyvista import examples

filename = examples.download_file('my-directory/my-file.vtk')
```

This assumes that you had uploaded the file to the `Data` directory as `Data/my-directory/my-file.vtk`.

### Adding the Example to PyVista's Downloads

See the documentation within [downloads.py](https://github.com/pyvista/pyvista/blob/main/pyvista/examples/downloads.py) for adding a new method to download the example file within the ``pyvista.examples`` module.
