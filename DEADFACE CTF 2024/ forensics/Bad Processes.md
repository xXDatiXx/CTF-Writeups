# Bad Processes
## Challenge
- Forensics
- DEADFACE CTF

### Description
>Note: Use the memory dump file from Right Time.
> 
>What is the PID of the malicious file that Garry ran after falling victim of the phishing scam.
>
>Submit the flag as flag{PID}.

### Objective 
Identify a malicious process that had been carried out after Garry's fall in a phishing campaign. The goal was to find the PID of the malicious file that Garry, the victim, executed after committing through a downloaded file after accessing a phishing link.

## Analysis
Clone the repo `https://github.com/volatilityfoundation/volatility3`. 
With the command `python3 vol.py -f /home/dati/Downloads/physmem.raw windows.cmdline` we can extract information about the processes in execution and their respective arguments.
![image](https://github.com/user-attachments/assets/a5700239-914a-48d5-bce4-e714576aceb7)

## Theory
In volatility the comand `windows.cmdline` allows us to obtain a detailed list of the processes executed and their arguments.
Looking at the results we see the command `8460 winupdate.exe "C:\Users\garry\Downloads\update\winupdate.exe"`, which is extremely strange that after a phishing attack you decide to update your windows. 
When reading the path where `winupdate.exe` was run we see that it was run from the `/Downloads` folder, which obviously would not be a legitimate Windows program 

## Solution
- Run `python3 vol.py -f /home/dati/Downloads/physmem.raw windows.cmdline`
- Obtain the PID of the de malicious process. `8460 winupdate.exe "C:\Users\garry\Downloads\update\winupdate.exe"`
- `flag{8460}`
