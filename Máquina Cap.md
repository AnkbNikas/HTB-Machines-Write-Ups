Maquina: Cap
Autor: 
Dificultad: Facil

Verificamos que la máquina está activa haciendo un ping.
ping -c 1 10.10.10.245 –R
Vemos que el ping nos responde con el ttl de la máquina de 63, maquina Linux, que en realidad serían 64 pero HTB por la conexión con un nodo intermediario, lo reduce en 1 unidad.

Enumeración: Usamos Nmap para escanear y descubrir posibles vulnerabilidades. Aquí es donde la curiosidad se convierte en acción.

Sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.245 -oG Escaneo

Una vez comprobado que la máquina está activa, vamos a lanzar un nmap con permisos de root para ver qué puertos están abiertos y exportar el resultado en un fichero llamado Escaneo.
Después el escaneo me devuelve que los puertos 21 ftp, 22 ssh y 80 http están abiertos.

Sabiendo que estos puertos están abiertos, lo que hacemos a continuación es lanzar un comando completo que recoja todo:
sudo nmap -sCV -p- --open --min-rate 5000 -n -vvv -Pn 10.10.10.245 -oG Scan 

Explotación:
Ataque IDOR (Insecure Direct Object Reference)
Parece que la opción «Security Snapshot» de la izquierda 
(si haces hovering la ruta es /capture ) va cambiando y 
generando nuevas urls con la opción de poder Descargar esa información.

Por ejemplo: http://10.10.10.245/data/6
Si más adelante vuelves a clicar, ya no tienes la /data/6 si no por ejemplo /data/2
Probamos con incluir un valor directamente de cero en la ruta para ver la respuesta, y efectivamente nos devuelve información con la opción de descargarnos el informe, por lo que nos lo descargamos y lo analizamos para ver qué contiene.
Parece que modificando el valor de la ruta puedemos acceder a información que de forma natural de funcionamiento de la página no tendrías, por lo que estamos ante un posible ataque IDOR.
Para analizar el contenido de ese fichero donde se están registrando información sobre paquetes de red, vamos a usar la herramienta tshark y vamos a incluir una serie de parámetros para facilitar la extracción de los datos que nos interesa ver.

tshark -r 0.pcap -Tfields -e tcp.payload 2>/dev/null | xxd -ps -r 
Podemos ver información interesante en el detalle del contenido de ese archivo como la PASS.

Acceso por SSH:

Teniendo esa PASS y con el usuario Nathan, vamos a intentar acceder por ssh.
Ssh Nathan@10.10.10.245
Buck3tH4TF0RM3!

y efectivamente hemos podido acceder y ver la user.txt flag.
ls –la
Sin embargo este usuario no tiene permisos de root en la máquina.

Escalada de privilegios: 
ESCALADA DE PRIVILEGIOS CON CAPABILITES
En este punto, vemos los permisos id y de sudoers por si ese usuario pudiera tener permisos especiales de ejecución como root, y parece que no.

Id

Teniendo eso en cuento vamos a ver si hay algún binario que nos permite abusar del privilegio de root.
Con esta línea vamos a buscar archivos con el bit SUID (Set User ID) activado. El valor 4000 representa este permiso especial en octal. Los archivos con este permiso se ejecutan con los privilegios de su propietario (root), incluso si quien los ejecuta es un usuario diferente.

find / -perm -4000 -user root 2>/dev/null | xargs ls- l

Hay muchos pero son los comunes por lo que parece que no podemos abusar de ninguno.
Teniendo en cuenta que la máquina se llama «Cap» es posible que mediante la opción de Linux Capabilities podamos encontrarlas y vemos si un archivo vulnerable o mal configurado tiene el bit SUID activo y es propiedad de root, y así cualquier usuario como Nathan puede ejecutarlo con privilegios de root.

getcap -r / 2>/dev/null

Si vemos la línea de python 3.8 , este archivo tiene las siguientes capabilities:
cap_net_bind_service: Permite que el archivo pueda asociarse a puertos de red por debajo del 1024, que normalmente requieren permisos de root.
cap_setuid: Permite que el archivo cambie su UID efectivo (identificador de usuario). Esto es importante porque podría usarse para cambiar de usuario, lo que en ciertos casos podría permitir la escalación de privilegios.
Posible riesgo: La combinación de estas capabilities significa que cualquier script o proceso que use python3.8 podría, en teoría, manipular puertos de red y cambiar su UID.

Linux Capabilities es un sistema más granular que permite asignar permisos específicos a un proceso en lugar de usar el sistema SUID, que otorga todos los permisos de root. Capabilities divide los permisos de root en unidades más pequeñas (capabilities), como CAP_NET_ADMIN para administración de red o CAP_SYS_MODULE para cargar módulos del kernel. Esto ayuda a mejorar la seguridad al evitar la necesidad de usar SUID en muchos casos.

Si ahora vamos a GTFObins en Python y Capabilities , podemos ver el CAP_SETUID que podemos aplicar para comprometer la máquina.

Ajustas la petición teniendo en cuenta lo anterior, y ya tendríamos permisos de root y podríamos acceder a la flag de root:

python3.8 -c 'import os; os.setuid(0); os.system("bash")’

whoami

Cd /root
ls
Cat root.txt

¡Pwned!













