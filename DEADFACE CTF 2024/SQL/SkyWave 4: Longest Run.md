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
### Objetivo
Determinar qué dispositivo tuvo la conexión más larga en una de las torres con las coordenadas proporcionadas y filtrar las conexiones que tienen una señal de más de -100 dBm.

## Analysis
Primero nos debemos conectar a MySQL `ssh skywave@skywave.deadface.io` `d34df4c3`.
Con el comando `show tables;` podemos ver todas las tablas:

Imagen

Con el comando `describe [nombre_de_tabla]` podemos ver las columnas que tienen las tablas de interés:

Imagen
Imagen
Imagen

## Solution

1. Identificar los `tower_id` de las torres en las corrdenadas dadas:
```SQL
SELECT tower_id FROM Towers
WHERE (latitude = 41.639642 AND longitude = -79.220682)
   OR (latitude = 40.598271 AND longitude = -78.801089)
   OR (latitude = 41.045892 AND longitude = -79.068358)
   OR (latitude = 41.257279 AND longitude = -77.529468);
```
Resultado:
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
2. Buscar la conexión más larga:
```SQL
SELECT device_id, MAX(connection_duration)
FROM Connections
WHERE tower_id IN (105, 123, 187, 200)
  AND signal_strength > -100
GROUP BY device_id
ORDER BY MAX(connection_duration) DESC
LIMIT 1;
```
Resultado:
```
+-----------+--------------------------+
| device_id | MAX(connection_duration) |
+-----------+--------------------------+
|       344 |                    85709 |
+-----------+--------------------------+
```
3. Obtener IMEI
```SQL
SELECT device_imei
FROM Devices
WHERE device_id = 344;
```
Resultado:
```
+-----------------+
| device_imei     |
+-----------------+
| 845303290931675 |
+-----------------+
```
`flag{845303290931675}`