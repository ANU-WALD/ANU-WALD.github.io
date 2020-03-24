---
layout: post
title:  The simplest way of running parallel programs on Gadi
categories: [gadi,nci]
---

## Description

Parallelism is not easy and there are many ways of structuring a computer program to make things run concurrently. For example, Python has the multiprocessing library for using more than one processor when executing the code. These libraries are quite complex and debugging errors in the code is difficult. Luckily, for most of the data processing that we do in the group, there is an easier way to achieve paralelism by running multiple independent jobs on Gadi. 

The basic setup for doing this requires a program that accepts input arguments (see argparse library in Python) and a PBS script that also accepts the same arguments and a third script to call them all iterating over the range of values for these parameters. This might sound a little bit complicated, so let's see how this all works using a simple example:

Imagine we have a collection of 100 NetCDF files in the `/g/data/xc0/tiles` directory with names `00.nc` to `99.nc`. We want to compute the mean of the values in each file, saving the output to a text file, which for the first file would be `00.out`.

This is the Python code to perform the operation, saved in a file named `compute_mean.py`:

```
import xarray as xr
import argparse

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="""Compute means of netcdfs""")
    parser.add_argument('-fname', '--file_name', type=str, required=True, help="Specifies the input NetCDF file.")
    args = parser.parse_args()

    ds = xr.open_dataset(args.file_name)
    mean = ds.NDVI.mean().values

    with open(args.file_name[:-2] + "out", "w") as out_file:
        out_file.write("mean value: {}".format(mean))

```

So, to compute the mean of file `44.nc` we would do:

```
$ python compute_mean.py -fname 44.nc
```

Imagine that executing this code takes one hour and we want to use the supercomputer. We can write the following PBS script, with name `gadi_mean.pbs`:

```
#!/bin/bash
#PBS -P aa00
#PBS -q normal
#PBS -l ncpus=1
#PBS -l mem=4GB
#PBS -l walltime=01:00:00

python compute_mean.py -fname $fname
```

Note the `$` symbol in the parameter of our Python script, that is an environment variables in Bash scripts and is something we can define when calling this script. For example to submit a job and process the file `44.nc` we can do:

```
qsub -v "fname=44.nc" gadi_mean.pbs
```

Finally, all we need to submit all our 100 jobs is another script that makes this calls. We can use another Bash script but in this case we use Python:

```
import os
import glob

tiles = glob.glob('/g/data/xc0/tiles/*')

for tile in tiles:
    os.system('qsub -v "fname={}" gadi_mean.pbs'.format(tile))
```

Running this code will submit 100 jobs to Gadi and we will have achieved running our code in parallel. If you want to try this be carefull with the resources that you request and test your code before sending jobs to Gadi as a bug in the last script could result in thousands of jobs being submitted and no allocation for the rest of the group.
