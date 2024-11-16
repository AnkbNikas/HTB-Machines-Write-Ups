aquina: Active

Autor:

Dificultad: Facil

![Screenshot 2024-11-16 073259](https://github.com/user-attachments/assets/0d7be530-5bd6-4948-8b64-21856aca457e)

Iniciamos la máquina y verificamos la conexión

ping 10.129.135.122 -c 1

![Screenshot 2024-11-16 073316](https://github.com/user-attachments/assets/39e62893-7ef3-4009-b47c-0c3cd5f76c2e)

nmap -p- --open -sS -sC -sV --min-rate 5000 -vvv -n -Pn 10.129.135.122 -oN escaneo

![Screenshot 2024-11-16 074828](https://github.com/user-attachments/assets/4568bbbc-a67c-4d23-9c8f-b0b254739bab)

añadimos la ip al /etc/hosts
![Screenshot 2024-11-16 075516](https://github.com/user-attachments/assets/4247f061-a92a-436a-9e43-7f2fa68e843c)


usamos smbmap  para ver permisos

![Screenshot 2024-11-16 075859](https://github.com/user-attachments/assets/fc13d101-363a-4b72-91ba-504ca7a7a7aa)

![Screenshot 2024-11-16 075927](https://github.com/user-attachments/assets/114e6062-2ea1-46b0-83ab-4e3a8e1eeaa1)

![Screenshot 2024-11-16 075945](https://github.com/user-attachments/assets/437e3cc0-dba2-4383-acc5-f24a63d3d6ba)

![Screenshot 2024-11-16 080005](https://github.com/user-attachments/assets/53d261c6-60d1-4ad7-8a5c-62c3f9b9b71e)

![Screenshot 2024-11-16 080043](https://github.com/user-attachments/assets/8855a110-3720-4163-9569-5feed28b4368)

![Screenshot 2024-11-16 080106](https://github.com/user-attachments/assets/d6bcebbc-538e-4f39-82ba-a1cca978f018)

![Screenshot 2024-11-16 080133](https://github.com/user-attachments/assets/1770c3db-4029-4e1a-bc9b-8a51e9908e91)

![Screenshot 2024-11-16 080157](https://github.com/user-attachments/assets/fd70aa2f-04cd-468c-a223-3b1c94c05578)

nos bajamos el archivo Groups.xml

![Screenshot 2024-11-16 080341](https://github.com/user-attachments/assets/7d442d3a-3c96-4851-8a32-4f0c2192ae7b)

le cambiamos el nombre para que sea mas comodo

![Screenshot 2024-11-16 080614](https://github.com/user-attachments/assets/2fbe4f57-4b80-4216-a12f-7b53cb557345)

Descubrimos un userName y un cpassword

![Screenshot 2024-11-16 080644](https://github.com/user-attachments/assets/50f45162-32dd-44b3-bd89-634d13f93aaa)

desencriptamos el cpassword con gpp-decrypt

![Screenshot 2024-11-16 080831](https://github.com/user-attachments/assets/1d8a22b6-9e17-458a-9d77-7255fff7282d)

usamos smbmap y descubrimos el user.txt

![Screenshot 2024-11-16 083939](https://github.com/user-attachments/assets/c9808703-be82-4443-a961-2dced2e19323)

nos lo bajamos y le cambiamos el nombre para que sea mas facil

![Screenshot 2024-11-16 084349](https://github.com/user-attachments/assets/86f82480-029a-45c4-95e4-4f77db7483a3)

![Screenshot 2024-11-16 084404](https://github.com/user-attachments/assets/6a5a0df7-21a4-4215-bffc-5ed93bc36c43)

Ahora usamos impacket con las credenciales obtenidas y encontramos un hash del usuario Administrator

![Screenshot 2024-11-16 081600](https://github.com/user-attachments/assets/bda8a821-3595-42ee-9a06-4fce0678647b)

copiamos el hash, creamos un file y lo pegamos ahi

![Screenshot 2024-11-16 081656](https://github.com/user-attachments/assets/6abf5a23-5c49-4a65-8384-d045d55fc112)

usamos John y conseguimos descifrar la contraseña

![Screenshot 2024-11-16 082553](https://github.com/user-attachments/assets/1e6eb92f-6b8a-4cdc-9ea5-6075f36e0630)

Accedemos con impacket con los credenciales obtenidos

![Screenshot 2024-11-16 082841](https://github.com/user-attachments/assets/f4233950-5037-4183-bfd7-c22a6d390ef1)

Vamos buscando hasta encontrar el root.txt

whoami
cd..
cd..
dir
cd Users
dir
cd Administrator
dir
cd Desktop
type root.txt

![Screenshot 2024-11-16 083311](https://github.com/user-attachments/assets/105168de-56c4-4475-ab43-86c73320a06b)

![Screenshot 2024-11-16 083344](https://github.com/user-attachments/assets/69c0d9da-0761-4161-bb49-839661841935)
