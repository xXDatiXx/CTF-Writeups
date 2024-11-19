# Password
## Challenge
- Forensics / Traffic Analysis
- Beginner
- DEADFACE CTF

### Description
>Note: Use the PCAP from Big Fish.
>
>Garry Sartoris provided his credentials to the fake login page. TGRI wants to ensure that their password policy enforces secure practices. What is Garry’s password?
>
>Submit the flag as flag{password}.

## Analysis
First I opened de PCAP file on wireshark.
In the description they tell us that some credentials were lost and that we can find them. So I first decide to see http traffic that could contain the transmission of credentials on unencrypted forms:
```
http.request.method == "POST"
```
This will filter all HTTP POST packets, which is the common method of submitting login forms. 

## Theory
HTTP (Hypertext Transfer Protocol) and HTTPS (Hypertext Transfer Protocol Secure) are the two primary protocols used for transmitting data over the web. While they serve the same basic function, the key difference between them lies in security.

*HTTP* transmits data in plain text. This means that all the information exchanged between the client (browser) and the server is unencrypted. Any attacker intercepting the communication (via man-in-the-middle attacks, packet sniffing, etc.) can easily read the transmitted data, including sensitive information like usernames, passwords, and credit card numbers.

*HTTPS* transmits data in an encrypted format using SSL/TLS (Secure Socket Layer / Transport Layer Security). This encryption ensures that even if a third party intercepts the communication, the data will appear as gibberish to them, making it unreadable without the proper encryption key.
## Solution
When we filter the PCAP file on wireshark with the command `http.request.method == "POST"` que end up with only 1 event:
![image](https://github.com/user-attachments/assets/9c990e92-9592-4233-a125-b9e975a87cd6)
If the enter the information of the packet and expand the `HTML Form URL Encoded: application/x-www-form-urlencoded` we can see the password.
![image](https://github.com/user-attachments/assets/a1195941-1818-4d19-8148-e3be238c109a)
`flag{S4rt0RIS19&&}`
## How to avoid
Dont send information over http pages.
