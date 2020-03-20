---
layout: post
title:  GDAL and NetCDFs
categories: [gdal]
---

## Description

GDAL is a C++ library which also offers a series of command line tools for reading and converting multiple raster data formats. On the NCI there are several version of GDAL available. To load GDAL you'll need to type:

```
module load gdal
```

Or to load a specific version:

```
module load gdal/2.2.2
```

GDAL is quite old and was designed to work with GeoTIFFs mainly. NetCDFs can also be used but the interface requires of a weird notation to make it work. The problem is that NetCDFs can contain multiple variables and we need to be explicit to GDAL about the variable that we refer in a NetCDF file. For example if we use `gdalinfo` to display the information of a flammability index NetCDF file on the NCI:

```
$ gdalinfo /g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc
Driver: netCDF/Network Common Data Format
Files: /g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc
Size is 512, 512

...

Subdatasets:
  SUBDATASET_1_NAME=NETCDF:"/g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc":flammability
  SUBDATASET_1_DESC=[88x2400x2400] flammability (32-bit floating-point)
  SUBDATASET_2_NAME=NETCDF:"/g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc":anomaly
  SUBDATASET_2_DESC=[88x2400x2400] anomaly (32-bit floating-point)
  SUBDATASET_3_NAME=NETCDF:"/g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc":quality_mask
  SUBDATASET_3_DESC=[88x2400x2400] quality_mask (8-bit integer)
Corner Coordinates:
Upper Left  (    0.0,    0.0)
Lower Left  (    0.0,  512.0)
Upper Right (  512.0,    0.0)
Lower Right (  512.0,  512.0)
Center      (  256.0,  256.0)
```

At the end of the output returned by `gdalinfo` there is a section listing the subdatasets contained in this file with a 'handler', which is how GDAL refers to each variable.

Now, we can use this handler with `gdalinfo` to visualise the contents of each variable in the NetCDF:

```
$ gdalinfo NETCDF:"/g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc":flammability/g/data/ub8/au/FMC/c6/flam_c6_2001_h29v11.nc
```

This command will print information relative to the Coordinate Reference System (CRS) of the variable, extents will also list the temporal dimension as bands in the file. This is another limitation of GDAL not being able to handle multidimensional files properly and having to map the extra dimensions into bands as if it was a GeoTIFF.

