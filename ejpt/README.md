## Notes on eJPT

I recently passed the eJPT. I found it to be a fun and excellent way to solidify my fundentmentals in Pentesting.

### Routing

### Enumeration

#### Nmap

Nmap is used to scan networks and servers for open hosts and ports.

This is how we can test for servers that are up on the network
```
nmap -sn 10.10.10.0/24
```
This is how we can scan a server for open ports, find what version of software is running and what OS it is and saving it to a file named nmap_scan.txt.
```
nmap -sV -O -oN nmap_scan.txt 10.10.10.X
```

This is how we can scan a server for open ports, find what version of software is running, scan all ports (not just the common ports, so this will take more time) and will treat all host as online (useful if scan is being blocked by firewall)
```
nmap -sV -p- -Pn 10.10.10.x
```

### Web Hacking

#### ZAP

#### XSS
The important part of XSS is knowing the context of where the output of the input is going. For example, if we see an input field that ask to list unformation with a username 

#### SQLi

##### SQLmap

#### Directory Scanning 

##### Gobuster

### SMB Hacking

##### SMCclient

#### Password Cracking

##### Unshadow

##### John the Ripper

##### SSH Hacking 

##### Metasploit

##### Meterpreter

##### Pivoting with Meterpreter 
