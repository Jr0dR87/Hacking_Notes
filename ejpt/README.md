eJPT Guide
==========

[September 13, 2021](https://jarrodrizor.com/ejpt-guide/ "11:02 am") [Jarrod](https://jarrodrizor.com/author/jarrod/ "View all posts by Jarrod")

Notes on eJPT
=============

I recently passed the eJPT and I have been receiving several questions on how I passed the exam. I am writing out this guide in hopes that it will provide solid and detailed information (without giving out answers) to help others pass. This is not meant to be a replacement for the INE Penetration Testing Student material.

I found the eJPT to be a fun and excellent way to solidify my fundamentals in Pentesting. It took me around 8 hours to complete. I spent a good deal of time brushing through details and taking my time with a few breaks in between such as a meal and a mile run to clear my head.

Routing
-------

Knowing how to setup routing tables is pretty important. When I was getting ready to take the eJPT I heard a few horror stories about students failing because routing on the exam didn't make sense to them. I'm here to tell you it's not that bad, as long as your understand how adding a route works and how to read data to understand what is routing to where.

With the information given from various sources in the exam, map out what parts of the network you have. What communicates to what and routes to what. I took some time and mapped out the source / destination IP addresses based off what was given to me and updated my routing tables.

Display your routing table.

```
route
```

Add route to routing table.

```
ip route add 192.168.222.52 via 10.175.30.1
```

Enumeration
-----------

I can't stress how important it is to take your time and before you start hacking your little hearts out to enumerate, enumerate, and do more enumeration. Do you know the OS? The version of Apache you might be attacking? Do you know if the version of SMB has a CVE? Do you know the CEO of the website your are attacking? Research! Take Notes!

### Nmap

Nmap is used to scan networks and servers for open hosts and ports.

This is how we can test for servers that are up on the network.

```
nmap -sn 10.10.226.0/24
```

This is how we can scan a server for open ports, find what version of software is running and what OS it is and saving it to a file named nmap_scan.txt.

```
nmap -sV -O -oN nmap_scan.txt 10.10.226.53
```

This is how we can scan a server for open ports, find what version of software is running, scan all ports (not just the common ports, so this will take more time) and treat all host as online (useful if scan is being blocked by firewall).

```
nmap -sV -p- -Pn 10.10.226.53
```

### Banner Grabbing

This is how we can use nc to do banner grabbing.

```
echo " " | nc -v 10.10.226.5 80
```

If we need to do Banner Grabbing with HTTPS we can do.

```
openssl s_client -connect 10.10.226.5:443
HEAD / HTTP/1.1
```

### Directory Scanning

As stated earlier, we want to learn as much as we can from a target. This is active scanning since we are interacting directly with the target.

This will perform a Gobuster scan of a website to find hidden files and directories. The -u is for url, -w is the word list to use to scan to see if it can find a match, -o is the output file we want to save. You can add -x and then extensions to help search. I like adding .txt and .bak in my searches because they can often lead to old back up files or txt files with sensitive info.

### Gobuster

```
gobuster dir -u http://10.10.226.146/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -o gobuster_scan.txt
```

Web Hacking
-----------

### ZAP

Zap is one of my favorite tools. I love automating and speeding up my workflow and ZAP is a great way to get excellent information on web site vulnerabilities. A simple and effective way to learn about a website is to open up ZAP, paste a URL and hit attack. (Note* becareful with ZAP as it can go out of scope to scan other things and it can perform actions that might have negatives affects on a site. It's okay for eJPT, but know it can do something harmful to a target).

[![alt ZAP Scan](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/zap_scan.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/zap_scan.png)

You can find XSS, IDORs (Insecure Direct Object References), dated libraries / frameworks and so much more. I will say I used ZAP on this exam and it helped me multiple times. I highly recommend bringing it in your tool box come exam time.

### XSS

The important part of XSS is knowing the context of where the output of your input is going. For example, if we can put `<h2>TEST</h2>` in the input field and the output displays the TEST in an h2 tag, then we will most likely be able to perform XSS exploits.

Inside of input fields, the following command will help find XSS by creating a simple alert.

```
<script>alert(1)</script>
```

If we get an alert with 1, then we have XSS. This would be reflected XSS.

Another example would be adding the payload in the URL. This example is exploiting a vulnerable URL parameter and alerting the users cookie.

```
http://10.10.226.56/vulnerabilities/xss_r/?name=<script>alert(document.cookie)</script>
```

If we perform the command and it gets stored in a database or file on the machine and every time we visit the page and get the alert, this would be store XSS.

If we manipulate the HTML/JavaScript in the source code to perform XSS, this would be DOM XSS.

I found PortSwigger has a great cheat sheet. [PortSwigger Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet).

ZAP is a great tool that can also automate finding XSS for you. It's worth the time to open it up and do a scan with it to see if you can get easy wins. Here in the example, we can see ZAP found a reflected XSS.

[![alt xss](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/zap_xss.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/zap_xss.png)

### SQLi

SQL Injection is one of my favorite vulnerabilities to exploit. Trying to determine the syntax of the SQL in the background is going to be the key in helping exploit the SQL on the backend server. Looking at a simple example `SELECT * FROM users WHERE id = 1` this should pull all information from the users table that belongs to a user with an id of 1.

A lot of SQLi, XSS, SSRF, and XXE is playing around with the application to see how it will respond. It's a game of patience and drawing out how something should be interpreted on the back-end server. My advice is to not just machine gun payloads into inputs for quick wins. Take your time and use cheat sheets to help learn what DB you are attacking, what the query could look like, number of columns in a table, and so on.

A very simple SQLi payload is to simply add a ' in an input field to see if a we can get feedback about breaking the SQL statement.

Sometimes I fill in a value and then close the statement with a 1' or 1=1-- to put a breaking point in the SQL statement and add a 1=1 (which is always true) to see if this will put out data from the database. This would look like the screenshot below.

[![alt sqli](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqli.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqli.png)

Again, a lot of these input injection attacks involve playing with the values you are putting in and mapping out what affects are happening.

Burp Suite is king for helping with this. If you know how to use Burp Suite Repeater, it can be a huge help with this.

#### Sqlmap

Sqlmap is an awesome tool. It does do what I mentioned not to do in the previous section and that is it machine gun payloads into a parameter to find a SQLi vulnerability. It does this much faster and more effective than you though. That's the positive side of automating these attacks. The downside is it's very loud (and can be very dangerous on pentest). This tool is great for eJPT, but just know in the real world it can be dangerous and also very disruptive during a pentest. Don't use it unless you have written authorized permission to do pentesting. Do not use on a production database!

My personal preference when it comes to using Sqlmap is using Burp Suite to save out a request to the server and use that file as a parameter in Sqlmap. I find it to be cleaner and I have more success than `sqlmap -u "http://vulnsitetosqli.com/?id=243"` for example, but again this is just my approach.

Here I'll get the request I sent to the website and save it from Burp Suite by right clicking and saving item. I'll save it to my workstation.

[![alt request](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/saved_request.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/saved_request.png)

Here I'm using the file that holds the requests I saved from Burp Suite and I'm using the -r to read the file contents and I'm trying to get the databases.

This is how the command looks to read the file and get the databases if SQLi is possible.

```
sqlmap -r sql_map_file -dbs
```

This is my output.\
[![alt DB](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqlmap_databases.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqlmap_databases.png)

This is how the command looks to get the tables from the database owasp10.

```
sqlmap -r sql_map_file -D owasp10 --tables
```

This is how the command looks to query the table accounts in the database owasp10 and dump out the data.

```
sqlmap -r sql_map_file -D owasp10 -T accounts --dump
```

This is the output.\
[![alt dump](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqlmap_account_dump.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/sqlmap_account_dump.png)

SMB Hacking
-----------

SMB allows file sharing, network browsing, and printing over a network. Usually we will be going for network file shares. What I think of when I see these shares on machines is we can either get in using anonymous login or we need to find credentials to get in, but most of the time they have something useful and for the most part are not rabbit holes.

### SMBclient

This will list out shares.

```
smbclient -L \\\\10.10.226.54\

```

This will attempt to connect to the share as an anonymous user. If you know of a user and password, swap out Anonymous with a user and after you hit enter you will be prompt for a password that you can input. If you are going the anonymous route then just hit enter when you get the prompt to see if the share was setup to allow connections with no passwords.

```
smbclient \\\\10.10.226.54\\profiles -U Anonymous
```

### Enum4Linux

Enum4Linux is also a great tool to do SMB enumeration.

This will scan the system and should get the domain, usernames, OS, shares, and other bits of information.

```
enum4linux -a 10.10.226.50
```

Password Cracking
-----------------

Password cracking can play a large part in compromising a machine. If you are able to take advantage of poorly managed user accounts on a Linux system and get the contents of the /etc/passwd file AND /etc/shadow file, then you could attempt to crack the hashes found in /etc/shadow. Note that /etc/shadow is only supposed to be read only by root.

### Unshadow

Unshadow will combine /etc/passwd users with /etc/shadow hashes to form one file to crack.

```
unshadow /etc/passwd /etc/shadow > hashes
```

### John the Ripper

John the Ripper is a great password cracker and can help us find passwords to user accounts on systems. Here we are using john and trying to crack an MD5 hash using --format=Raw-MD5 and trying to crack it with the passwords in rockyou.txt. The file simple_hash is the file containing our hash to crack.

```
john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt simple_hash
```

SSH Hacking
-----------

When I see SSH open I always assume we need to get into the server via a means of finding credentials. For the most part we aren't going to search out an online exploit that auto SSH us in without credentials. What I mean is I don't tend to spend much time looking for online exploits for it. I will, however, use brute force attacks against it or ssh in with credentials I find in other areas such as a file share.

### Hydra

Using Hyrda we can brute force servers to see if we can find a user with a weak password and compromise the server.

Here is a simple Hyrda command that is going brute-force the jim user account using the password list rockyou.txt to see if it can find a password that will let us ssh in as root. You can add a -v for verbose output.

```
hydra -l jim -P rockyou.txt ssh://10.10.226.55
```

Metasploit
----------

I'll do my best with the next two topics as they are guides in their own right. If you need more practice TryHackMe has wonderful rooms on Metasploit. Here is one and it's free with a walk through! [TryHackMe Metasploit](https://tryhackme.com/room/rpmetasploit).

To me Metasploit has always been like Game Genie. The old device that allowed you to cheat in Nintendo games as a kid by inputting various values without having to know how the values were changing the code. Metasploit is similar. If you know the values of an exploit, and can fill them in, then you can hack something. It will do the hack for you.

A detailed walk through of me compromising a server using Metasploit can be found on my [blog](https://jarrodrizor.com/bolt-writeup/).

Here are some quick simple commands:

-   To start Metasploit with no banner, run `msfconsole -q`
-   To search, use `search {name of thing I want to exploit}`
-   To select an module from the list, run `use {the number in the list}`
-   To see what values I need to set, run `options`
-   To set an option run `set {name} {value}` Example would be `set RHOSTS 10.10.10.50`
-   To begin the exploit, run `exploit`

[![alt exploit](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_metasploit_exploit.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_metasploit_exploit.png)\
[![alt options](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_metasploit_options.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_metasploit_options.png)\
[![alt root](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_Becoming_Root-1.png)](https://github.com/JarrodRizor/Hacking_Notes/raw/main/ejpt/screenshots/Bolt_Becoming_Root-1.png)

Meterpreter
-----------

Meterpreter is a payload that will give us a shell with commands we can use on a given system and the payload lives in memory. It works by executing a reverse shell on a target machine and connecting back to us. I like to think of it as a more elegant nc connection.

To list Meterpreter payloads, run

```
search meterpreter
```

The following two examples are for simple examples for Linux and Windows. You will need to pick the right payload for the correct architecture of the target system.

To set a meterpreter payload for a windows target, run

```
set payload windows/meterpreter/reverse_tcp
```

To set a meterpreter payload for a linux (64 bit) target, run

```
payload/linux/x64/meterpreter/reverse_tcp
```

To background the meterpreter session, run

```
background
```

To get back your meterpreter session run

```
session -i {session number}
```

When you need to see more options in your meterpreter session, run

```
help
```

### Final Thoughts

I hope you found this information useful. If you have any questions, you can always reach out to me on [Twitter](https://twitter.com/Jrod_R87) and I will do my best to help you out.

My final bit of advice is the following.

-   Make sure you are comfortable with the Penetration Testing Prerequisites lab Find the Secret Server. I can't stress enough how important it is you know routing and how to set it up.
-   If you can complete the Penetration Testing Basics labs Black-box Penetration Test 1,2,3 with little to no issues and you understand it, I would say that's a good indicator you are ready for the exam. I found these to be a bit harder than the exam itself. This is again my opinion.
-   Make sure you map out the network, take solid notes, and take breaks when you need them. You have three days to do this. Use as much time as you need and access outside resources.
-   Have sources for cheat sheets. I love cheat sheets because you can't be expected to remember every way to perform an exploit. XSS has numerous ways to be exploited for example. It's not cheating yourself or the exam if you need to use them.
-   You got this!
