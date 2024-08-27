# Máquina Monkey

Dificultad --> Fácil-Medio 

Enlace de la maquina --> https://we.tl/t-RyoGIDKsOI

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la máquina

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la máquina 💻 victima


[![reconocimiento.jpg](https://i.postimg.cc/4drQ4Fkn/reconocimiento.jpg)](https://postimg.cc/gXHZNs0P)

Escaneando puertos y sus versiones utilizando el comando **nmap** con diferentes parámetros a la IP que conseguimos 🔎


[![reconocimiento2.jpg](https://i.postimg.cc/MTFqnVdn/reconocimiento2.jpg)](https://postimg.cc/sQ7t8QFz)


[![reconocimiento3.jpg](https://i.postimg.cc/bwprc0MP/reconocimiento3.jpg)](https://postimg.cc/CzrSb8Sr)

Convertimos el archivo que se nos genero para poder visualizarlo mejor con el siguiente comando **--> xsltproc nmap-results.xml -o nmap-results.html <--** y luego lo abrimos con el navegador


[![reconocimiento4.jpg](https://i.postimg.cc/wTxZC7VP/reconocimiento4.jpg)](https://postimg.cc/ThStm2Bq)


Nos podemos fijar que hay un login por ftp de manera anónima y que tenemos un archivo de texto llamado notas.txt


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                          | 
|:------------: |:-----------------:| :-----------------------------------------:| 
| 192.168.1.18  | Debian            | 21/tcp ftp vsftpd 3.0.3                    |
|               |                   | 22/tcp ssh openSSH 7.9p1 Debian 10+deb10u2 |
|               |                   | 80/tcp apache httpd 2.4.38                 |



## 2. Análisis de Vulnerabilidades / Debilidades

Nos conectamos mediante ftp a la máquina, colocamos usuario anonymous y la clave igual anonymous

[![analisis.jpg](https://i.postimg.cc/bNLp7Xjy/analisis.jpg)](https://postimg.cc/RN3ktDFy)


Buscamos el archivo notas.txt y vemos su contenido


[![analisis2.jpg](https://i.postimg.cc/jSSttRHg/analisis2.jpg)](https://postimg.cc/56TZmdkL)

## 3. Explotación


Deciframos la credencial obtenida en el archivo notas.txt en la pagina **crackstation** 

[![explotacion.jpg](https://i.postimg.cc/yNFHTWsN/explotacion.jpg)](https://postimg.cc/675gWW7s)


Realizamos una enumeración de directorios con gobuster

[![explotacion2.jpg](https://i.postimg.cc/HL0qyK9c/explotacion2.jpg)](https://postimg.cc/JynYwpgM)


Descargamos la base de datos

[![explotacion3.jpg](https://i.postimg.cc/HsJF2jtC/explotacion3.jpg)](https://postimg.cc/RNBPCS8P)


Recopilamos información dentro de la base de datos

[![explotacion4.jpg](https://i.postimg.cc/zfkQBhqT/explotacion4.jpg)](https://postimg.cc/3yd1fd4w)


Procedemos a cifrar la credencial obtenida de la base de datos en la pagina **crackstation**

[![explotacion5.jpg](https://i.postimg.cc/59rR6bWK/explotacion5.jpg)](https://postimg.cc/F1jZ6tR0)


Accedemos a uno de los directorios obtenidos 192.168.1.18/monkey/index.php y utilizamos las credenciales obtenidas

[![explotacion6.jpg](https://i.postimg.cc/ZYv1PCHz/explotacion6.jpg)](https://postimg.cc/w3Hfgxnw)

Ingresamos el Pincode que es **777777**  

Creamos un script para una shell reverse y modificamos la dirección IP y el puerto que vamos a utilizar

[![explotacion7.jpg](https://i.postimg.cc/yYyB9D6Y/explotacion7.jpg)](https://postimg.cc/QHCRZxWR)


Ya que pudimos ingresar a la pagina con las credenciales obtenidas vamos a subir al servidor nuestra Shell reverse

[![explotacion8.jpg](https://i.postimg.cc/9MLZrm4x/explotacion8.jpg)](https://postimg.cc/62GyPNfR)

Colocamos nuestro equipo al modo escucha con el comando nc -lvnp 9001 y luego procedemos a ejecutar nuestro reserve shell que subimos a la pagina lo cual nos dará un ingreso

[![explotacion9.jpg](https://i.postimg.cc/CLGDqsfB/explotacion9.jpg)](https://postimg.cc/kB5Br8sq)

Activamos todo el esquema de nuestra shell con los siguientes comandos:

**echo $TERM dumb**

**echo $SHELL /usr/sbin/nologin**

**TERM=xterm**

**SHELL=bash**

Seguimos recopilando información utilizando el comando **--> grep -r password<--**. El cual nos ayudara a conseguir otra credencial

[![explotacion10.jpg](https://i.postimg.cc/sDTjqMkH/explotacion10.jpg)](https://postimg.cc/rKt2twF5)


Esta contraseña pertenece al usuario hackermentor, lo podemos comprobar usando la herramienta “**crackmapexec**”. Ahora nos vamos a conectar mediante ssh 

[![explotacion11.jpg](https://i.postimg.cc/mDSf9qRp/explotacion11.jpg)](https://postimg.cc/qhg5V1N2)


Una vez obtenido el acceso ejecutamos el comando ls , tenemos la primera bandera 🚩 ejecutamos el comando cat para ver su contenido 

[![bandera1.jpg](https://i.postimg.cc/Jhb9wgy7/bandera1.jpg)](https://postimg.cc/Lq8yLxBG)

## 4. Escalación de privilegios

Ahora vamos a buscar la manera de escalar privilegios, primero buscamos en nuestra máquina una herramienta que se llama Linpeas.sh, algunas Kali ya la traen sino la puedes descargar ⬇️ en Github. Nos situamos en el directorio donde tenemos el archivo Linpeas.sh y procedemos a colocar nuestra máquina como servidor para luego descargar ⬇️ ese archivo en la máquina victima 💻 

[![escalar.jpg](https://i.postimg.cc/0rRYqtKx/escalar.jpg)](https://postimg.cc/m1jzYyS6)


Ejecutamos el linpeas.sh en la máquina victima 💻, el cual hará un análisis del equipo arrojando mucha información que la clasifica por color, conseguimos algunos UID que nos podemos aprovechar para escalar privilegios

[![escalar2.jpg](https://i.postimg.cc/BQbbq5Bf/escalar2.jpg)](https://postimg.cc/QHLhmTzf)


Editamos el archivo Backup.sh agregando **chmod +s /bin/bash** para obtener permisos SUID

[![escalar3.jpg](https://i.postimg.cc/L5qcWsLw/escalar3.jpg)](https://postimg.cc/fSNqtDHf)


Ejecutamos los siguientes comandos y logramos tener acceso como root

[![escalar4.jpg](https://i.postimg.cc/gk3zcML5/escalar4.jpg)](https://postimg.cc/cr4yXBmM)


Buscamos la badera 2 🚩 que se encuentra en el directorio root y vemos su contenido con el comando cat

[![bandera2.jpg](https://i.postimg.cc/28dYNVPN/bandera2.jpg)](https://postimg.cc/N50S1fsd)


## 5. Banderas 🏁

|Bandera 1 | 47ee0702e489445bae251df46bc88b73 |
|:--------:|:--------------------------------:|
|Bandera 2 | D844ce556f834568a3ffe8c219d73368 |

