## wpscan cheatsheet

**wpscan vulnerable themes**
'wpscan --url http://10.10.110.100:65000/wordpress/ --enumerate vt --api-token {API TOKEN}'

**wpscan enumerate users**
'wpscan --url http://blog.inlanefreight.local --enumerate u --api-token {API TOKEN}'

**wpscan enumerate plugins**
'wpscan --url http://blog.inlanefreight.local --enumerate p --api-token {API TOKEN}'

**wpscan enumerate Themes**
'wpscan --url http://blog.inlanefreight.local --enumerate t --api-token {API TOKEN}'
