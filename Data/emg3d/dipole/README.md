### Simple Dipole Example

The datasets here are generated using [emg3d](https://github.com/emsig/emg3d) from the [Minimum Example](https://emsig.xyz/emg3d-gallery/gallery/tutorials/minimum_example.html):

```py
import emg3d
import pyvista as pv

src_coo = (0, 0, 0, 0, 0)  # (x, y, z, azimuth, elevation)
frequency = 160
source = emg3d.TxElectricDipole(coordinates=src_coo)
grid = emg3d.construct_mesh(
    center=src_coo[:3],   # Center of wanted grid
    frequency=frequency,  # Frequency we will use the grid for
    properties=2,         # Reference resistivity
    domain=[-800, 800],   # Domain in which we want precise results
)
model = emg3d.Model(grid, property_x=1.5, property_y=1.8, property_z=3.3)
efield = emg3d.solve_source(model=model, source=source, frequency=20, verb=4)

# save the electromagnetic field
shape = efield.fx.shape
efield_grid = pv.UniformGrid(dims=shape)
efield_grid['efield_fx'] = np.abs(efield.fx.ravel('F')).astype(np.float32)
efield_grid['efield_fy'] = np.abs(efield.fy.ravel('F')).astype(np.float32)
efield_grid['efield_fz'] = np.abs(efield.fz.ravel('F')).astype(np.float32)
efield_grid.save('efield.vtk')


# save the magnetic field
hfield = emg3d.get_magnetic_field(model=model, efield=efield)

hfield_grid = pv.UniformGrid(dims=hfield.fx.shape)
hfield_grid['hfield_fx'] = np.abs(hfield.fx.ravel('F')).astype(np.float32)
hfield_grid['hfield_fy'] = np.abs(hfield.fy.ravel('F')).astype(np.float32)
hfield_grid['hfield_fz'] = np.abs(hfield.fz.ravel('F')).astype(np.float32)
hfield_grid.save('hfield.vtk')

```
