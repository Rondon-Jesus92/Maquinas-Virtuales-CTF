## 1.Reconocmiento

Comenzamos realizando un escaneo general con nmap sobre la IP de la máquina víctima para ver que puertos tiene abiertos.

[![reconocimento.jpg](https://i.postimg.cc/jSDk2G1h/reconocimento.jpg)](https://postimg.cc/qNdGZZ4t)

Ejecutamos el siguiente comando para verificar si el servicio samba esta activo

[![reconocimiento2.jpg](https://i.postimg.cc/jjPTwW1v/reconocimiento2.jpg)](https://postimg.cc/DWvHRznb)

### Resumen de la información obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                | Usuario       |
| :------------ |:-----------------:| :-------------------------------:| -------------:| 
| 192.168.1.15  | Red hat/Linux     | 22/tcp ssh OpenSSh 2.9p2         |  KIO-KID      | 
|               |                   | 80/tcp http Apache 1.3.20        |               | 
|               |                   | 111/tcp rpcbind                  |               | 
|               |                   | 139/tcp netbios-ssn Samba 2.2.1a |               |  
|               |                   | 443/tcp https Apache 1.3.20      |               |
|               |                   | 1024/tcp kdm                     |               |


## 2. Análisis de Vulnerabilidades/Debilidades

