---
layout: post
title:  "Submitting interactive jobs to Gadi"
---

## Description

Sometimes you need to run code with specific needs of hardware or memory which are not available on you local computer or VDI. Gadi offers the option of requesting a node though its PBS system in interactive mode, meaning that when the request is accepted you receive an interactive session of the requested machine. This can also be usefull to test code on the supercomputer before submitting to debug problems or make sure everything works as expected.

```
qsub -I -P aa00 -q express -lwalltime=1:00:00,mem=4GB,ncpus=1,storage=gdata/ub8+gdata/xc0+gdata/u39,jobfs=100GB
```
