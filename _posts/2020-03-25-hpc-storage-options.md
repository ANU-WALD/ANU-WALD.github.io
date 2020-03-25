---
layout: post
title: Data intensive jobs on Gadi - storage options
categories: [gadi,nci]
---

## Description

We can say that any computer program is either limited by compute, memory or IO (input/output). Profiling programs can give us an idea of how it performs and what are the parts of a program that we improve to make it more efficient. 

Processing remote sensing and climate data often requires reading and writing large amounts of data to and from disk which is often one of the slowest parts of the code. If the proportion of time that a program spends reading and writting data to files on disk is large, we can say that the program is "IO bounded".

A supercomputer offers different options for saving files to disk with different levels of performance and capacity. Gadi offers three options:

* gdata (low performance & high capacity)
* scratch (good compromise between performance and capacity)
* JOBFS (high performance & low capacity)

It really depends on the use-case, but it's worth thinking about what your program is doing as moving data to either `scratch` or `JOBFS` instead of using the files on `gdata` can save you hours of computation.

As a rule of thumb, if you have a read/write intensive job and your data fits in `scratch`, moving it there and pointing your program to the new location can make your program much faster. If your program deals with thousands/millions of small files intensively `JOBFS` will probably improve performance significantly.

It's normal practice to move input data to one of these faster locations at the beginning of your script and move the output of the computations back to `gdata` at the end.

`JOBFS` space is requested as part of the job specification in a PBS script by adding this line to the header specifying the desired amount of space:

```
#PBS -l jobfs=100GB
```

And then the allocated path can be accessed using the `$PBS_JOBFS` enviroment variable.

**`scratch` space is wiped automatically after 90 days. Remember to move important data from `scratch` to `gdata` to avoid losing results of your experiments.**
