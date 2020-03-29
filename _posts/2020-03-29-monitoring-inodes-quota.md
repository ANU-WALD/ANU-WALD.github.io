---
layout: post
title: Monitoring our project's inodes (number of files) usage on the NCI.
categories: [nci]
---

## Description

Each project on the NCI comes with a certain amount of compute and storage allocation. For storage allocation on the global file system `gdata`, each project defines a quota for the total size and also the total number of files the project can contain, called inodes. Often, projects reach the limitation on the number of inodes (number of files) with plenty of storage space left in the project. This is the case when users create large numbers of small files. A couple of recommendations to avoid this situation is to try to pack data together using file formats that can contains multiple variables and dimensions, such as NetCDF or HDF. Another option when we want to keep these data for using it at a later stage is to pack all the files into a `tar` archive which can convert a directory containinig large numbers of files into a single compesed file. You can use the following command to pack and compress a directory:

```
tar -zcvf archive-name.tar.gz directory-name
```

Finally, use this command to list and keep an eye on the number of files in a directory. For example to list the users under project `xc0` sorted by their inodes usage, you can type:

```
$ cd /g/data/xc0/user
$ du -a | cut -d/ -f2 | sort | uniq -c | sort -nr
```

This command can be used to find out inodes with any other directory on `gdata` and Linux file system.

