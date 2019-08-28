---
layout: default
title: customizing_shell_prompt
parent: linux
nav_order: 4
---

### Shell prompt customization

- \d Date in "Weekday Month Date" format "Tue May 26"
- \h Hostname up to the first period
- \H Hostname
- \n Newline
- \t Current time in 24-hour HH:MM:SS format
- \T Current time in 12-hour HH:MM:SS format

- \@ Current time in 12-hour am/pm format
- \A Current time in 24-hour HH:MM format
- \u Username of the current user
- \w Current working directory
- \W Basename of the current working directory
- \$ if the effective UID is 0, a #, otherwise a $

``` bash
$ echo 'export PS1="[\u@\h \w]\$ "' >> ~/.bash_profile
setting step:
echo $PS1
[\u@\h \W]
PS1="\u@\h \$"
PS1 = "<\t \u@\h \w>\$"
<09:23:32 Jerry@linux_user ~>$
```

