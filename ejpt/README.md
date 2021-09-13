# Notes on eJPT

I recently passed the eJPT and I have been receiving serverl questions on how I passed the exam. I am writing out this guide in hopes it will provide solid and detailed information (without giving out my answers) to help others pass. 

I found the eJPT to be a fun and excellent way to solidify my fundentmentals in Pentesting. It took me around 8 hours to complete. I spent a good deal of time brushing through details and taking my time with a few breaks in between such as a meal and a mile run to clear my head. 

I would say the bulk of my time was understanding the network around me and enumrating it and setting it up.

## Routing
Setting up routing and routing tables was one of the first things I did once I began the eJPT. When I was getting ready to take the eJPT, I heard a few horror stories about students failing out right because routing on the exam didn't make sense to them. I'm here to tell you it's not that bad as long as your understand how adding a route works and how to read data to understand what is routing to where. Think what is the source to destination.

Here is some useful infomation on how to get started on the exam. With the information given from various sources in the exam, map out what parts of the network you have. What communicates to what and routes to what (ie routing). What client is talking to what server? I took some time and mapped out the souces / destination IP addresses and after I felt I had a good handle of the network, I set up my routing used the commands.

Display your routing table
```
route
```

Add route to routing table
```
ip route add 192.168.100.0/24 via 10.175.30.1 
```

## Enumeration
I can't stress how important it is to 

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
The important part of XSS is knowing the context of where the output of your input is going. Know where the output is going to display can be a huge part in finding XSS vulnerabilities. For example, if we can put <h2>TEST</h2> in the input field and the output displays the TEST in an h2 tag, then we will most likely be able to perform XSS exploits. We sometimes might need to get fancy because old faithful as I call it <script>alert(1)</script> could be filtered. This is where I turn to cheatsheets. 

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
