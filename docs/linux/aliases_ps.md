---
layout: default
title: aliases_ps_jobcontrol
parent: linux
nav_order: 6
---


### Aliases
- Fix Typos
``` bash
$> alias grpe='grep'
```

- Make Linux behave like another OS.
``` bash
$> alias cls='clear'
```

- unalias name Remove the “name” alias.
- unalias -a Remove all aliases
- Add aliases to your personal initialization
``` bash
$> echo "alias ll='ls -a'" >> ~/.bashrc
$> source ~/.bashrc
```

### Listing Processes and Information

#### ps Options
- -e Everything, all processes.
- -f Full format listing.
- -u username Display username’s processes.
- -p pid Display information for PID.

#### Common ps Commands
- ps -e Display all processes.
- ps -ef Display all processes, full.
- ps -eH Display a process tree.
- ps -e --forest Display a process tree.
- ps -u username Display user’s processes.

#### Other ways to view processes
- **pstree** Display processes in a tree format.
- top Interactive process viewer.
- htop Interactive process viewer.

### Background and Foreground Processes
- command & Start command in background.
- Ctrl-c Kill the foreground process.
- Ctrl-z Suspend the foreground process.
- bg [%num] Background a suspended process.
- fg [%num] Foreground a background process.
- kill Kill a process by job number or PID.
- jobs [%num] List jobs.

### Killing Processes
- Ctrl-c Kills the foreground proc.
- kill [-sig] pid Send a signal to a process.
- kill -l Display a list of signals.
- kill 123
- kill -15 123
- kill -TERM 123 kill -9 123