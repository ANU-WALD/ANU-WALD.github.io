---
layout: post
title: Running multicore programs on Gadi.
categories: [nci,gadi]
---

## Description

In a previous [post](https://anu-wald.github.io/gadi/nci/2020/03/24/simplest-way-to-concurrency-on-gadi.html) we saw how to run programs in parallel on Gadi by creating a single core version of our program and requesting multiple single core nodes modifying the input parameters of our program. This is one approach to achieve parallelism easily but sometimes it's not the most efficient and fast way of running our code.

There is a program under `/g/data/xc0/software/` that helps running code using all the cores in node. The program is called `conc_exec` and this is its help function describing the input parameters that it accepts:

```
$ ./conc_exec --help
Usage of ./conc_exec:
  -c int
    	Concurrency level for executing. (default 2)
  -f string
    	Input file with list of commands to execute.
```

`conc_exec` needs an input text file with the list of taks to perform, one per line. This program will take care of distributing these tasks over the available cores for running in parallel. The tasks could be python, matlab or any other programming language or also command line expressions. For example, here is the contents of a sample `input.txt` file:

```
matlab -r -nodisplay -nojvm 'process_biomass(2011)';
matlab -r -nodisplay -nojvm 'process_biomass(2012)';
matlab -r -nodisplay -nojvm 'process_biomass(2013)';
matlab -r -nodisplay -nojvm 'process_biomass(2014)';
matlab -r -nodisplay -nojvm 'process_biomass(2015)';
matlab -r -nodisplay -nojvm 'process_biomass(2016)';
matlab -r -nodisplay -nojvm 'process_biomass(2017)';
matlab -r -nodisplay -nojvm 'process_biomass(2018)';
matlab -r -nodisplay -nojvm 'process_biomass(2019)';
matlab -r -nodisplay -nojvm 'process_biomass(2020)';
```

Now, we can call the commands in this file using the `conc_exec` program for running in parallel:

```
$ ./conc_exec -f input.txt -c 4
```

This will distribute each line between 4 cores in the node. When one core finishes running the program it will start executing the next line until all the lines have been executed. We can request a node in Gadi with multiple cores and then call `conc_exec` passing a file as input parameter with the list of tasks to complete. Using this program has two benefits compared to submitting individial single core programs: first it will run quicker as we are just requesting one node and second we'll be making a better use of resources as memory in the node will be shared between processes. You'll have to request enough memory to be able to run your program at the desired level of concurrency, which means you'll be limited by either the number of cores available in a node or the total amount of memory. Remember you can use an interactive session to test and profile this.
