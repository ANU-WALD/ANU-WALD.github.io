---
layout: post
title:  Solving _Fillvalue type mismatch error on Thredds requests
categories: [thredds,netcdf,nci]
---

## Description

Thredds allows to share netCDF data with the community freely over the internet. WALD has a directory under the [NCI Thredds](http://dap.nci.org.au/thredds/catalog.html) server where we share some of the datasets we produce. 

Some netcdfs storing data as Bytes can cause problems when accessed through DAP (the most common protocol used by THREDDS).

For example, using Python XArray to access one of our files returns this error:

```
ds = xr.open_dataset("http://dapds00.nci.org.au/thredds/dodsC/ub8/au/LandCover/OzWALD_LC/WCF_2018_mosaic_AustAlb_25m.nc")
...
OSError: [Errno -45] NetCDF: Not a valid data type or _FillValue type mismatch: b'http://dapds00.nci.org.au/thredds/dodsC/ub8/au/LandCover/OzWALD_LC/WCF_2018_mosaic_AustAlb_25m.nc'
```

This is apparently a common and known problem related to Thredds configuration and the original developers have provided a workaround:

```
ds = xr.open_dataset("http://dapds00.nci.org.au/thredds/dodsC/ub8/au/LandCover/OzWALD_LC/WCF_2018_mosaic_AustAlb_25m.nc#fillmismatch")
```

Notice the `#fillmismatch` at the end of the url which fixes the problem. Here is the [link](https://github.com/Unidata/netcdf-c/issues/1299#issuecomment-458312804) to the original discussion where one of the thredds developers explains the problem and how to fix it.

