nmap: 

![image](https://github.com/user-attachments/assets/d16c07ba-9aa8-4742-a50e-a4dd478c52d6)

entramos a la pagina que esta en el puerto 80

![image](https://github.com/user-attachments/assets/77a96fe0-4bdb-43e9-9dbd-1ecef4c9b00e)

no hay mucho, tampoco en el código fuente. Vemos que hay en el puerto 21, le hacemos un escaneo

![image](https://github.com/user-attachments/assets/9e51ed5f-543a-4a60-a206-d6737cfcc055)

vemos que no tiene password y que se conecta con el nombre "anonymous" y nos pasamos la imagen que contiene

![image](https://github.com/user-attachments/assets/3a39b7bc-340b-4a44-bbfd-f33d1585fc7b)

la imagen son dulces

![[imagen.png]]

con exiftool no da información importante, extraemos los metadatos de la imagen con

     stegseek gum_room.jpg  

y nos da

![[steeg.png]]

como en el final da esa "= =" pienso que es base64 porque aparte cuando lance el comando dio esto

![[bs64.png]]

el bs64.txt es porque tiene que ser base64, lo pasamos a esta pagina que aparece de las primeras https://www.base64decode.org/

y nos da como el contenido de "/etc/passwd" 

![[tryhackme/easy/chocolate factory/base64.png]]

y la ultima linea tiene un usuario "charlie" y con un hash que podría ser la contraseña 
creamos un hash con la ultima linea y lanzamos con jhon

     john contraseña.hash --wordlist=/usr/share/wordlists/rockyou.txt

![[john.png]]

obtenemos la contraseña que podríamos probar en el login como en el ssh, si probamos en el login nos manda aqui

![[tryhackme/easy/chocolate factory/login.png]]

en el cuadro de ahí si ejecutamos por ejemplo whoami nos da información

![[whoami.png]]

después de probar varias cosas ahí podemos lanzar una shell python3#2 https://www.revshells.com/

la colocamos en el ejecutable y colocamos la terminal en escucha y lanzamos

![[tryhackme/easy/chocolate factory/conexion.png]]

dentro en home/charlie encontré esto 

![[rsa.png]]

que podemos pasarlo para poder entrar a charlie creando un rsa sin necesidad de saber la contraseña en si, creamos con nano un "id_rsa" dentro colocamos la clave que nos dio, le damos permisos 600 e intentamos ingresa

     ssh -i id_rsa charlie@10.10.132.145

![[tryhackme/easy/chocolate factory/conexion2.png]]

y obtenemos conexión a charlie.
al hacer un sudo -l veremos que se puede escalar por "vi" buscamos en https://gtfobins.github.io/gtfobins/vi/#sudo

y lanzamos este comando

     sudo -u root  vi -c ':!/bin/sh' /dev/null

![[tryhackme/easy/chocolate factory/root.png]]

y seremos root! 


