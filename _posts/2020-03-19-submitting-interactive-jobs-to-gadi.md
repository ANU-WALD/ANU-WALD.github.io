---
layout: post
title:  Submitting interactive jobs to Gadi
categories: [nci, gadi]
---

## Description

Sometimes you need to run code with specific needs of hardware or memory which are not available on you local computer or VDI. Gadi offers the option of requesting a node though its PBS system in interactive mode, meaning that when the request is accepted you receive an interactive session of the requested machine. This can also be usefull to test code on the supercomputer before submitting to debug problems or make sure everything works as expected.

```
qsub -I -P aa00 -q express -lwalltime=1:00:00,mem=4GB,ncpus=1,storage=gdata/ub8+gdata/xc0+gdata/u39,jobfs=100GB
```

This line has to be written on a Gadi login node. We specify `qsub` the same way as we submit PBS jobs but in this case we specify the option `-I` for interactive mode and the rest of the options describe the project we want to use and the characteristics of the requested node similarly to how this is done in a PBS file.

**In interactive mode you'll probably want to specify the express queue to minimise your waiting time. Be careful with the resources you request in interactive mode. Normally you'll need just one cpu to test or run your code and remember to exit the session when you are done to avoid being charged**
