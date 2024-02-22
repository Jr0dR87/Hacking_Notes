## wpscan cheatsheet

**wpscan Vulnerable Themes**
`wpscan --url http://10.10.110.100:65000/wordpress/ --enumerate vt --api-token {API TOKEN}`

**wpscan Enumerate Users**
`wpscan --url http://blog.inlanefreight.local --enumerate u --api-token {API TOKEN}`

**wpscan Enumerate Plugins**
`wpscan --url http://blog.inlanefreight.local --enumerate p --api-token {API TOKEN}`

**wpscan Enumerate Themes**
`wpscan --url http://blog.inlanefreight.local --enumerate t --api-token {API TOKEN}`

**wpscan Brute Force User with Rockyou.txt Password List**
wpscan --url blog.inlanefreight.local --usernames doug --passwords /usr/share/wordlists/rockyou.txt --api-token `{API TOKEN}`
