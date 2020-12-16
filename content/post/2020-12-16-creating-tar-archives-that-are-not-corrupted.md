---
title: Creating Tar Archives that are not corrupted
author: Vasant
date: '2020-12-16'
slug: creating-tar-archives-that-are-not-corrupted
categories:
  - TIL
tags:
  - technical
  - til
  - linux
  - sys-admin
subtitle: ''
summary: 'How to compress a multiple directories with tar and gz safely'
authors: []
lastmod: '2020-12-16T10:38:40-05:00'
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

tl;dr version: Make sure you pipe stdout and stderr

Part of my job is to be a sys-admin, this involves dealing with raw data, servers and my arch nemesis - disk usage! When you work with mass spec data and further downstream analysis, disks can get filled up, albeit slower than say genomic analyses. Being a research lab, we have to retain everything and of course have these backed up.

Recently ran into the issue where our primary backup was close to running out of space, we were at more 80% usage of capacity. We could always expand, but buying something, configuring it takes time and given the pandemic in 2020, everything takes longer! I figured since these were backups, I could compress the data and come up with space quickly. 

The fun started when I a run the script below and got corrupt tar.gz files.
See if you can figure out why?

```
#!/bin/sh
#compress-dirs.sh - A script to iterate thru
# list of items in dir,
# checks if it's a directory, get the name
# and creates a tar.gz (tar ball compressed with gzip)
# Also prints out messages of which directory it found,
# and which one is compressing.

for each_dir in *;do
echo "$each_dir"
  if [ -d "$each_dir" ];then 
  tgz_name="$each_dir"-sh.tar.gz
  echo "Dir is $each, compressing to $tgz_name"
  tar -cvzf $tgz_name $each_dir
  fi 
done
```

Turns out, running the script `#./compress-dirs.sh` at the command line
will result in a corrupt tar.gz file!! Got to run and redirect stdout and std error `#./compress-dirs.sh >compress-dirs.out &2>1` to get non-corrupt tar file.


A further, good practice would be adding a check to make sure the resulting
tar archive is not corrupt, we can use `tar -t` for that and check the return status of the command by checking value of `$?`, which holds the return value of the last command run. In *nix systems, a return value of 0 means no error.
`gunzip -c $tgz_name| tar -t >/dev/null and check $? == 0` and if it passes, 
can safely delete the original files.

