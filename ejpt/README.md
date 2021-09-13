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
I can't stress how important it is to take your time and before you start hacking your little hearts out to enumrate, enumerate, and do more enumration. Do you know the OS? The version of Apache you might be attacking? Do you know if the version of SMB has a CVE? Do you know the CEO of the website your are attacking? Research! Take Notes! 

### Nmap

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

## Web Hacking

### ZAP
Zap is one of my favorite tools. I love auotmating and speeding up my workflow and ZAP is a great way to get excellent information on web site vulnerablities. A simple and effective way to learn about a website is to open up ZAP, paste a URL and hit attack.

![alt ZAP Scan](https://github.com/JarrodRizor/Hacking_Notes/blob/main/ejpt/screenshots/zap_scan.png)

You can find XSS Attacks, IDORs (Insecure Direct Object References), Sensitive files, robot.txt files, dated libaries / frameworks and so much more. ZAP is a guide in of itself. I will lsay I used ZAP on this exam and it helped me multiple times. Highly recommend brining it in your tool box come exama time.

### XSS
The important part of XSS is knowing the context of where the output of your input is going. Know where the output is going to display can be a huge part in finding XSS vulnerabilities. For example, if we can put ```<h2>TEST</h2>``` in the input field and the output displays the TEST in an h2 tag, then we will most likely be able to perform XSS exploits.

Inside of input fields, the following command will help find XSS.
```
<script>alert(1)</script>
```
If we get an alert with 1, then we have XSS. This would be reflected XSS.

If we perform the command and it gets stored in a database or file on the machine and every time we visit the page and get the alert, this would be store XSS.

If we manipulate the HTML/JavaScript in the source code to perform XSS, this would be DOM XSS.

I found PortSwigger has a great cheat sheet. [PortSwigger Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) 

ZAP is a great tool that can also automate finding XSS for you. It's worth the time to open it up and do a scan with it to see if you can get easy wins. Here in the example, we can see ZAP found a reflected XSS.

![alt xss](https://github.com/JarrodRizor/Hacking_Notes/blob/main/ejpt/screenshots/zap_xss.png)

### SQLi

Need to finish

#### SQLmap

Need to finish

## Directory Scanning 

As stated earlier, we want to learn as much as we can from a target. This is active scanning since we are interacting directly with the target.

This will perform a gobuster scan of a website to find hidden files and directories. the -u is for url, -w is the wordist to use to scan to see if it can find a match, -o is the output file we want to save. You can add -x and then extenions to help search. I like adding .txt and .bak in my searches because they can offten lead to old back up files or txt files with sensitve info. 
##### Gobuster

```
gobuster dir -u http://192.168.0.146/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -o gobuster_scan.txt
```

## SMB Hacking

Need to finish

### SMCclient

Need to finish

## Password Cracking

Need to finish

### Unshadow

Unshadow will combine /etc/passwd users with /etc/shadow hashes to form one filel to crack.
```
unshadow passwd shadow > unshadowed.txt
```

### John the Ripper

John the Ripper is a great password cracker and can help us find get passwords to user accounts on systems. Here we are using john and trying to crack an MD5 hash using --format=Raw-MD5 and trying to crack it with the passwords in rockyou.txt. The file simple_hash is the file containg our hash to crack. 

```
john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt simple_hash
```

## SSH Hacking

When I see SSH open, I  always assume we need to get into the server via a means of finding credentials. For the most part, we aren't going to search out an online exploit that auto SSH us in without credientals. What I mean is I don't tend to spend much time looking for online exploits for it. I will however use brute force attacks againist it or ssh in with credentials I find in other areas such as a file share for example. 

### Hydra
Using hyrda, we can brute force servers to see if we can find a user with a weak password and compromise the server.

Here is a simple hyrda command that is going brute-force the jim user account using the password list rockyou.txt to see if it can find a password that will let us ssh in as root. You can add a -v for verbose output.
```
hydra -l jim -P rockyou.txt ssh://10.10.10.55 
```

## Metasploit

Need to finish all three

## Meterpreter

## Pivoting with Meterpreter 
