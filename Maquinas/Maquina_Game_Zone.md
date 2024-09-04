# Máquina Game Zone

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Game Zone**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos


![image](https://github.com/user-attachments/assets/33ca4154-cca0-427d-9d77-4a52a79e26ed)


Ya obtenidos los puertos abiertos buscamos sus versiones y vulnerabilidades utilizando la herramienta Nmap con diferentes parámetros.


![image](https://github.com/user-attachments/assets/328f8dde-0a61-47a6-83d8-43cc161f7935)


Conertimos el archivo PC.xml a HTML para poder visualizarlo mejor.


![image](https://github.com/user-attachments/assets/74791144-36b1-4f1c-b372-5498d992b352)



![image](https://github.com/user-attachments/assets/d9b3bfc2-4b7f-4851-93da-486bfa457ec1)



Lo convertimos a formato HTML para poder visualizarlo mejor


![image](https://github.com/user-attachments/assets/18bd259c-0c83-4df7-beeb-87d81519e6e7)



### Resumen de la Información Obtenida

|IP          | Sistema OPerativo    | Puertos/Servicios              | 
|:---------: |:--------------------:| :-----------------------------:| 
| 10.10.1.29 | Ubuntu Linux 16.04.6 | 22 ssh 7.2p2 Ubuntu 4ubuntu2.7 |
|            |                      | 80 http Apache httpd 2.4.18    |


## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos un Fuzzing


![image](https://github.com/user-attachments/assets/99acaa09-8ba0-4f9f-9078-308f77013e0f)

hacemos mysql injection en login 

    ' o 1=1 -- -

![image](https://github.com/user-attachments/assets/81597124-439e-4777-837e-222c80629bc6)



Logramos entrar


![image](https://github.com/user-attachments/assets/03ee549c-c53b-43f0-baff-e93f129387a0)


Aplicamos la siguiente consulta mysql para obtener el Hostname y su versión: 

       ' UNION SELECT NULL, @@HOSTNAME, @@VERSION#


![image](https://github.com/user-attachments/assets/f2d6ebf4-420e-478c-820a-5c58ffb8b25f)


Ahora aplicamos la siguiente consulta para encontrar el usuario y la base de datos:

    ' UNION SELECT NULL, user(), database()#


![image](https://github.com/user-attachments/assets/392a9b88-23ce-42bb-b603-4ce36a398382)


Ahora con la siguiente consulta vamos a enumerar las bases de datos disponibles 

    ' UNION SELECT NULL,NULL,SCHEMA_NAME FROM information_schema.SCHEMATA#


![image](https://github.com/user-attachments/assets/ecbeea66-15f8-4bb2-a17e-44867e5a2013)


Consultamos el esquema de la tabla db:

    ' UNION SELECT NULL,TABLE_NAME, COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_SCHEMA=’db’#


![image](https://github.com/user-attachments/assets/de7e8312-951b-45d2-80ad-c06e6060bfdf)

Realizamos una enumeración de username y las pwd de la tabla users:

    ' UNION SELECT 1, username, pwd FROM users #

![image](https://github.com/user-attachments/assets/654bf580-5066-4324-b515-7234031b9dde)


Guardamos el hash que conseguimos en un archivo de texto para luego decodificarlo con la herramienta de john the ripper


![image](https://github.com/user-attachments/assets/7e5fdf15-56ba-4b87-8d67-47f5f16da43f)


## 3. Explotación

Accedemos mediante ssh


![image](https://github.com/user-attachments/assets/6efb6497-f92d-491a-8160-e28d1c8cd716)

Obtenemos el contenido de user.txt 649ac17b1480ac13ef1e4fa579dac95c


![image](https://github.com/user-attachments/assets/b8ad5fd0-2a2d-494b-a58a-249ce0cb5ed9)


Colocamos nuestra máquina como servidor para poder descargar linpeas


![image](https://github.com/user-attachments/assets/d21621e6-47d9-4bde-bb63-c1e65fd84cdb)


Lo descargamos en la maquina destino, es recomendable situarnos en la ruta /dev/shm para hacer cualquier descarga en el equipo destino


![image](https://github.com/user-attachments/assets/e82b9d00-b925-4e5f-bd32-55d35e8b06b6)


Le damos permiso de ejecución y ejecutamos linpeas.sh


![image](https://github.com/user-attachments/assets/7db2e9e1-4dca-439f-981e-8ba0292bd455)

Nos detecta que tenemos el puerto 10000 abierto
![image](https://github.com/user-attachments/assets/395e5cc5-d348-4d43-a349-32dedafc01a1)



## 4. Escalación de privilegios

Ejecutamos un Port Forwarding creando un tunel del puerto 10000 de la máquina destino hacia nuestro puerto 10000


![image](https://github.com/user-attachments/assets/8eebc555-bf6a-406b-9611-45024178546c)

Luego de haber creado el túnel, abrimos en el navegador la siguiente dirección:

    http://localhost:10000/

![image](https://github.com/user-attachments/assets/ee1f29dd-1610-4dd7-8ae1-05813ae8555e)


Entramos con el usuario agent47 y la contraseña videogamer124

![image](https://github.com/user-attachments/assets/49b217b6-adf6-4b0f-b7cc-fb2f0083cbc6)


Abrimos el Meterpreter, buscamos la vulnerabilidad para webmin y vamos a utilizar la primera opción


![image](https://github.com/user-attachments/assets/7b2fd594-53ff-4aae-9bfc-7b29eb2921e8)


Show options


![image](https://github.com/user-attachments/assets/a4ece5bd-248d-468e-a6bd-8b6ec320e456)


Modificamos los diferentes parámetros para poder ejecutar el exploit


![image](https://github.com/user-attachments/assets/18ab38f2-4cd5-4beb-8a6b-ca7720cfaace)


Ejecutamos show payloads para ver las diferentes opciones que tenemos y en este caso vamos a utilizar la opción 5


![image](https://github.com/user-attachments/assets/7f698b83-80eb-4943-ae29-8ae36c7795f8)


Ejecutamos el comando:

     set payload 5 
                              
Y le damos exploit logrando acceder como root


![image](https://github.com/user-attachments/assets/624a73aa-1975-4c1d-948f-4d69d886e745)


Mejoramos la consola con el siguiente comando: 

    python -c 'import pty; pty.spawn("/bin/bash")’ 

Y encontramos el contenido del archivo de texto root.txt = a4b945830144bdd71908d12d902adeee


![image](https://github.com/user-attachments/assets/186327ec-97c4-401e-91de-f0c83bbb0710)



## 4.Banderas 🏁

|user.txt | 649ac17b1480ac13ef1e4fa579dac95c |
|:-------:|:--------------------------------:|
|root.txt | a4b945830144bdd71908d12d902adeee |


## 5.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 

• What is the name of the large cartoon avatar holding a sniper on the forum?

Agent 47

• When you've logged in, what page do you get redirected to?

portal.php

• In the users table, what is the hashed password?

ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14

• What was the username associated with the hashed password?

agent47

• What was the other table name?

Post

• What is the de-hashed password?

videogamer124

• What is the user flag?

649ac17b1480ac13ef1e4fa579dac95c

• How many TCP sockets are running?

5

• What is the name of the exposed CMS?

Webmin

• What is the CMS version?

1.580

•What is the root flag?

a4b945830144bdd71908d12d902adeee
