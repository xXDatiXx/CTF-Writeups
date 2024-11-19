# SkyWave 7: Bad Handwriting
## Challenge
- SQL
- DEADFACE CTF

### Description
>Note: Access the database from High Tower.
>
>One of the technicians performed maintenance on Tower 133 on August 26, 2024, but the technician's handwriting on the repair log cannot be deciphered. What is the employee number of the employee who conducted maintenance on that tower and on that date?
>
>Submit the flag as flag{employee_number}. Example: flag{T123456789}.
### Objective
Find the employee number of the technician who performed maintenance at Tower 133 on the specified date.

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

![image](https://github.com/user-attachments/assets/e5895fbe-b150-4c24-a754-9cfc27b8aba5)
![image](https://github.com/user-attachments/assets/deaef8b2-1dea-40d2-b2e6-d33ae520343e)

## Solution
1. Find the technician who performed maintenance at Tower 133
```SQL
SELECT technician_id
FROM Tower_Maintenance
WHERE tower_id = 133
AND maintenance_date = '2024-08-26';
```
Result:
```
+---------------+
| technician_id |
+---------------+
|           323 |
+---------------+
```
2. Obtain the technician's employee number
```SQL
SELECT employee_number
FROM Technicians
WHERE technician_id = 323;
```
Result:
```
+-----------------+
| employee_number |
+-----------------+
| T263739990      |
+-----------------+
```
`flag{T263739990}`
