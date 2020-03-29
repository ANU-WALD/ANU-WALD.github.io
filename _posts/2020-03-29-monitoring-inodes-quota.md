---
layout: post
title: Monitoring NCI's project inodes (number of files) usage.
categories: [nci]
---

## Description

Each project on the NCI comes with a certain amount of compute and storage allocation. For storage on the global file system, `gdata`, each project defines a quota for the total size and also the total number of files (inodes) the project can contain. Often, projects reach the limitation on the number of inodes (number of files) with still plenty of storage space left in the project. This is the case when users create a large number of small files. A couple of recommendations to avoid this situation is to try to pack data together using file formats that can contains multiple variables and dimensions, such as NetCDF or HDF. If that is not an option, and we want to keep the data for using it at a later stage, is to pack the files into a `tar` archive, which can convert a directory containinig large numbers of files into a single compressed file. You can use the following command to pack and compress a directory:

```
tar -zcvf archive-name.tar.gz directory-name
```

And then you'll need to delete the original directory containing the files. 

Finally, use this command to list and keep an eye on the number of files in a directory. For example to list the users under project `xc0` sorted by their inodes usage, you can type:

```
$ cd /g/data/xc0/user
$ du -a | cut -d/ -f2 | sort | uniq -c | sort -nr
```

This same command can be used to find out the number of inodes at any other directory on `gdata` and Linux file system.

