---
layout: default
title: file_manipulate
parent: linux
nav_order: 3
---

### Removing Files
- rm file Remove file.
- rm -r dir Remove the directory and its contents recursively.
- rm -f file Force removal and never prompt for confirmation.

### copy
- cp source_file destination_file Copy source_file to destination_file.

- cp src_file1 [src_fileN ...] dest_dir Copy source_files to destination_directory.

### Moving and Renaming Files
- mv source destination
- mv -i source destination

### Creating a Collection of Files

- tar [-] c|x|t f tarfile [pattern]
>Create, extract or list contents of a tar archive
using pattern, if supplied.

- tar Options
  
|options||
|--|--|
|c |Create a tar archive.|
|x | Extract files from the archive.|
|t |Display the table of contents (list).|
|v |Be verbose.|
|z |Use compression.|
|f |file Use this file.|

### Disk Usage

- du Estimates file usage.
- du -k Display sizes in Kilobytes.
- du -h Display sizes in human readable format.

### Comparing Files
- diff file1 file2 Compare two files.
- sdiff file1 file2 Side-by-side comparison.
- vimdiff file1 file2 Highlight differences in vim.
