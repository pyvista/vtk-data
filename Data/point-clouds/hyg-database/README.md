### Stars from the HYG-Database

This dataset was generated from the [HYG-Database](https://github.com/astronexus/HYG-Database)


#### License and Versions:

This data set is licensed by a Creative Commons Attribution-ShareAlike license. For more details, read the Creative Commons page (https://creativecommons.org/licenses/by-sa/2.5/).
 
For additional background details, and older versions of the database, visit  http://www.astronexus.com/hyg.

For the most current version of the applications using this database, visit http://www.astronexus.com/endeavour. **

#### Code to generate

The following code was used to create the "point cloud" of stars based on [hygdata_v3.csv](https://github.com/astronexus/HYG-Database/blob/master/hygdata_v3.csv)


```py

from astropy.table import Table
import numpy as np
import pyvista as pv

# Read stars data into an astropy table
# https://www.astronexus.com/downloads/catalogs/hygdata_v3.csv.gz
stars_table = Table.read("hygdata_v3.csv")


###############################################################################$
# Convert Star B-V color index to apparent RGB color
# See: https://stackoverflow.com/questions/21977786

redco = [ 1.62098281e-82, -5.03110845e-77, 6.66758278e-72, -4.71441850e-67, 1.66429493e-62, -1.50701672e-59, -2.42533006e-53, 8.42586475e-49, 7.94816523e-45, -1.68655179e-39, 7.25404556e-35, -1.85559350e-30, 3.23793430e-26, -4.00670131e-22, 3.53445102e-18, -2.19200432e-14, 9.27939743e-11, -2.56131914e-07,  4.29917840e-04, -3.88866019e-01, 3.97307766e+02]
greenco = [ 1.21775217e-82, -3.79265302e-77, 5.04300808e-72, -3.57741292e-67, 1.26763387e-62, -1.28724846e-59, -1.84618419e-53, 6.43113038e-49, 6.05135293e-45, -1.28642374e-39, 5.52273817e-35, -1.40682723e-30, 2.43659251e-26, -2.97762151e-22, 2.57295370e-18, -1.54137817e-14, 6.14141996e-11, -1.50922703e-07,  1.90667190e-04, -1.23973583e-02,-1.33464366e+01]
blueco = [ 2.17374683e-82, -6.82574350e-77, 9.17262316e-72, -6.60390151e-67, 2.40324203e-62, -5.77694976e-59, -3.42234361e-53, 1.26662864e-48, 8.75794575e-45, -2.45089758e-39, 1.10698770e-34, -2.95752654e-30, 5.41656027e-26, -7.10396545e-22, 6.74083578e-18, -4.59335728e-14, 2.20051751e-10, -7.14068799e-07,  1.46622559e-03, -1.60740964e+00, 6.85200095e+02]

redco = np.poly1d(redco)
greenco = np.poly1d(greenco)
blueco = np.poly1d(blueco)

def temp2rgb(ci):
    temp = 4600 * (1 / (0.92 * ci + 1.7) + 1 / (0.92 * ci + 0.62))

    red = redco(temp)
    green = greenco(temp)
    blue = blueco(temp)

    if red > 255:
        red = 255
    elif red < 0:
        red = 0
    if green > 255:
        green = 255
    elif green < 0:
        green = 0
    if blue > 255:
        blue = 255
    elif blue < 0:
        blue = 0

    color = (int(red)/255,
             int(green)/255,
             int(blue)/255)
    return color


###############################################################################
# Create the pyvista.PolyData
# 

# only include stars that have a valid distance and valid color
valid = np.logical_and(
    stars_table["dist"] != 100000,
    ~stars_table['ci'].mask,
)
coord = np.vstack((
    stars_table['x'],
    stars_table['y'],
    stars_table['z'],
)).T.astype(np.float32)

stars = pv.PolyData(coord[valid])

###############################################################################
# Generate an RGBA array based on the color and luminosity
# Here, we scale the luminosity to make most stars visible 
lum = stars_table['lum'][valid].copy()
lum /= lum.max()
rgba = np.ones((stars.n_points, 4))
rgba[:, -1] = lum**(0.2)

for ii, ci in enumerate(stars_table['ci'][valid]):
    if isinstance(ci, float):
        rgba[ii, :-1] = temp2rgb(ci)

stars['lum'] = stars_table['lum'][valid].astype(np.float32)
stars['ci'] = stars_table['ci'][valid].astype(np.float32)
stars['_rgba'] = (rgba*255).astype(np.uint8)

###############################################################################
# plot it

stars.plot(
    style='points_gaussian',
    background='k',
    point_size=0.5,
    scalars='_rgba',
    render_points_as_spheres=False,
    zoom=3.0,
)

stars.save('/tmp/stars.vtp')
```
