# Deep Pockets
## Challenge 
- Forensics/ Traffic Analysis
- DEADFACE
- Medium

### Description
>Note: Use the PCAP from Big Fish.
>
>Garry Sartoris had a CSV file of VIP clients on his machine that DEADFACE exfiltrated. TGRI wants us to confirm William Harrington's account number to determine if his information was compromised. Submit William Harrington's account number as the flag.
>
>Submit the flag as flag{AAAA##############}.

### Objective
Find the VIP file and find the account number of William Harrington.

## Analysis
First we need to find a packet that has a `.csv` file, as said in the description.
![image](https://github.com/user-attachments/assets/61fbaf70-9896-4c3f-8285-fa5360498367)
We get this 5 tcp packets. If we click on follow tcp stream which allows to analyze the entire conversation of a TCP connection.
![image](https://github.com/user-attachments/assets/d2ab50e2-7e8c-47fe-a5a0-32bda429d4e7)
![image](https://github.com/user-attachments/assets/5ef65663-263d-413e-8ef1-4ab9aff4737b)
![image](https://github.com/user-attachments/assets/70ed8199-61d6-4ce0-bef8-ae9c8588834c)
In this TCP conversation we see that the file `vip-clients.csv` was moved to a `.zip` file `collections.zip`.
Reading the consersation we found:
```
C:\Users\garry>
.\945f 45.55.201.188 7523 < collection.zip
```
So we know in wich port was the `collection.zip` send.

## Theory
You can find the file information in TCP (Transmission Control Protocol) because TCP is a transmission protocol that sends data in packets, and that data can include any type of file or information that is being transferred between two devices on a network.

TCP divides information into packets, which are then sent from the sender to the recipient. Each packet contains a piece of the file's data.

In a file transfer, all packets belonging to that transfer travel on a specific connection between the IP and port of the sender and the receiver.

When you send a file over TCP, the contents of the file are sent in either binary or text format, depending on the file type.

TCP ensures complete and ordered delivery of the data, meaning that each piece of the file will be sent and reassembled at the destination.

## How to avoid
To avoid exfiltration of sensitive data:
- Network Traffic Monitoring and Intrusion Detection:
  - Set up alerts for sensitive file access and transmission patterns, particularly when data is transmitted to untrusted or unusual destinations. 
- Data Loss Prevention (DLP)
  - DLP policies can be set to restrict the transmission of specific file types or patterns (e.g., .csv files containing sensitive data).
- Access Controls and Authentication
- File Encryption

## Solution
1. Find packets with the file: `frame conatins ".csv"`
![image](https://github.com/user-attachments/assets/61fbaf70-9896-4c3f-8285-fa5360498367)
2. Click on one packet and the follor TCP stream
3. See that de file `vip-clients.csv` was compresed in `colletion.zip`
![image](https://github.com/user-attachments/assets/5ef65663-263d-413e-8ef1-4ab9aff4737b)
4. Here you can see the collection.zip got sent over on port 7523
![image](https://github.com/user-attachments/assets/70ed8199-61d6-4ce0-bef8-ae9c8588834c)
5. Filter the `pcap` with `tcp.port == 7523`, so we get only specific packets
![image](https://github.com/user-attachments/assets/c3b28dbe-054d-4a24-ad89-c1f5a5dc5653)
6. Click on one packet and the follor TCP stream
7. Change it to raw and then save as
![image](https://github.com/user-attachments/assets/1418000a-c287-4a65-bdd3-bc2008c93e38)
8. Save the file as `[name].raw`
9. unzip the file
10. Open `vip-clients.csv`
  ![image](https://github.com/user-attachments/assets/9bf72b76-9282-41eb-9215-6d08b83e0cc5)

`flag{RRNQ85158591854615}`
