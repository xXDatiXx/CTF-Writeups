# In Plain Sight
## Challenge
- Forencics
- 1337UP LIVE 2024
### Description
Barely hidden tbh..

![meow](https://github.com/user-attachments/assets/d5a87ac2-a767-40c0-b51b-23169da5bb19)


### Objective
Obtain the flag hidden in the image.

## Analysis
Using the command `strings meow.jpg` we obtain all readable strings in the file. 
![image](https://github.com/user-attachments/assets/295c6d8f-8332-402a-876d-0e1b118cc236)
In here we can see a very uncommon string `YoullNeverGetThis719482` and `flag.pngUT` this second tells us that there is a hidden png image.

I used `binwalk meow.jpg` we see that a zip file with a png file was hidden. 

![image](https://github.com/user-attachments/assets/07da9269-5700-48b2-9f14-fb47d66b0531)

Then we use `binwalk -e meow.jpg` for extracting this data. It extracts a folder with a file `flag.png` and `20BA6E.zip`.
When extracting the information od the zip file we get another `flag.png` that is only a white image.

Using this last png file in https://georgeom.net/StegOnline/image and selecting LSB Half we get the flag.

![image](https://github.com/user-attachments/assets/c380d33b-caba-4243-9f30-7a4d6f5c0a3a)

## Theory
`strings` extracts and prints readable text strings (ASCII or Unicode) from a binary file or any file containing non-readable data.
`binwalk`  is a static analysis tool used to inspect, extract and analyze data embedded in binary files.

## Solution
- Use `strings meow.jpg` for extract all readable strings in the file. In here we get `YoullNeverGetThis719482`.
- Using `binwalk -e meow.jpg` we get a zip file and a a trap image.
- When we click on the zip file ask us for a password, this is `YoullNeverGetThis719482`.
- We get another flag.png
- Using this last png file in https://georgeom.net/StegOnline/image and selecting **LSB Half** we get the flag.

flag: `INTIGIRTI{w4rmup_fl46z}`
