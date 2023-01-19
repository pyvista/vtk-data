### magpylib DataSets

The [magpylib](https://github.com/magpylib/magpylib) is a Python package for calculating 3D static magnetic fields of magnets, line currents and other sources. Here, it's used to model the the magnetic field of a coil.

``magpylib`` is licened under the [BSD-2-Clause license](https://github.com/magpylib/magpylib/blob/main/LICENSE).

### coil_field.vti

This example was based the [magpylib - Coil Field Lines](https://magpylib.readthedocs.io/en/latest/examples/examples_30_coil_field_lines.html) example.

```py
import numpy as np
import magpylib as magpy
import pyvista as pv

coil1 = magpy.Collection()
for z in np.linspace(-8, 8, 16):
    winding = magpy.current.Loop(
        current=100,
        diameter=10,
        position=(0,0,z),
    )
    coil1.add(winding)

grid = pv.UniformGrid(
    dimensions=(81, 81, 81),
    spacing=(1, 1, 1),
    origin=(-40, -40, -40),
)

# compute B-field and add as data to grid
grid['B'] = coil1.getB(grid.points)
grid.save('coil_field.vti')
```

