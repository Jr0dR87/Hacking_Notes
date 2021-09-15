## msfvenom

### Staged and Stageless Payloads

Staged Payloads require a handler to catch the payload and send the appropriate reponse back to the server.

Stageless Payloads do not require a handler and any utility like netcat or socat can be used.

Metasploit has a naming convention for to know if it's a staged payload or stageless.
Three slashes in the name mean it's a staged payload. Example windows/x64/shell/reverse_tcp
Two slashes in the name mean it's a stageless payload. Example windows/x64/shell_reverse_tcp

List Payloads
```
msfveom -l payload
```

