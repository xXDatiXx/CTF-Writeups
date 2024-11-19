# SkyWave 4: Longest Run
## Challenge
- SQL
- Beginner
- DEADFACE CTF

### Description
>Note: Access the database from High Tower.
>
>We need to determine which device had the longest running connection out of the towers with the following coordinates:
>
>- (41.639642, -79.220682)
>- (40.598271, -78.801089)
>- (41.045892, -79.068358)
>- (41.257279, -77.529468)
>
>Additionally, let’s focus on only finding the longest running connection with a dBm greater than -100.
>
>Submit the flag as flag{device_imei}. Example: flag{123456789012345}
### Objective
Determine which device had the longest connection at one of the towers with the provided coordinates, and filter the connections that have a signal strength greater than -100 dBm.

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

![image](https://github.com/user-attachments/assets/46fbbe73-7ac6-4c20-a517-6ae56f4e00d5)
![image](https://github.com/user-attachments/assets/f9800dca-e916-40bc-846a-492995ec7aca)
![image](https://github.com/user-attachments/assets/1ca5e383-55e3-44a9-b300-4d0f6b6acc27)
## Solution

1. Identify the `tower_id` of the towers at the given coordinates:
```SQL
SELECT tower_id FROM Towers
WHERE (latitude = 41.639642 AND longitude = -79.220682)
   OR (latitude = 40.598271 AND longitude = -78.801089)
   OR (latitude = 41.045892 AND longitude = -79.068358)
   OR (latitude = 41.257279 AND longitude = -77.529468);
```
Result:
```
+----------+
| tower_id |
+----------+
|      105 |
|      123 |
|      187 |
|      200 |
+----------+
```
2. Find the longest connection:
```SQL
SELECT device_id, MAX(connection_duration)
FROM Connections
WHERE tower_id IN (105, 123, 187, 200)
  AND signal_strength > -100
GROUP BY device_id
ORDER BY MAX(connection_duration) DESC
LIMIT 1;
```
Result:
```
+-----------+--------------------------+
| device_id | MAX(connection_duration)  |
+-----------+--------------------------+
|       344 |                    85709  |
+-----------+--------------------------+
```
3. Obtain the IMEI:
```SQL
SELECT device_imei
FROM Devices
WHERE device_id = 344;
```
Result:
```
+-----------------+
| device_imei     |
+-----------------+
| 845303290931675 |
+-----------------+
```
`flag{845303290931675}`
