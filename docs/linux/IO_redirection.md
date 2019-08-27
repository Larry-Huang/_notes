---
layout: default
title: IO_redirection
parent: linux
nav_order: 5
---

I/O Redirection 
{: .label }

### input/output types
---

|IO Name|file descriptor|
|:------|:--------------|
|stdin  |0|
|stdout |1|
|stderr |2|


### Redirection

- \> Redirects standard output to a file.
Overwrites (truncating) existing contents.
- \>> Redirects standard output to a file.
Appends to any existing contents.
- < Redirects input from a file to a command.
- & Used with redirection to signal that a
file descriptor is being used.
- 2>&1 Combine stderr and standard output.
- 2>file Redirect standard error to a file.
```
ls file.txt not-here 2 > out.err

combine 1 & 2 output
ls file.txt not-here > out.both 2 > &1
```
### The Null Device
- \> /dev/null Redirect output to nowhere.

