# Right Time
## Challenge
- Forensics
- Beginner
- DEADFACE CTF

### Description
>TGRI employee Garry Sartoris fell for a phishing attack recently. It’s hard to say what DEADFACE was after, but Turbo Tactical needs your help looking through the attack artifacts. Take a look at this memory dump and submit the system time on which the memory was captured.
>
>Submit the flag as flag{YYYY-MM-DD hh:mm:ss+00:00}.

### Objective
Obtain the time on which the memory was captured.

## Analysis
When we unzip the downloades data que obtain the file `physmem.raw`. 
We can open this file with https://github.com/volatilityfoundation/volatility3 to analize.

## Solution
Running the command `python3 vol.py -f /home/dati/Downloads/physmem.raw windows.info` will give the info of the `.raw` file, in which we can see the time.
![image](https://github.com/user-attachments/assets/b7296b43-2b8b-43b7-b46d-4b8488bb2b9e)

`flag{2024-10-06 23:38:00+00:00}`
