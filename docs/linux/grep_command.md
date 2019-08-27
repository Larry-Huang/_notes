---
layout: default
title: grep_command
parent: linux
nav_order: 6
---

### Grep Command
- grep Display lines matching a pattern.
- grep pattern file

```
-i Perform a search, ignoring case.
-c Count the number of occurrences in a file.
-n Precede output with line numbers.
-v Invert Match. Print lines that don’t match.
```

### The file Command
file file_name Display the file type
```
$ file sales.data
sales.data: ASCII text
$ file *
bin: directory
jason.tar: POSIX tar archive
```
### Searching for Text in Binary Files
strings Display printable strings.

### Pipes
- \| Pipe symbol
- command-output | command-input

``` shell
grep pattern file
cat file | grep pattern
```

### The cut Command
-cut [file] 
>Cut out selected portions
of file. If file is omitted,
use standard input.
```
-d delimiter Use delimiter as the field
separator.
-f N Display the Nth field.
```
### sample code 
```
grep bob /etc/passwd | cut -d: -f 1,5 | sort | tr ":" " " | column -t
```


