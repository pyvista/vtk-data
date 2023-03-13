### Cylindrical Hertzian Contact on a Plane

This dataset was generated from OnScale Solve and depicts the finite element
analysis solution to a cylinder in contact with a plane.

Dataset originally from several `vtu` files with a `vtup` header in `Hertzian_cylinder_on_plate.zip` and merged to a single dataset with:

```py
import pyvista
mesh = pv.read(os.path.join('bfac9fd1-e982-4825-9a95-9e5d8c5b4d3e_result_1.pvtu'))

cleaned = mesh.clean()
cleaned.save('hertzian_contact_cylinder_plane.vtu')
```
