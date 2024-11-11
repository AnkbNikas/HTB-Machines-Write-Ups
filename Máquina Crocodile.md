Maquina: Crocodile

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 163108](https://github.com/user-attachments/assets/2d1d68c5-9b58-493a-85b0-cd0e56da3ec5)

Iniciamos la máquina y verificamos la conexión

ping 10.129.67.140 -c 1

![Screenshot 2024-11-10 163340](https://github.com/user-attachments/assets/1a99c3fe-c841-4c78-9686-b039b52d6705)

realizamos un escaneo de puertos y servicios

nmap -p- --min-rate 5000 -sV 10.129.67.140

el puerto 21 perteneciendo al servicio FTP y el puerto 80 perteneciendo al servicio HTTP están abiertos

![Screenshot 2024-11-10 163458](https://github.com/user-attachments/assets/6b0b54f4-6479-4e6f-95e8-19a4a8c37a46)

nos conectamos por ftp con anonymous sin password

![Screenshot 2024-11-10 163705](https://github.com/user-attachments/assets/cc21538d-febe-4fe8-9cd8-44df8c411134)

ls

cogemos las credenciales encontradas

![Screenshot 2024-11-10 163725](https://github.com/user-attachments/assets/fc3b0583-3a88-40a9-a55b-5c45be60a4e7)

![Screenshot 2024-11-10 163829](https://github.com/user-attachments/assets/af62e603-6197-4013-a1d5-4d3ef1c99d57)

buscamos directorios con Gobuster

gobuster dir -u http://10.129.67.140/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x .php

![Screenshot 2024-11-10 164434](https://github.com/user-attachments/assets/7aa4e0a9-ab7f-45d0-b676-d596c3fdd1f1)

hacemos loging con las credenciales encontradas
admin:rKXM59ESxesUFHAd
![Screenshot 2024-11-10 164538](https://github.com/user-attachments/assets/567312d5-f044-4c4d-9d79-b28ccb27f4a9)

y encontramos el flag

![Screenshot 2024-11-10 165007](https://github.com/user-attachments/assets/d4e8a21b-476b-4e64-90b0-c4712b131f9b)
