# Máquina Monkey

Dificultad --> Fácil-Medio 

Enlace de la maquina --> https://we.tl/t-RyoGIDKsOI

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la máquina

'

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la máquina 💻 victima

[![reconocimiento.jpg](https://i.postimg.cc/4drQ4Fkn/reconocimiento.jpg)](https://postimg.cc/gXHZNs0P)


Escaneando puertos y sus versiones utilizando el comando **nmap** con diferentes parámetros a la IP que conseguimos 🔎


[![reconocimiento2.jpg](https://i.postimg.cc/MTFqnVdn/reconocimiento2.jpg)](https://postimg.cc/sQ7t8QFz)


[![reconocimiento3.jpg](https://i.postimg.cc/bwprc0MP/reconocimiento3.jpg)](https://postimg.cc/CzrSb8Sr)


Convertimos el archivo que se nos genero para visualizarlo mejor con el siguiente comando **--> xsltproc nmap-results.xml -o nmap-results.html <--** y luego lo abrimos con el navegador


[![reconocimiento4.jpg](https://i.postimg.cc/wTxZC7VP/reconocimiento4.jpg)](https://postimg.cc/ThStm2Bq)


Nos podemos fijar que hay un login por ftp de manera anónima y que tenemos un archivo de texto llamado notas.txt


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                          | 
|:------------: |:-----------------:| :-----------------------------------------:| 
| 192.168.1.18  | Debian            | 21/tcp ftp vsftpd 3.0.3                    |
|               |                   | 22/tcp ssh openSSH 7.9p1 Debian 10+deb10u2 |
|               |                   | 80/tcp apache httpd 2.4.38                 |


