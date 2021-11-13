### Systen and Netowrk Information Gathering (Linux) ###

#### Network Routes ####
Is the compromised machine routed to other networks?
```route -n```

#### DNS Server ####
Obtain info about AD Accounts and Zone Transfers
```cat /etc/resolve.conf```

#### Arp Cache ####
Obtain information on how our target can talk to other machines
```arp -a```

#### Current Network Connections ####
Find connections to and from other machines
```netstat -peanut```

### User Information ###

#### Current User Perms ####
```find / -user username```

#### Last Logged on users ####
```last -a```

#### Check Home Directories ####
```ls -la /home/*```

#### Check Service Accounts ####
``` cat /etc/passwd ```

### Priv Esc (Linux) ###

#### See what current user can execute with sudo ####
``` sudo -l```

#### Find SUID Binaires ####
``` find / -perm -4000 -type f 2>/dev/null ```


