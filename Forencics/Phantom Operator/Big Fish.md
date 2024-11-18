# Bad Processes
## Challenge
- Forensics / Traffic Analysis
- DEADFACE CTF

### Description
>TGRI employee Garry Sartoris fell for a phishing attack recently. It’s hard to say what DEADFACE was after, but Turbo Tactical needs your help looking through the attack artifacts. Take a look at this PCAP and submit the attacker’s IP address.
>
>Submit the flag as flag{IP Address}.


### Objective
Find the IP address od the attacker.

## Analysis
If Garry fell into a phishing attack, chances are it was through http.
![image](https://github.com/user-attachments/assets/c0695c32-5c12-4f0c-9edb-c284bc0b0355)

## Solution
Filter the PCAP fiel with http.
![image](https://github.com/user-attachments/assets/5fc435d5-b88f-4a44-ab05-45484579f4d4)
From this we can figure out that teh IP address is 45.55.201.188

`flag{45.55.201.188}`
