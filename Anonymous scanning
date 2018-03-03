https://miloserdov.org/?p=131
--------------------------------------------------------------torsocks-----------------------------------------------------
--
 How to install and run Tor in Kali Linux

Install necessary packages:
	
- sudo apt-get install torsocks tor

Then open the /etc/tor/torrc file:
	
- sudo gedit /etc/tor/torrc

And append lines:
	
AutomapHostsOnResolve  1
DNSPort                53530
TransPort              9040

Start and add tor service in autorun:

- sudo systemctl start tor
- sudo systemctl enable tor

-----------------------------------------------------------ProxyChains-NG---------------------------------------------------

 Installing ProxyChains-NG: 
 
sudo apt-get install git gcc
sudo apt-get remove proxychains
git clone https://github.com/rofl0r/proxychains-ng.git
cd proxychains-ng/
./configure --prefix=/usr --sysconfdir=/etc
make
sudo make install
sudo make install-config

-----------------------------------------------------------Sqlmap---------------------------------------------------

Sqlmap anonymous scanning through Tor, Sqlmap has the –proxy option, 
therefore you just need to append –proxy socks5://127.0.0.1:9050 to you command:
	
sqlmap -u TARGET --proxy socks5://127.0.0.1:9050

-----------------------------------------------------------WPScan---------------------------------------------------

WPScan anonymous scanning through Tor WPScan has the similar –proxy flag, 
so just append –proxy socks5://127.0.0.1:9050 to you normal command:
	
wpscan -u TARGET -e p,vt,u --proxy socks5://127.0.0.1:9050

I recommend you consider usage of –request-timeout 500 –connect-timeout 120 options, because scanning through Tor leads to significant delays.

Example:
sudo wpscan -u spryt.ru -e p,vt,u --proxy socks5://127.0.0.1:9050

-----------------------------------------------------------NMAP---------------------------------------------------

proxychains4 nmap -sT -PN -sV --open -n -p 80 mi-al.ru 2>/dev/null
 
Starting Nmap 7.40 ( https://nmap.org ) at 2017-03-18 04:21 EDT
Nmap scan report for mi-al.ru (224.0.0.1)
Host is up (0.15s latency).
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.10.2
 
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.41 seconds
