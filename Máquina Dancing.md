Maquina: Dancing

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 151019](https://github.com/user-attachments/assets/c897f35c-1e31-4543-874a-8b436fa081fe)

Iniciamos la máquina y verificamos la conexión

ping 10.129.67.110 -c 1

![Screenshot 2024-11-10 151148](https://github.com/user-attachments/assets/ac35d6ea-efb4-49ec-9494-f8bfee4de85c)

realizamos un escaneo de puertos y servicios detallado

sudo nmap -sV 10.129.67.110

![Screenshot 2024-11-10 151255](https://github.com/user-attachments/assets/dccf2dff-573e-4559-9a0a-c9327ad1066a)

accedemos por smb

smbclient -L 10.129.67.110

vemos todos los ficheros compartidos que hay

![Screenshot 2024-11-10 151713](https://github.com/user-attachments/assets/8e4924c1-fb89-4b74-b78a-15dfae289ad2)

smbclient \\\\10.129.67.110\\WorkShares

Nos hemos conectado al servicio SMB y no ha requerido ninguna contraseña

![Screenshot 2024-11-10 152120](https://github.com/user-attachments/assets/12187e7a-796f-4007-875b-7dbea73b8f41)

encontramos dos directorios

entramos en los dos y cogemos el worknotes.txt y el flag.txt

![Screenshot 2024-11-10 152204](https://github.com/user-attachments/assets/5c6095f2-d0a3-4b1c-85c1-f8ee6a038d98)
