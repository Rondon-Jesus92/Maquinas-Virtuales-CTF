# Maquina Eternal

**Dificultad:** Facil

**Enlace de la maquina** --> https://github.com/Rondon-Jesus92/Maquinas-Virtuales-CTF/blob/main/Descarga-de-Maquinas/eternal-VMware.zip

**Instrucciones** --> Se necesita instalar VMawre Workstation para poder virtualizar la maquina
'
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
'
## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la Maquina 💻 victima

[![reconocimiento.jpg](https://i.postimg.cc/RVGV6vBt/reconocimiento.jpg)](https://postimg.cc/1fnZbh4R)

Escaneando puertos y sus versiones utilizando el comando **nmap** con diferentes parámetros a la IP que conseguimos 🔎

[![reconocimiento2.jpg](https://i.postimg.cc/bJjs0LFS/reconocimiento2.jpg)](https://postimg.cc/2q2krdYC)


[![reconocimiento3.jpg](https://i.postimg.cc/SRgRh4Qc/reconocimiento3.jpg)](https://postimg.cc/2LBrQPZ5)

Convertimos el archivo que se nos genero para visualizarlo mejor con el siguiente comando **--> xsltproc blue-nmap-results.xml -o xsltproc blue-nmap-results.html <--** y luego lo abrimos con el navegador

[![reconocimiento4.jpg](https://i.postimg.cc/pL60d5c6/reconocimiento4.jpg)](https://postimg.cc/NKR8JFc6)

[![reconocimiento5.jpg](https://i.postimg.cc/76vm38k1/reconocimiento5.jpg)](https://postimg.cc/JyqN8Ftt)

### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                | 
|:------------: |:-----------------:| :-------------------------------:| 
| 192.168.1.17  | Windows 7 ultime  | 135/tcp msrpc                    |
|               |                   | 139/tcp netbios-ssn              |
|               |                   | 445/tcp microsoft-ds             |
|               |                   | 49152-49157/tcp microsoft-rpc    |


## 2. Análisis de Vulnerabilidades / Debilidades

Buscamos un exploit con el comando **--> searchsploit ms17-010 <--** la vulnerabilidad que conseguimos

[![analisis.jpg](https://i.postimg.cc/HWNjBpKC/analisis.jpg)](https://postimg.cc/G9kcp1dM)

## 3. Explotación

Abrimos metasploit con el comando **--> msfconsole <--**, luegos buscamos el exploit MS17-010 con el comando **--> search MS17-010 <--**
Se nos desplegara varias opciones la cual vamos a seleccionar es la N° 0 con el comando **--> use 0 <--**

[![explotacion.jpg](https://i.postimg.cc/HsCnfSpS/explotacion.jpg)](https://postimg.cc/s1w3W4GS)

Cambiamos el parametro RHOSTS por la IP de la maquina victima 💻, para luego proceder a ejecutar el exploit 💣

[![explotacion2.jpg](https://i.postimg.cc/P5SqvT1c/explotacion2.jpg)](https://postimg.cc/DSbhtVjd)

Ejecutamos el siguiente comando **--> hashdump <--**

[![explotacion3.jpg](https://i.postimg.cc/sXdCvNHq/explotacion3.jpg)](https://postimg.cc/670mPYXf)

Conseguimos unas credenciales encriptadas las cuales la podemos decifrar en diferentes paginas online como por ejemplo **crackstation**

[![explotacion4.jpg](https://i.postimg.cc/yxjMpY6C/explotacion4.jpg)](https://postimg.cc/XBZ1pWys)

Ya tenemos la contraseña del usuario **Hacker Mentor User** y del usuario **Hacker Mentor Admin**, podemos acceder a cada uno de los usuario y en el escritorio vamos a conseguir la **Bandera1** y la **Bandera2**

#### Accediendo al usuario Hacker Mentor User

[![bandera1.jpg](https://i.postimg.cc/W36Ctbkq/bandera1.jpg)](https://postimg.cc/vgT3NyXG)

#### Accediendo al usuario Hacker Mentor Admin

[![bandera2.jpg](https://i.postimg.cc/wMwrcfF6/bandera2.jpg)](https://postimg.cc/30D93ZP6)

## 4.Banderas

|Bandera 1 | 0ef3b7d488b11e3e800f547a0765da8e |
|:--------:|:--------------------------------:|
|Bandera 2 | A63c1c39c0c7fd570053343451667939 |

