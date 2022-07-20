## Sample GIFs


### `sample.gif`

The `sample.gif` generated with:

```py

import pyvista

filename = 'sample.gif'

pl = pyvista.Plotter(window_size=(300, 200), off_screen=True)
pl.open_gif(filename, palettesize=16, fps=1)

mesh = pyvista.Sphere()
opacity = mesh.points[:, 0]
opacity -= opacity.min()
opacity /= opacity.max()
for color in ['red', 'blue', 'green']:
    pl.clear()
    pl.background_color = 'w'
    pl.add_mesh(mesh, color=color, opacity=opacity)
    pl.camera_position = 'xy'
    pl.write_frame()

pl.close()
```
