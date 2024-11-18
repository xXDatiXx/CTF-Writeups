# Product ID
## Challenge
- Forensics / Traffic Analysis
- Beginner
- DEADFACE CTF

### Description
>Note: Use the PCAP from Big Fish.
>
>We suspect that Garry Sartoris was using a non-compliant Windows host and not his work-provided laptop from TGRI. Is there any way to confirm this in the data that DEADFACE exfiltrated? Provide the Windows Product ID of Garry Sartoris’s machine.
>
>Submit the flag as flag{Windows Product ID}. Example: flag{12345-67890-12345-67890}.

## Analysis
First I opened de PCAP file on wireshark.
As they asked to find the Product ID of their windows, I decided to search for the ProductName field.
`frame matches "ProductName”`
## Theory
The ProductName appears in these records because the captured traffic likely contains system or network diagnostics, or information being transferred over the network that includes metadata about the operating system or the machine itself.

## Solution
First we see some records that have Windows Pro, that seems that its a work-provided laptop.
![image](https://github.com/user-attachments/assets/ab7b902f-9d19-4b38-9d50-12d433827593)
Then I saw a record with Windows Home, that doesnt semm to be a work-provided laptop. 
This confirms Garry wasn't using a work-provided laptop 
![image](https://github.com/user-attachments/assets/80431f93-c136-401c-a920-b528493810ef)
`flag{00326-10000-00000-AA973}`
