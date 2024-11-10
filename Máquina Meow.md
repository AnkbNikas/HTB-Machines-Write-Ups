Maquina: Meow 

Autor: 

Dificultad: Muy Facil

![Screenshot 2024-11-10 125853](https://github.com/user-attachments/assets/4282b9cf-d7fb-4412-a9a3-6363edfb9b68)

Para empezar, hacemos un ping para asegurarnos de que esté activa

ping 10.129.1.17 -c 1

![Screenshot 2024-11-10 130155](https://github.com/user-attachments/assets/93318a38-5d05-42c9-8a52-35dc807caa5d)

ejecutar un nmap para enumerar los servicios en ejecución y sus respectivos puertos

nmap 10.129.1.17

![Screenshot 2024-11-10 130221](https://github.com/user-attachments/assets/0dd5d941-4de5-42b5-b6af-ac915488b177)

Podemos ver que el puerto 23 está abierto, ejecutando el servicio telnet

Usando el comando telnet, podemos intentar iniciar sesión en la máquina objetivo

se nos concedió acceso sin una contraseña utilizando el nombre de inicio de sesión root

![Screenshot 2024-11-10 130416](https://github.com/user-attachments/assets/933505ac-e896-444f-b85e-29758daba3c0)

Usando el comando ls, podemos ver qué contenidos hay en el directorio actual

ls

Vemos el archivo flag.txt

Usando el comando cat, podemos ver el contenido del archivo

cat flag.txt

![Screenshot 2024-11-10 130451](https://github.com/user-attachments/assets/d959747c-8130-4fc4-84b0-07d8d9aee9c1)

hemos capturado la bandera!
