# Máquina Robot

Dificultad --> Medio

Enlace de la maquina --> https://drive.google.com/file/d/17aUpHkn3ZuuOGcksA_U7_-Xp4ImOCojx/view

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la maquina

'

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Buscando la IP de la máquina con sudo arp-acan -l

![image](https://github.com/user-attachments/assets/a5427343-b0f5-465a-882c-fc01d2de39f8)


Buscando los puertos abierto utilizando Nmap:

    sudo nmap -p- -T4 -sS -vvv 192.168.1.13 | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//

![image](https://github.com/user-attachments/assets/c0d70f3e-ed56-4201-ae83-9b7c841883a4)


Buscando vulnerabilidades a los puertos abiertos con Nmap


![image](https://github.com/user-attachments/assets/1b8afef8-a49c-49ed-8d18-2d50814b06e1)


Convirtiendo el archivo pc.xml a html para poder visualizarlo mejor




![image](https://github.com/user-attachments/assets/9eac354b-eb6d-437f-929c-f5f9babc37f7)


![image](https://github.com/user-attachments/assets/703e56e8-1f08-44dc-993f-998475d3e9a4)


![image](https://github.com/user-attachments/assets/3ec563b6-b595-497a-b658-a147c82083b5)


Convirtiendo el archivo pv.xml a html para poder visualizarlo mejor


![image](https://github.com/user-attachments/assets/c97df58e-702f-4afa-bfb5-d324e6ca9b90)


![image](https://github.com/user-attachments/assets/e83574c7-a1ed-4995-acc6-2d918d64a019)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios | 
|:------------: |:-----------------:| :----------------:| 
| 192.168.1.13  | Ubuntu 14.04.2 LTS| 80 http Apache    |
|               |                   | 443 http Apache   |


## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos fuzzing

![image](https://github.com/user-attachments/assets/c0b4de2a-cac9-472d-a905-eb1e3d4a1616)


Revisamos la dirección 192.168.1.13/robots.txt


![image](https://github.com/user-attachments/assets/fa337f19-ffa6-4e6c-813b-198e4776b18f)


Conseguimos el contenido de la bandera 1 🚩: b8a2bd7f70b405df8823bd4442892c6c


![image](https://github.com/user-attachments/assets/90fe3866-d926-4d45-8fb6-cd14f3ff5d46)


## 3. Explotación

El archivo fsocity.dic lo descargamos y le aplicamos el comando --> **sort -u**<-- para que solo nos de entradas únicas y así eliminar las repetidas creando un nuevo diccionario llamado **dic1.txt**

![image](https://github.com/user-attachments/assets/b0a6d48f-ffce-43c0-b15c-3d4f861aa633)


Ingresamos a la dirección 192.168.1.13/wp-login.php presionamos F12 para buscar el payload que se genero al ingresar admin:admin en usuario y contraseña


![image](https://github.com/user-attachments/assets/9bd6cb38-4982-45bd-8323-e4bd0f1a8eb9)


Armamos el payload y buscamos el usuario con hydra


    hydra -t 64 -L diccionario -p test 192.168.1.13 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F192.168.1.13%2Fwp-admin%2F&testcookie=1:Invalid username" -V


![image](https://github.com/user-attachments/assets/9a87bdb1-ee05-4d75-a91b-c1d3c6343a5e)


Encontramos el usuario: Angela

![image](https://github.com/user-attachments/assets/f1e0057b-9bbc-4d3e-bc16-7b955062a784)

Realizamos fuerza bruta con:

    wpscan --url http://192.168.1.13/ --usernames user.txt --passwords dic1.txt 

Y conseguimos la contraseña


![image](https://github.com/user-attachments/assets/d30fc919-d70f-4513-824e-db085fdf3b34)


Tenemos la contraseña


![image](https://github.com/user-attachments/assets/8bdc799a-79ad-40fd-ab2d-76a6ff2cc355)


Ahora vamos a utilizar esta vulnerabilidad para crear nuestra shell reverse


![image](https://github.com/user-attachments/assets/15e18b6b-f1d4-49e6-845e-9960c18bc512)


Vamos a utilizar esta shell reverse que tenemos en nuestro Kali


![image](https://github.com/user-attachments/assets/492c540c-e050-47fb-9fd5-ff32aa47305c)


Copiamos el contenido de php-reverse-shell.php y lo pegamos en el script del error de 404 template, modificando la ip y el puerto que vamos activar el netcat


![image](https://github.com/user-attachments/assets/88d69b76-d4cd-49aa-98d2-87f5845931ad)


Copiamos en el navegador el siguiente link terminando con el puerto 443 con el caul activamos el netcat en nuestro equipo


![image](https://github.com/user-attachments/assets/e9531acb-c91c-4177-bc5d-78f6746e0fc1)


Ingresamos


![image](https://github.com/user-attachments/assets/aca12688-b622-49b5-bc98-203538b6ae65)


Buscando las Banderas

![image](https://github.com/user-attachments/assets/0edba3fa-e269-4185-97fc-43763ce2c45d)


Mejoramos nuestra Shell


![image](https://github.com/user-attachments/assets/6b6e73e6-fd44-43d8-94c4-b90814fe8df7)


Conseguimos el siguientes hash 3f15b52bfa4d874fa7d42b173c1a341d


![image](https://github.com/user-attachments/assets/d2c258c7-9de3-490a-9ac2-94aac0fd735b)


Codificamos el Hash en la página https://crackstation.net/ : sayajin23

![image](https://github.com/user-attachments/assets/cfba0a1e-c28c-4920-bb13-3731d8ac4243)


## 4. Escalación de privilegios


Escalamos al usuario **robot** utilizando la contraseña encontrada


![image](https://github.com/user-attachments/assets/73f2c861-f188-4207-ab2b-cbb5f56d0d9b)


Obtenemos el valor de la bandera 2 🚩: c6ad356a6d4ab0c2c9d033caadf28469


![image](https://github.com/user-attachments/assets/687cd7c1-5293-4c43-a736-8cc6ffa15d95)


Buscamos para escalar privilegios como root

![image](https://github.com/user-attachments/assets/c80f2d18-f54f-4b20-8b37-d4f320bde144)


Buscando los comando a ejecutar para escalar privilegios en la pagina de gtfobins


![image](https://github.com/user-attachments/assets/0c4edc05-8a3a-4ba5-9036-1dfdf4df0fb8)


Logramos acceder como root


![image](https://github.com/user-attachments/assets/27d51572-a721-4dea-ae13-de6733d1e033)


Obtenemos el contenido de la bandera 3🚩: 6c6b1c7089af9c9bb7ac78f06c3c1685


![image](https://github.com/user-attachments/assets/da6e4e7f-8412-4de2-a79f-49c2ddfbc0ad)


## 4.Banderas 🏁

|Bandera 1 | b8a2bd7f70b405df8823bd4442892c6c |
|:--------:|:--------------------------------:|
|Bandera 2 | c6ad356a6d4ab0c2c9d033caadf28469 |
|Bandera 3 | 6c6b1c7089af9c9bb7ac78f06c3c1685 |



