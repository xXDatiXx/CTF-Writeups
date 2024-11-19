# SkyWave 5: Connections
## Challenge
- SQL
- Beginner
- DEADFACE CTF

### Description
>Note: Access the database from High Tower.
>
>We’re running with an assumption that d34th drove around and connected to various cell towers the day leading up to the attack.
>
>We need you to determine which device IMEI connected to the most unique towers on **September 7 from 16:10 to 18:54**.
>
>Submit the flag as flag{device_imei}. Example: flag{123456789012345}.

### Objective
The challenge provides us with a database of cell towers and mobile device connections. The goal is to determine which device (identified by its IMEI) connected to the most unique towers between September 7th from 16:10 to 18:54.

## Analysis
First, we need to connect to MySQL: `ssh skywave@skywave.deadface.io` `d34df4c3`.
With the `show tables;` command, we can see all the tables:

```
+-------------------------+
| Tables_in_cell_tower_db |
+-------------------------+
| Antennas                |
| Carriers                |
| Connections             |
| Device_Types            |
| Devices                 |
| Operators               |
| Technicians             |
| Tower_Maintenance       |
| Tower_Sectors           |
| Towers                  |
+-------------------------+
```
With the `describe [table_name]` command, we can see the columns in the tables of interest.

![image](https://github.com/user-attachments/assets/a97dde5f-bb9a-4d4a-9585-5003ddf74c18)
![image](https://github.com/user-attachments/assets/1b770318-dcd2-47e1-bb84-ed54d687d628)
![image](https://github.com/user-attachments/assets/4957d9c8-7498-48be-a309-85b9ed4099d5)

## Solution

1. SQL query to filter relevant connections
```SQL
SELECT device_id, COUNT(DISTINCT tower_id) as unique_towers
FROM Connections
WHERE connection_time BETWEEN '2024-09-07 16:10:00' AND '2024-09-07 18:54:00'
GROUP BY device_id
ORDER BY unique_towers DESC
LIMIT 1;
```
Result:
```
+-----------+---------------+
| device_id | unique_towers |
+-----------+---------------+
|      2279 |             5 |
+-----------+---------------+
```
2. Obtain the IMEI:
```SQL
SELECT device_imei
FROM Devices
WHERE device_id = 2279;
```
Result:
```
+-----------------+
| device_imei     |
+-----------------+
| 643366592089524 |
+-----------------+
```
`flag{643366592089524}`
