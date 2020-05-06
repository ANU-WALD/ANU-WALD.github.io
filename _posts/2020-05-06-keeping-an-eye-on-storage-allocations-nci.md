---
layout: post
title:  Command for keeping an eye on NCI storage allocations
categories: [nci, gadi]
---

## Description

Gadi offers a couple of new commands to monitor the storage space of our NCI projects. As you know, resources are limited and we need to keep an eye on how the projects are doing and how much space and number of files we are using out of the allocation. 

The first command is `lquota`, which gives us an overall view of the state of the different projects we belong to. For example:

```
$ lquota
-----------------------------------------------------------------------
           fs      Usage     Quota     Limit   iUsage   iQuota   iLimit
-----------------------------------------------------------------------
   xc0 scratch   21.97GB     1.0TB     2.0TB      979   202000   404000
   rs0 scratch   12.86MB    72.0GB   144.0GB     3292   164000   328000
   fj4 scratch   960.0KB    72.0GB   144.0GB      241    53000   106000
  ai53 scratch   180.0KB    72.0GB   144.0GB       45    53000   106000
   ub8 scratch   140.0KB     5.0TB     8.0TB       35   150000   300000
   r78   gdata   22.36TB    30.0TB    31.5TB  4453467  6000000  6300000
   oe9   gdata    4.13TB     5.0TB    5.25TB   582524  1018000  1068900
   xc0   gdata   14.26TB    20.0TB    21.0TB  1946864  2000000  2100000
   rr5   gdata  551.57TB   585.0TB  614.25TB 22736153 43000000 45150000
   rs0   gdata  543.75TB   700.0TB   735.0TB 16332072 45000000 47250000
   fj4   gdata   17.23TB    20.0TB    21.0TB   750256  1772000  1860600
   ub4   gdata   86.07TB    95.0TB   99.75TB   114083   177000   185850
   ub8   gdata   17.85TB    18.0TB    18.9TB    80916  1627000  1708350
-----------------------------------------------------------------------
```

Gives us information about the quota and current usage in total space and the number of files (Quota and iQuota respectively). 

The other command is `nci-files-report` and this one gives us detailed information about users and theirs different usage shares. For example, if we want to get information about a specific project on gdata:

```
$ nci-files-report -g xc0 -f gdata
------------------------------------------------------------------------------
         project             user     space used      file size          count
------------------------------------------------------------------------------
             xc0           cxh603          804kB          800kB              2
             xc0           ljr599         1479MB         1478MB             51
             fj4           ljr599         32.0kB          4.9kB              4
             xc0           dm7414         6411MB         6467MB             60
             ub8           pl5189         2387MB         2387MB             54
             xc0           pl5189          759GB          753GB        1207898
             fj4           pl5189         21.6GB         21.6GB           1645
             xc0           evg603         1209MB         1209MB            290
             xc0           prl900         4600kB         4355kB            152
             xc0           smm652          278GB          278GB          10042
...

```

Notice that although we are listing the contents of a specific project, other projects get listed. This is because users can keep files owned by any group (user:group) inside a project. Be careful because files count against the quota of the project they belong no matter where they are. A good way of knowing where are our files is by using this command `nci-files-report -u user_id -f gdata` with the right NCI username in it. It's a good practice to keep the ownership and location of files consistent.

Remember, to change the group of a file we can use the `chown` command line as follows:

```
$ chown user_id:group_id my_file.txt
```
