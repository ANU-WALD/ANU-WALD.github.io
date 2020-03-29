---
layout: post
title: Sharing files across users on the NCI
categories: [nci,gadi]
---

## Description

NCI organises users and data by projects. Users belonging to the same project can easily share files using the space under `/g/data/proj_code`. However, sometimes we need to share a file with other NCI users with no project in common. For this cases, Gadi has a dedicated place on the file system where all the NCI users can access, read and write. This location is `/scratch/public` and we can all copy files and create directories there for sharing them with colleagues. The Linux permission rules for the files and directories created there apply the same way so remember to `chmod` to the appropriate access level using the desired combination of ([u]ser, [g]roup, [a]ll) and ([r]ead, [w]rite, e[x]ecute).

Here is a copy of the `README` file in this location which explains its intended use and data policies applied.

```
$ cat /scratch/public/README
ABOUT THIS DIRECTORY
/scratch/public is for sharing files between NCI users across different
projects. Files in this directory are still counted towards your /scratch quota.

EXPIRY OF FILES
Any files placed into this directory will be automatically removed without
warning 2 weeks after they have been placed into this directory. Files removed
in this way can not be recovered.

PERMISSIONS
Please ensure that the permissions of any files you place in this directory
restrict access to the users or groups you expect them to. There is no
protection provided by default so you may be sharing your files with all users
of Gadi.
```
