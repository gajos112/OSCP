# 1. LDAP

**1. LDAPSEARCH** 

**Command:** ldaspsearch -x -h 10.10.10.107  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/2.png)

**2. NMAP**   

**Command:** locate -r nse& | grep -i ldap  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/1.png)

**Command:** nmap --script ldap-rootdse -p 389 10.10.10.107  
![alt text](https://raw.githubusercontent.com/gajos112/OSCP/master/images/3.png)
