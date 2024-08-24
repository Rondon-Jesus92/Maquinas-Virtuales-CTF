#MAQUINA KIO

###Dificultad --> Easy
###Enlace a la Maquina --> 

## 1.Reconocmiento / Análisis de Vulnerabilidades / Debilidades

Comenzamos realizando un escaneo general con nmap sobre la IP de la máquina víctima para ver que puertos tiene abiertos.

[![reconocimento.jpg](https://i.postimg.cc/jSDk2G1h/reconocimento.jpg)](https://postimg.cc/qNdGZZ4t)

Ejecutamos el siguiente comando para verificar si el servicio samba esta activo

[![reconocimiento2.jpg](https://i.postimg.cc/jjPTwW1v/reconocimiento2.jpg)](https://postimg.cc/DWvHRznb)

Ejecutamos msfconsole para utilizar un auxiliar que nos va permitir saber que version del servicio samba que esta ejecutando

[![reconocmiento3.jpg](https://i.postimg.cc/0jV4rZtS/reconocmiento3.jpg)](https://postimg.cc/30D1cjQr)

Una vez iniciado msfconsole buscamos el auxiliar con el comando --> **search smb_version**

[![reconocmiento4.jpg](https://i.postimg.cc/gkwsvYjQ/reconocmiento4.jpg)](https://postimg.cc/ppHKvMDJ)

Luego ejecutamos el comando --> **set rhost 192.168.1.15** <-- que seria la direccion IP de la maquina victima donde vamos a buscar el servicio, verificamos que se haya realizado el cambio con show options y procedemos a ejecutar con el comando exploit 💣 

[![reconocimiento5.jpg](https://i.postimg.cc/8CGbQM4g/reconocimiento5.jpg)](https://postimg.cc/YvXFQ4Td)

Ya una vez obtinida la version del samba vamos a buscar con el comando --> **searchsploit samba2.2** <-- el exploit que vamos a usar, en este caso usaremos uno que se encuentra en Metasploit que se llama  **trans2open**  para (**Linux x86**) 

### Resumen de la información obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                | Usuario       |
|:------------: |:-----------------:| :-------------------------------:| -------------:| 
| 192.168.1.15  | Red hat/Linux     | 22/tcp ssh OpenSSh 2.9p2         |  KIO-KID      | 
|               |                   | 80/tcp http Apache 1.3.20        |               | 
|               |                   | 111/tcp rpcbind                  |               | 
|               |                   | 139/tcp netbios-ssn Samba 2.2.1a |               |  
|               |                   | 443/tcp https Apache 1.3.20      |               |
|               |                   | 1024/tcp kdm                     |               |


## 2.Explotación

Ya con la recopilación obtenida procedemos abrir nuevamente el metasploit y buscamos el exploit  **trans2open** 

[![explotacion.jpg](https://i.postimg.cc/y6fBDT3n/explotacion.jpg)](https://postimg.cc/Rqn28KRn)

utilizamos el comando -->**use 1**<-- para selecionar el exploit, una vez seleccionado ejecutamos el comando --> **show options**<-- para verificar los parametros que nos hacen falta, colocamos la ip de la maquina victima con el comando -->**set rhost 192.168.1.15**

Ahora toca cambiar el payload con el comando -->**show payloads**<-- y te saldra un lista de todos los payloads disponibles, nosotros vamos utilizar el N° 33

[![explotacion2.jpg](https://i.postimg.cc/q7k4tDwb/explotacion2.jpg)](https://postimg.cc/YvyJZ3JY)



