---
layout: post
title: Transforming between coordinate reference systems (using Python)
categories: [python]
---

## Description

Proj4 is probably the library for converting coordinates between different reference systems or projections. The library is quite old and written in C, so most of its users use it through other interfaces such as QGIS or GDAL. Most programming languages also offer libraries that wrap Proj4 and allow using its functionality. In Python this library is called `pyproj` which allows using Proj4 functions in Python programs. 

The best way of understanding how this library work is seeing an example. The following code defines a function `xy2tile()` which transform a coordinate in sinusoidal projection into the Modis tile that contains this coordinate. The program first converts a `(lat, lon)` coordinate into sinusoidal projection and then calls this function to find the horizontal and vertical Modis tile numbers. It demonstrate a basic but useful functionality of `pyproj` but there is much more that it can do.

```
from pyproj import Proj, transform
import math

xExtentModis  = 1111950.519666
yExtentModis  = 1111950.519667
sinu_proj = "+proj=sinu +lon_0=0 +x_0=0 +y_0=0 +a=6371007.181 +b=6371007.181 +units=m +no_defs "
wgs84_proj = "+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs "

def xy2tile(x, y):
    return int(math.floor(x/xExtentModis)) + 18, -1*int(math.ceil(y/yExtentModis)) + 9

if __name__ == "__main__":

    inProj = Proj(wgs84_proj)
    outProj = Proj(sinu_proj)

    # Canberra (Australia)
    lat = -35.28
    lon = 149.13
    x, y = transform(inProj,outProj,lon,lat)
    print(x, y)
    print(xy2tile(x, y))

    # Pamplona (Spain)
    lat = 42.81
    lon = -1.64
    x, y = transform(inProj,outProj,lon,lat)
    print(x, y)
    print(xy2tile(x, y))
```
