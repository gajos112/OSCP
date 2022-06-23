# 1. LDAP

**1. LDAPSEARCH**

ldaspsearch -x -h 10.10.10.107

**2. NMAP**

locate -r nse$ | grep -i ldap

/usr/share/nmap/scripts/ldap-brute.nse  
/usr/share/nmap/scripts/ldap-novell-getpass.nse  
/usr/share/nmap/scripts/ldap-rootdse.nse  
/usr/share/nmap/scripts/ldap-search.nse  

nmap --script ldap-search 10.10.010.107

map --script ldap-rootdse -p 389 10.10.10.107  
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-23 05:54 EDT  
Nmap scan report for 10.10.10.107  
Host is up (0.040s latency).  
  
PORT    STATE SERVICE  
389/tcp open  ldap  
| ldap-rootdse:  
| LDAP Results  
|   <ROOT>  
|       supportedLDAPVersion: 3  
|       namingContexts: dc=hackthebox,dc=htb  
|       supportedExtension: 1.3.6.1.4.1.1466.20037  
|_      subschemaSubentry: cn=schema  
  
Nmap done: 1 IP address (1 host up) scanned in 0.49 seconds  
