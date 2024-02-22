## Possible files to fingerprint version. ##
/administrator/manifests/files/joomla.xml
/README.txt

## Joomla Scan Commands ##
python2.7 joomlascan.py -u http://dev.inlanefreight.local

## Joomla Brute Password Brute Force (Admin) ##
python3 joomla-brute.py -u http://dev.inlanefreight.local/ -w /usr/share/metasploit-framework/data/wordlists/http_default_users.txt -usr admin
