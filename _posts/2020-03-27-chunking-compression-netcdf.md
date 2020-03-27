---
layout: post
title: Compression and chunking in NetCDF files
categories: [netcdf]
---

## Description

NetCDF is a file format that can be defined as a numerical multidimensional data container. Having this kind of container where we can add different variables and stack data over the different dimensions is very conveneint for organisation purposes and for reducing the number of files. However the size of the resulting files can be a problem. In this case, applying compression can significantly reduce the size of NetCDF files.

How do I know if a NetCDF has compression being applied?:

```
# We use ncdump to list the header info

$ module load netcdf
$ ncdump -sh /g/data/ub8/au/FMC/c6/mosaics/fmc_c6_2003.nc
netcdf fmc_c6_2003 {
dimensions:
	time = UNLIMITED ; // (92 currently)
	longitude = 8200 ;
	latitude = 6800 ;
variables:

...

float fmc_stdv(time, latitude, longitude) ;
		fmc_stdv:long_name = "Standard Deviation Live Fuel Moisture Content" ;
		fmc_stdv:units = "%" ;
		fmc_stdv:_FillValue = -9999.9f ;
		fmc_stdv:missing_value = -9999.9f ;
		fmc_stdv:_Storage = "chunked" ;
		fmc_stdv:_ChunkSizes = 1, 340, 410 ;
		fmc_stdv:_DeflateLevel = 4 ;
		fmc_stdv:_Endianness = "little" ;

...

```

The attributes on the `fmc_stdv` variable tell us that this file is using compression `_DeflateLevel = 4`, the number indicating the level of compression in a [0-9] scale. See at the end of this post how to apply compression to a NetCDF using the `nccopy` tool.

The previous file has also some more information telling us that the file uses chunking `_Storage = "chunked"` and the size of these chunks `_ChunkSizes = 1, 340, 410`. Chunking allows us to divide a file in smaller cubes, called chunks, and apply compression to each of them individually. The benefit of using chunking is that, when reading a subset of the NetCDF, we won't need to decompress the entire file, just the chunks that contain our query. Chunking can improve the read time of a file significatly, specially when reading small subsets. Chunking and compression are quite interesting concepts and is tricky to get them right -- choosing the right size for your chunks really depends on the data and type of queries that the files are intended to receive. We just want to cover the basics of this functionality and flag that you can use this functionality if one day you notice that your NetCDFs are becoming too large or slow.

Finally, `nccopy` is a tool available on the NCI systems for applying compression and chunking to NetCDF files and converting files between NetCDF versions. For example, to apply compression to a NetCDF file we can do:

```
$ module load netcdf
$ nccopy -d 4 input.nc output.nc
```

Here is the help documentation for `nccopy` where all the options for creating performant NetCDFs are listed:

```
nccopy [-k output_kind] [-d level] [-s] [-c chunkspec]
       [-u] [-m n] [-h n] [-e n] input output

  [-k output_kind] kind of output netCDF file
              omitted => same as input
              '1' or 'classic' => classic file format
              '2' or '64-bit-offset' => 64-bit offset format
              '3' or 'netCDF-4' =>  netcdf-4 format
              '4' or 'netCDF-4 classic  model' => netCDF-4 classic model
  [-d level]  deflation level, from 1 (faster but lower compression)
              to 9 (slower but more compression)
  [-s]        shuffling option, sometimes improves compression
  [-c chunkspec] specify chunking for dimensions, e.g. "dim1/N1,dim2/N2,..."
  [-u]        convert unlimited dimensions to fixed size in output
  [-m n]      memory buffer size (default 5 Mbytes)
  [-h n]      set size in bytes of chunk_cache for chunked variables
  [-e n]      set number of elements that chunk_cache can hold
  input       name of input file or OPeNDAP URL
  output      name of output file
```

