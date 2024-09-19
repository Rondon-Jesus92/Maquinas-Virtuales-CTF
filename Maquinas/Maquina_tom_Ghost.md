# Máquina Tom Ghost

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Tom Ghost**


'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

[![image.png](https://i.postimg.cc/wMwHzTkg/image.png)](https://postimg.cc/cttPMWjb)

## 1.Reconocmiento

Escaneando puertos y sus versiones utilizando el comando de nmap con diferentes parámetros

[![reconocimiento.png](https://i.postimg.cc/DwJFVh43/reconocimiento.png)](https://postimg.cc/TK6BmZct)


    nmap -sV -sC -T4 10.10.97.38

[![reconocimiento1.png](https://i.postimg.cc/9fyhMBBx/reconocimiento1.png)](https://postimg.cc/McZ43VNR)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                    | 
|:------------: |:-----------------:| :-----------------------------------:| 
| 10.10.97.38   |Linux- Ubuntu      | 22  ssh OpenSSH 7.2p2                |
|               |                   | 53 domain tcpwrapped                 |
|               |                   | 8009 ajp13 Apache Jserv              |
|               |                   | 8080 http-proxy Apache tomcat 9.0.30 |


## 2. Análisis de Vulnerabilidades / Debilidades


Buscamos en nuestro equipo para ver si hay un exploit para el servicio ajp que corre en el puerto 8009

![image](https://github.com/user-attachments/assets/eca4cbd6-efd3-418b-9b78-15e7165f0280)

Buscamos la ruta del exploit

![image](https://github.com/user-attachments/assets/ee9440ed-d18d-4978-8da1-6e9ceb1f3061)


Ejecutamos el siguiente comando para ver si nos indica el correcto uso

    python2 exploit_ghostcat.py


![image](https://github.com/user-attachments/assets/3f2f550b-698c-493a-91b0-64a2c27fe8cf)


Armamos nuestra linea de comando :

    python2 exploit_ghostcat.py -p 8009 -f /WEB-INF/web.xml 10.10.97.38


Una vez ejecutado nos permite leer el archivo web.xml del servidor y conseguimos un usuario con una credencial 🔑

8730281lkjlkjdqlksalks

![image](https://github.com/user-attachments/assets/e63e2a06-fea5-48ac-a7c2-fa9ee73c58b0)


## 3. Explotación

Nos conectamos via ssh

![image](https://github.com/user-attachments/assets/ea89480f-6425-4533-8591-bd9b8e7fd061)


Ahora buscamos la bandera user.txt 🚩 en el directorio merlin

THM{GhostCat_1s_so_cr4sy}

![image](https://github.com/user-attachments/assets/11c24b5b-7df3-4e8c-ba64-e59155bfb049)


## 4. Escalación de privilegios

ahora vamos a buscar escalar privilegios

activamos un servidor http en la máquina victima para descargar en nuestra máquina los dos archivos que conseguimos en el directorio skyfuck

![image](https://github.com/user-attachments/assets/41ae077e-acd8-453d-82cf-83b3e2f864fb)


![image](https://github.com/user-attachments/assets/5edd8171-508b-4233-855c-c22fdd514a98)


Utilizamos el siguiente comando para convertir el archivo tryhackme.asc en uno que pueda leer john the ripper

    gpg2john tryhackme.asc > hash


![image](https://github.com/user-attachments/assets/46e2b5f8-585c-402d-927a-2302b8ab6ad9)


Crackeamos el archivo y obtenemos otra credencial

    john --wordlist=/usr/share/wordlists/rockyou.txt hash

![image](https://github.com/user-attachments/assets/0061a43a-7da3-497e-a4eb-393258ec1a6b)


ejecutamos los siguientes comando, en ambas oportunidades nos va pedir la contraseña y colocamos alexandru

obtuvimos las siguientes credenciales 🔑

merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j


![image](https://github.com/user-attachments/assets/ce0e5a14-3798-44e7-ab24-d586468eac65)


ejecutamos **--> su merlin <--** y escalamos al usuario merlin

ahora ejecutamos **--> sudo -l <--*


![image](https://github.com/user-attachments/assets/1a47f194-f09a-4fc7-9975-f1745998b85a)

Ahora buscamos como aprovecharnos de este binario


![image](https://github.com/user-attachments/assets/0e33118a-7367-4e61-82da-4faead58fb13)


Ejecutamos los comandos que se nos indica


![image](https://github.com/user-attachments/assets/13535298-c8a3-4a33-a1f0-948054e0fe30)


Una vez obtenido el acceso root ahora buscamos la bandera root.txt 🚩

THM{Z1P_1S_FAKE}

![image](https://github.com/user-attachments/assets/7078a511-c06d-4659-9f23-08e92f800880)


## 5.Banderas 🏁

|user.txt | THM{GhostCat_1s_so_cr4sy} |
|:-------:|:-------------------------:|
|root.txt | THM{Z1P_1S_FAKE}          |



## 6.Extra 🚨

* Compromise this machine and obtain user.txt

THM{GhostCat_1s_so_cr4sy

* Escalate privileges and obtain root.txt

THM{Z1P_1S_FAKE}





























































































































