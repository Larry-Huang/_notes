---
layout: default
title: linux_basic_command
parent: Linux
nav_order: 2
---

### ls options
- -a List all files, including hidden files.
- --color List files with colorized output.
- -d List directory names, not contents.
- -l Use the long listing format.
- -r Reverse the order.
- -R List files recursively.
- -t Sort by time, most recent first.


### Permission Explained

|options|U  |G  |O  |
|:------|:--|:--|:--|
|Symbolic|rwx|r-x|r--|
|Binary|111|101|100|
|Decimal|7|5|4|


### The find and locate Command
```find [path…] [expression]```

- -name pattern Find files and directories
that match pattern.
- -iname pattern Like -name, but ignores
case.
- -ls Performs an ls on each of
the found items
- -mtime days Finds files that are days old.
- -size num Finds file that are of size
num.
- -newer file Finds files that are newer
than file.
