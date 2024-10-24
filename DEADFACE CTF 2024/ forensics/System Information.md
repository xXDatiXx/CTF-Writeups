# System Information
## Challenge
- Forensics
- DEADFACE CTF

### Description
>Note: Use the memory dump file from Right Time.
>
>The malware that was ran on Garry’s machine gathered system information. We suspect that this information was saved on the system somewhere. Provide the full filepath to the file where DEADFACE saved system information.
>
>Submit the flag as flag{path\to\file}. Example: flag{C:\Windows\System32\file.txt}

### Objective
The objective was to find the full filepath to a document where DEADFACE saved system information.
## Analysis
Clone the repo `https://github.com/volatilityfoundation/volatility3`.

With the comand `python3 vol.py -f /physmem.raw windows.filescan` we can scan the memory dump file to find recently opened or closed files.
But since the output is too large I kept it in a txt to analyze later. This also benefits me in being able to search for information in the command output.
`python3 vol.py -f /physmem.raw windows.filescan > salid.txt`

In the description we can see that they use .txt. So I decided to search for .txt files:
![image](https://github.com/user-attachments/assets/78dcb742-d209-4c38-b799-13feb196cbf7)

## Theory
`windows.filescan` Scans for file objects present in a particular windows memory image.

## Solution
- Run `python3 vol.py -f /physmem.raw windows.filescan > salid.txt`
- Search for .txt files.
- `flag{C:\Windows\Temp\sysinfo.txt}`
