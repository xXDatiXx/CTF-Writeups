# Suspicious Sender
## Challenge
- Forensics / Traffic Analysis
- DEADFACE CTF

### Description
>Note: Use the PCAP from Big Fish.
>
>Which IT member did DEADFACE pose as in the email in order to trick Garry Sartoris?
>
>Submit the flag as flag{firstname lastname}. Example: flag{John Smith}.

### Objective
Find out who the attacker was posing as.

## Analysis
If Garry fell into a phishing attack, chances are it was through http.
Seeing the content of the packages that come out after filtering we can see in one of them what appears to be an email.
![image](https://github.com/user-attachments/assets/240d58b2-03fc-4d31-ac2e-e1fd3186ea81)

## Solution
Filter the PCAP fiel with http.
Seeing the content of the packages that come out after filtering we can see in one of them what appears to be an email.
Serching in this one we find the name of William Hadder.
![image](https://github.com/user-attachments/assets/240d58b2-03fc-4d31-ac2e-e1fd3186ea81)

`flag{William Hadder}`
