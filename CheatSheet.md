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

