nmap: 

![image](https://github.com/user-attachments/assets/d16c07ba-9aa8-4742-a50e-a4dd478c52d6)

entramos a la pagina que esta en el puerto 80

![image](https://github.com/user-attachments/assets/77a96fe0-4bdb-43e9-9dbd-1ecef4c9b00e)

no hay mucho, tampoco en el código fuente. Vemos que hay en el puerto 21, le hacemos un escaneo

![image](https://github.com/user-attachments/assets/9e51ed5f-543a-4a60-a206-d6737cfcc055)

vemos que no tiene password y que se conecta con el nombre "anonymous" y nos pasamos la imagen que contiene

![image](https://github.com/user-attachments/assets/3a39b7bc-340b-4a44-bbfd-f33d1585fc7b)

la imagen son dulces

![image](https://github.com/user-attachments/assets/3dc20c30-9330-4d50-a4bb-e45d9d9cfb64)

con exiftool no da información importante, extraemos los metadatos de la imagen con

     stegseek gum_room.jpg  

y nos da

![image](https://github.com/user-attachments/assets/abf2e269-2b66-466f-a082-2ea1654fb350)

como en el final da esa "= =" pienso que es base64 porque aparte cuando lance el comando dio esto

![image](https://github.com/user-attachments/assets/38c41722-4332-4bf6-9ea6-41f0fe5e2bd8)

el bs64.txt es porque tiene que ser base64, lo pasamos a esta pagina que aparece de las primeras https://www.base64decode.org/

y nos da como el contenido de "/etc/passwd" 

![image](https://github.com/user-attachments/assets/0a25ab61-fa3f-4ad4-a2b0-9f8cd42b9990)

y la ultima linea tiene un usuario "charlie" y con un hash que podría ser la contraseña 
creamos un hash con la ultima linea y lanzamos con jhon

     john contraseña.hash --wordlist=/usr/share/wordlists/rockyou.txt

![image](https://github.com/user-attachments/assets/4c47256a-51ca-4c2c-93a4-8e5951cfcacd)

obtenemos la contraseña que podríamos probar en el login como en el ssh, si probamos en el login nos manda aqui

![image](https://github.com/user-attachments/assets/b3e0a21c-ca70-4512-96aa-e2dd0916189d)

en el cuadro de ahí si ejecutamos por ejemplo whoami nos da información

![image](https://github.com/user-attachments/assets/1e1eb607-138b-469b-a2f0-feb6f7652bd4)

después de probar varias cosas ahí podemos lanzar una shell python3#2 https://www.revshells.com/

la colocamos en el ejecutable y colocamos la terminal en escucha y lanzamos

![image](https://github.com/user-attachments/assets/f5e20c09-e90e-4dd2-9d31-99a7b5c58b84)

dentro en home/charlie encontré esto 

![image](https://github.com/user-attachments/assets/24526083-812a-4eb0-9474-5c068eb229be)

que podemos pasarlo para poder entrar a charlie creando un rsa sin necesidad de saber la contraseña en si, creamos con nano un "id_rsa" dentro colocamos la clave que nos dio, le damos permisos 600 e intentamos ingresa

     ssh -i id_rsa charlie@10.10.132.145

![[tryhackme/easy/chocolate factory/conexion2.png]]

y obtenemos conexión a charlie.
al hacer un sudo -l veremos que se puede escalar por "vi" buscamos en https://gtfobins.github.io/gtfobins/vi/#sudo

y lanzamos este comando

     sudo -u root  vi -c ':!/bin/sh' /dev/null

![[tryhackme/easy/chocolate factory/root.png]]

y seremos root! 


