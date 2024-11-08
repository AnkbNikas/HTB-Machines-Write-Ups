Maquina: Meow 

Autor: 

Dificultad: Muy Facil

Para empezar, hacemos un ping para asegurarnos de que esté activa

ping 10.129.241.156

ejecutar un nmap para enumerar los servicios en ejecución y sus respectivos puertos

nmap 10.129.241.156

Podemos ver que el puerto 23 está abierto, ejecutando el servicio telnet

Usando el comando telnet, podemos intentar iniciar sesión en la máquina objetivo

se nos concedió acceso sin una contraseña utilizando el nombre de inicio de sesión root

Usando el comando ls, podemos ver qué contenidos hay en el directorio actual

ls

Vemos el archivo flag.txt

Usando el comando cat, podemos ver el contenido del archivo

cat flag.txt

hemos capturado la bandera!
