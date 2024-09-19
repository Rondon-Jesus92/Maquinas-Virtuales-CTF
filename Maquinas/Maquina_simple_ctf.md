# Máquina Simple CTF

Dificultad --> Facil

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Simple CTF**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

![image](https://github.com/user-attachments/assets/d732fcad-cc76-4228-b81d-ec7b618526db)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos


![image](https://github.com/user-attachments/assets/59607f6a-127b-4080-8e41-6ba158a6caa9)


    sudo nmap -sV -sC -T4 10.10.84.56 -vv

![image](https://github.com/user-attachments/assets/adddfe16-5bea-46fe-ab27-aecdf8ff6b08)


![image](https://github.com/user-attachments/assets/38256d6c-898c-49d1-ab52-e7d597653fd5)


![image](https://github.com/user-attachments/assets/1f45f5d0-2508-44fe-b21d-8e44c8907525)


### Resumen de la Información Obtenida

|IP           | Sistema OPerativo | Puertos/Servicios           | 
|:----------: |:-----------------:| :--------------------------:| 
| 10.10.84.56 | Ubuntu / Linux    | 21 ftp vsftpd 3.0.3         |
|             |                   | 80 http Apache httpd 2.4.18 |
|             |                   | 2222 OpenSSH 7.2p2          |


## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos un fuzzing con el siguiente comando :

    gobuster dir -u 10.10.84.56 -w /usr/share/dirb/wordlists/common.txt -t 100 


![image](https://github.com/user-attachments/assets/e1d0b4af-a27b-43eb-82cb-c22e60be79af)

Abrimos el directorio /simple/ en la cual conseguiremos una página que trabaja bajo CMS Made Simple version 2.2.8 el cual tiene una vulnerabilidad

![image](https://github.com/user-attachments/assets/732ae31b-b42a-4988-99c1-35b257ec1571)


## 3. Explotación 

Buscamos un exploit para esta vulnerabilidad

![image](https://github.com/user-attachments/assets/b8329223-3f3d-4bf4-96a0-c0ae0c8f0542)


localizamos la ruta del exploit para poder copiarlo en nuestra carpeta de trabajo

![image](https://github.com/user-attachments/assets/81f358bb-863c-490c-b25e-b486a65ae19a)


ejecutamos el exploit de la siguiente manera el cual nos va indicar la sintaxis correcta para ejecutarlo


![image](https://github.com/user-attachments/assets/f545fefb-a350-4e08-a36b-46a2f2dcc88a)


Ejecutamos el siguiente comando:


    python2 cmsexploit.py -u http://10.10.84.56/simple/ --crack -w /usr/share/wordlists/rockyou.txt


![image](https://github.com/user-attachments/assets/a2012cc5-3f45-4d8c-86db-57bf459ae1c9)


Obtuvimos un usuario, email y una contraseña


![image](https://github.com/user-attachments/assets/d3f7ce2b-fb49-4d4a-80b3-fc53667176e8)


Nos conectamos por ssh y el puerto 2222


![image](https://github.com/user-attachments/assets/a33f8195-e81c-4cb9-9b07-b3cf0e75e655)


Tenemos la bandera user.txt 🚩: G00d j0b, keep up!

![image](https://github.com/user-attachments/assets/38f3a61d-a4a9-4915-8871-c7780de896a8)


## 4. Escalación de privilegios

Buscando como escalar privilegios ejecutando el siguiente comando:

    sudo -l

![image](https://github.com/user-attachments/assets/6aba3933-4bd6-45e2-83ad-2abd9727ee21)


Buscamos como escalar privilegios a través del binario vim 


![image](https://github.com/user-attachments/assets/36ef67ad-022d-4f23-ad23-f5c52d07e7a2)


ejecutamos el siguiente comando para escalar privilegios como root

    sudo vim -c ':!/bin/sh’


![image](https://github.com/user-attachments/assets/ab1b7914-db11-41ad-a340-a98a6718f0d9)


Ahora buscamos la bandera root.txt 🚩 : W3ll d0n3. You made it!


![image](https://github.com/user-attachments/assets/76308b36-6a77-45b9-9644-310a7da9df08)



## 5.Banderas 🏁

|user.txt | G00d j0b, keep up!      |
|:-------:|:-----------------------:|
|root.txt | W3ll d0n3. You made it! |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 

- How many services are running under port 1000?

2

- What is running on the higher port?

ssh

- What's the CVE you're using against the application?

CVE-2019-9053

- To what kind of vulnerability is the application vulnerable?

sqli

- What's the password?

secret

Where can you login with the details obtained?

ssh

- What's the user flag?

G00d j0b, keep up!

- Is there any other user in the home directory? What's its name?

sunbath

- What can you leverage to spawn a privileged shell?

vim

- What's the root flag?

W3ll d0n3. You made it!





































































































