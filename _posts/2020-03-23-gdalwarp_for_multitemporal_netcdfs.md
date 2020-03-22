---
layout: post
title:  A gdalwarp-like script that keeps the temporal dimension in NetCDFs
categories: [gdal,netcdf]
---

## Description

Converting data stored as a NetCDF into a different projection, resolution or extent is a very common task when processing geospatial data. GDAL offers the `gdalwarp` command to perform these operations. Unfortunately, when the source file is a NetCDF contiaining a temporal dimension, the output of `gdalwarp` flattens the temporal dimension returning one band per time in the original file. To preserve the temporal dimension in the output file, there is an internally developed script that wraps around `gdalwarp` functionality with Python to merge back the bands into the original temporal dimension. This script can be found at the following location:

```
/g/data/xc0/software/geotools/gdalwarp_nctime
```

And its use is very similar to the command line `gdalwarp` tool defining similar arguments:

```
$ ./gdalwarp_nctime --help
usage: gdalwarp_nctime [-h] -fin FILE_INPUT -var VARIABLE [-lev LEVEL]
                       [-s_srs SOURCE_SRS] [-t_srs TARGET_SRS] -te
                       TARGET_EXTENT [TARGET_EXTENT ...] -tr TARGET_RESOLUTION
                       -fout FILE_OUTPUT

GDAL Warp for NetCDF4 files with temporal dimension

optional arguments:
  -h, --help            show this help message and exit
  -fin FILE_INPUT, --file_input FILE_INPUT
                        Specifies the input NetCDF file to warp.
  -var VARIABLE, --variable VARIABLE
                        Selects the input variable in NetCDF files.
  -lev LEVEL, --level LEVEL
                        Selects the atmospheric level in climate data NetCDF
                        files.
  -s_srs SOURCE_SRS, --source_srs SOURCE_SRS
                        Selects the target projection reference system as EPSG
                        code.
  -t_srs TARGET_SRS, --target_srs TARGET_SRS
                        Selects the target projection reference system as EPSG
                        code.
  -te TARGET_EXTENT [TARGET_EXTENT ...], --target_extent TARGET_EXTENT [TARGET_EXTENT ...]
                        Extent in target coordinate reference system [xmin
                        ymin xmax ymax].
  -tr TARGET_RESOLUTION, --target_resolution TARGET_RESOLUTION
                        Resolution in target coordinate reference system.
  -fout FILE_OUTPUT, --file_output FILE_OUTPUT
                        Specifies the destination NetCDF file.
```


And here there is an example of the script being called to subset and convert some climate data into a different projection:

```
$ ./gdalwarp_nctime -fin era5_geop_201901.nc -var z -lev 850 -t_srs EPSG:3577 -te 900000 -4524000 1924000 -3500000 -tr 16000 -fout era5_au_z850_201901.nc
```

This is a work in progress and functionality will be improved as it is tested with new use-cases. Please provide feedback or feel free to modify to serve your purposes.
