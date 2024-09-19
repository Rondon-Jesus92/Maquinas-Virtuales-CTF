# Máquina Bolt

Dificultad --> Fácil-Medio 

Enlace de la máquina --> https://we.tl/t-AoP1Y9yq1w

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


Una vez realizado el fuzzing accedemos a uno de los directorios que nos consiguió **192.168.1.22/app**, **buscamos en el archivo config-cache.json** y encontraremos una contraseña 🔑


[![analisis2.jpg](https://i.postimg.cc/3xZ6k0Tj/analisis2.jpg)](https://postimg.cc/w17FPv33)



## 3. Explotación

El otro directorio que vamos abrir es el **192.168.1.22:8080/dev** y encontramos que hay una pagina web en **Boltwire**, podemos buscar haber si encontramos una vulnerabilidad para **Boltwire**


[![explotacion.jpg](https://i.postimg.cc/7PJmkfYL/explotacion.jpg)](https://postimg.cc/jCKz6qfV)


Buscando la vulnerabilidad en la pagina exploit db


[![explotacion2.jpg](https://i.postimg.cc/CKZfqn2B/explotacion2.jpg)](https://postimg.cc/DWTZV0nn)


Abrimos en el navegador la vulnerabilidad que conseguimos del directorio /index.php?p=action.search&action=../../../../../../../etc/passwd esto lo vamos agregar después del directorio /dev/

Conseguimos un usuario llamado jeanpaul

[![explotacion3.jpg](https://i.postimg.cc/VN6nYV18/explotacion3.jpg)](https://postimg.cc/5Xh6gp67)


Creamos una carpeta que se llame montaje, nos situamos en ella y procedemos a realizar un montaje de tipo NFS


[![explotacion4.jpg](https://i.postimg.cc/xCZkTf5G/explotacion4.jpg)](https://postimg.cc/S2WQgpwj)


Se crackea utilizando fuerza bruta en el archivo save.zip con la ayuda de la herramienta🛠️ fcrackzip


[![explotacion5.jpg](https://i.postimg.cc/c1QgNSY3/explotacion5.jpg)](https://postimg.cc/V5kLj2Ks)


Con la credencial🔑 obtenida procedemos a descomprimir el archivo save.zip, nos genera 3 archivos, bandera1🚩, id_rsa y todo.txt

[![explotacion6.jpg](https://i.postimg.cc/NMvsNKdz/explotacion6.jpg)](https://postimg.cc/zLpYvGZC)


Vemos el contenido de la bandera1 🚩

[![bandera1.jpg](https://i.postimg.cc/tgWyVX9t/bandera1.jpg)](https://postimg.cc/7CHjFrGf)


Ahora con el archivo id_rsa que se nos descargo le vamos a dar permisos de ejecución y procedemos a ejecutarlo


[![explotacion7.jpg](https://i.postimg.cc/nhQf6H9b/explotacion7.jpg)](https://postimg.cc/FYm6SXxD)


 Nos logramos conectar al servidor, hacemos un ls y tenemos la bandera2 🚩

 [![bandera2.jpg](https://i.postimg.cc/N07ScLgV/bandera2.jpg)](https://postimg.cc/bGr30ySH)


 Necesitamos saber el sistema operativo y su versión para ejecutar la herramienta Linpeas para escalar privilegios 


[![escalar.jpg](https://i.postimg.cc/Mpt3PNd2/escalar.jpg)](https://postimg.cc/hXz0hCkp)


Ahora nos situamos en nuestra máquina 🖥️ en el directorio donde tenemos el Linpeas descargado para la versión Linux_amd64 y colocamos nuestro equipo como servidor para realizar la descarga en el equipo victima 💻


[![escalar2.jpg](https://i.postimg.cc/8PzZFtTZ/escalar2.jpg)](https://postimg.cc/WtxGCmFZ)


Le damos permiso de ejecución y lo ejecutamos


[![escalar3.jpg](https://i.postimg.cc/4xrPSGV0/escalar3.jpg)](https://postimg.cc/rDN5DvhG)


Encontramos que para ejecutar el zip no necesita password


[![escalar4.jpg](https://i.postimg.cc/9FkYD28f/escalar4.jpg)](https://postimg.cc/jLz71prV)


## 4. Escalación de privilegios

Procedemos a ejecutar el siguiente comando


[![escalar5.jpg](https://i.postimg.cc/8C4FrBpb/escalar5.jpg)](https://postimg.cc/944X69q4)


Extraemos el archivo root.zip


[![escalar6.jpg](https://i.postimg.cc/sDfV0QrM/escalar6.jpg)](https://postimg.cc/6TSJ8QTK)


Vamos a la pagina https://gtfobins.github.io/gtfobins/zip/#sudo, buscamos zip y ejecutamos los comando que nos indica para poder escalar privilegios como root. Una vez como root accedemos a /root y tenemos la bandera3 🚩


[![bandera3.jpg](https://i.postimg.cc/gjQNRLyg/bandera3.jpg)](https://postimg.cc/FkbyXRBS)


## 5.Banderas 🏁

|Bandera 1 | aa7153d8889e1efd2bd57dab46e528e5 |
|:--------:|:--------------------------------:|
|Bandera 2 | 2d1b15dceeaf04a2a6314135f845dee77|
|Bandera 3 | 3c14d6f8ee4c66f8c4d9569b3101605a |


## 6.Extra 🚨


### Persistencia


Generamos llaves Id_rsa o llaves ssh


[![persistencia.jpg](https://i.postimg.cc/QC2JVGsP/persistencia.jpg)](https://postimg.cc/dZ9ySfhm)


Descargamos de nuestra máquina hacia la maquina victima el archivo id_ed25519.pub y la renombramos como authorized_keys


[![persistencia2.jpg](https://i.postimg.cc/7PdFS5Lv/persistencia2.jpg)](https://postimg.cc/G9PgChYz)


Procedemos a conectarnos con nuestra llave 🔑 y tenemos privilegios root


[![persistencia3.jpg](https://i.postimg.cc/fRh1g1XB/persistencia3.jpg)](https://postimg.cc/ykvfDQLZ)


