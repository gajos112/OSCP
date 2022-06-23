# 1. LDAP

## 1. LDAPSEARCH 

**Command:** ldaspsearch -x -H ldap://10.10.10.107  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/6.png)

**Command:** ldapsearch -x -H ldap://10.10.10.107 -s base 'namingcontexts' 
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/4.png)

**Command:** ldapsearch -x -H ldap://10.10.10.107 -s sub -b 'dc=hackthebox,dc=htb'  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/5.png)


## 2. NMAP

**Command:** locate -r nse& | grep -i ldap  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/1.png)

**Command:** nmap --script ldap-search -p 389 10.10.10.107  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/2.png)

**Command:** nmap --script ldap-rootdse -p 389 10.10.10.107  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/3.png)
