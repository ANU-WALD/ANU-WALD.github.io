---
layout: post
title: A fast alternative to tar.gz
categories: [nci,vdi]
---

## Description

There was one directory one the NCI containing 15,000 numpy data files which I didn't need anymore but wanted to hold on for a while just in case. I previously posted about the need to keep the number of files under control for our projects and I used the suggested method of creating a tar.gz doing:

```
tar -xvfz blobs.tar.gz blobs/

# blobs is the name of the directory containing the numpy files
```

It turns out that this operation is very slow. It would have taken nearly 12 hours to run this command on the VDI. The reason for this is that every single file gets compressed individually using the GZip algorithm, which is very slow.

There are other compression algorithms available and LZ4 is currently famous for being fast and efficient so I tried this:

```
tar cvf - blobs/ | lz4 > blob.tar.lz4
```

Which in Linux bash language is a combination of two operations. First, tar packs the contents of the directory into a single file and then the result gets piped `|` into the second operation, which applies the LZ4 compression.

I was able to run this command in less than 30 minutes and I reckon the resulting compression ratio is similar to GZip, so all benefits.

And if you want to decompress or recover the tarred directory:

```
lz4 -dc blob.tar.lz4
```
