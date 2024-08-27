# Máquina Bolt

Dificultad --> Fácil-Medio 

Enlace de la maquina --> https://we.tl/t-AoP1Y9yq1w

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la máquina

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la máquina 💻 victima

[![reconocimiento.jpg](https://i.postimg.cc/hPT1jFBV/reconocimiento.jpg)](https://postimg.cc/tYRxSM7g)


Escaneando puertos y sus versiones utilizando el comando **nmap** con diferentes parámetros a la IP que conseguimos 🔎

[![reconocimiento2.jpg](https://i.postimg.cc/t4DPZpmn/reconocimiento2.jpg)](https://postimg.cc/fVtk1nsM)


[![reconocimiento3.jpg](https://i.postimg.cc/HW5y0Qhg/reconocimiento3.jpg)](https://postimg.cc/6TWqtGKj)


Convertimos el archivo que se nos genero para poder visualizarlo mejor con el siguiente comando **--> xsltproc bol.xml -o bol.html <--** y luego lo abrimos con el navegador


[![reconocimiento4.jpg](https://i.postimg.cc/90VFYqDm/reconocimiento4.jpg)](https://postimg.cc/2bcDzydM)


[![reconocimiento5.jpg](https://i.postimg.cc/MHDZPKBy/reconocimiento5.jpg)](https://postimg.cc/8f7VsGt5)


[![reconocimiento6.jpg](https://i.postimg.cc/bNywGkXL/reconocimiento6.jpg)](https://postimg.cc/gr7WQw26)



### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                   | 
|:------------: |:-----------------:| :----------------------------------:| 
| 192.168.1.22  | Debian            | 22 ssh 7.9p1 Debian 10+deb10u2      |
|               |                   | 80 http Apache 2.4.38               |
|               |                   | 111 rpcbind                         |
|               |                   | 2049 NFS                            |
|               |                   | 8080 http-proxy php 7.3.27.1~deb10u1|



## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos un fuzzing con la herramienta 🛠️ gobuster

[![analisis.jpg](https://i.postimg.cc/13F2s7Wd/analisis.jpg)](https://postimg.cc/YvpbxRBN)


Tenemos abierto el puerto 8080 realizamos igual un fuzzing pero agregando este puerto al final de la dirección IP

[![analisis3.jpg](https://i.postimg.cc/J0Bv0Trt/analisis3.jpg)](https://postimg.cc/CnSm6sfV)


Una vez realizado el fuzzing accedemos a uno de los directorios que nos consiguió **/app**, **buscamos en el archivo config-cache.json** y encontraremos una contraseña 🔑


[![analisis2.jpg](https://i.postimg.cc/3xZ6k0Tj/analisis2.jpg)](https://postimg.cc/w17FPv33)






