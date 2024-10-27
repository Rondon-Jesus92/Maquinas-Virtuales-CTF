# Máquina ColddBox

Dificultad --> Facil

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de ColddBox**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

![image](https://github.com/user-attachments/assets/4e7c65d6-ca3a-4c18-aeea-0814b3b6f26e)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos

    sudo nmap 10.10.128.13 -T4 


![image](https://github.com/user-attachments/assets/c7383a17-bb7a-4c40-b6df-eb4f6426e809)


    sudo nmap -sC -sV -Pn -T4 10.10.128.13


![image](https://github.com/user-attachments/assets/54bb630c-ab29-464d-98b8-55ec1929963e)



### Resumen de la Información Obtenida

|IP           | Sistema OPerativo | Puertos/Servicios           | 
|:----------: |:-----------------:| :--------------------------:| 
|10.10.169.58 | Ubuntu / Linux    | 80 http Apache httpd 2.4.18 |


## 2. Análisis de Vulnerabilidades / Debilidades 🔍

Realizamos un fuzzing con la herramienta gobuster

![image](https://github.com/user-attachments/assets/d9a6d2bc-75f9-4232-8fba-a284fa44fe1a)


En el directorio /hidden conseguimos tres usuarios C0ldd Hugo y Felipe

![image](https://github.com/user-attachments/assets/faf5f433-6b82-48c1-8019-bc8c3584e718)



Hacemos una enumeración de usuarios con la herramienta wpscan 

    wpscan --url http://10.10.169.58/ -e u


![image](https://github.com/user-attachments/assets/8235dc4c-37a7-4bc5-b4d5-894a0bdc5710)


Realizamos fuerza bruta con wpscan y el usuario c0ldd


    wpscan --url http://10.10.169.58/wp-login.php --passwords /usr/share/wordlists/rockyou.txt -U c0ldd


![image](https://github.com/user-attachments/assets/49546800-56e8-4412-9383-47bd515c8b17)



## 3. Explotación 💥

Con la contraseña obtenida logramos tener acceso al panel de control de wordpress, vamos a buscar una reverse shell en nuestra máquina, copiarla en nuestra carpeta de trabajo y editar el archivo


![image](https://github.com/user-attachments/assets/323e7086-0a27-45cf-a22e-253adc15ad66)


Colocamos nuestra dirección ip y el puerto el cual nos vamos a colocar a la escucha


![image](https://github.com/user-attachments/assets/1361a222-60bc-4fec-9a5a-577940122c58)


Una vez adentro en el panel de control de wordpress nos colocamos en la sección de  appearance , editor y seleccionamos 404 Template borramos la información y colocamos todo el contenido de nuestra reverse shell. Una vez hecho esto colocamos nuestra máquina en modo escucha y procedemos abrir el siguiente direccion la cual ejecutara nuestra revershell

    http://10.10.169.58/wp-content/themes/twentyfifteen/404.php


Una vez adentro procedemos a volver una shell mas dinámica 


![image](https://github.com/user-attachments/assets/0ccc1e57-a485-4cc6-9850-79ad94a16bef)


Como la pagina esta trabajando bajo wordpress mayormente se guarda una archivo llamado wp-config.php que guarda información de base datos, contraseña, usuarios. Vamos al directorio /var/www/html haber si conseguimos ese archivo 

Efectivamente encontramos el archivo


![image](https://github.com/user-attachments/assets/1aa4c6d8-c694-48b1-a94f-bca498d8a22e)


ejecutamos el comando cat al archivo y efectivamente conseguimos la contraseña para el usuario c0ldd


![image](https://github.com/user-attachments/assets/bdcf808a-17b7-4336-ba7a-2abf631e0f36)


Escalamos al usuario c0ldd y vamos al directorio home/c0ldd donde vamos a conseguir la primera bandera 🚩 la cual ya tenemos permiso para ver su contenido 

RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==


![image](https://github.com/user-attachments/assets/6bb10607-1eb4-4754-b1a0-f1b609638ff9)



## 4. Escalación de privilegios 🧗


Ahora vamos a proceder a escalar privilegios , ejecutamos sudo -l para ver si podemos aprovecharnos de un binario


![image](https://github.com/user-attachments/assets/1e0f67fb-bafd-4369-9154-03ae0e0620fc)


buscamos en la pagina gtfobins 


![image](https://github.com/user-attachments/assets/a97c85f9-69f3-4188-836b-76582243b32b)


Ejecutamos el comando que nos sale en la pagina, luego le damos un ENTER y verificamos que somos root con el comando whoami


![image](https://github.com/user-attachments/assets/6ff7edcf-ed09-454c-ada7-422a17151d0b)


Procedemos a ir al directorio root donde se encuentra la ultima bandera 🚩

wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=


![image](https://github.com/user-attachments/assets/ea4b69ca-24de-4d5c-baa5-51e00c71e36a)


## 5.Banderas 🏁

|user.txt | RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==  |
|:-------:|:-----------------------------------------------------:|
|root.txt | wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=      |


## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 


- user.txt
  
RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==

- root.txt
  
wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=







































