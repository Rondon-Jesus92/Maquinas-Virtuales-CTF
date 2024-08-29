# Máquina Navigator

Dificultad --> Fácil-Medio 

Enlace de la máquina --> https://we.tl/t-aFtDaZqZIZ

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la máquina

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Utilizamos el comando **--> sudo arp-scan -l <--** para encontrar la dirección IP de la máquina 💻 victima

[![reconocimiento.jpg](https://i.postimg.cc/YjcN4DjD/reconocimiento.jpg)](https://postimg.cc/MfDfND71)


Buscando los puertos abiertos con el comando sudo nmap -sS --min-rate 6000 -p- -vvv 192.168.1.23


[![reconocimiento2.jpg](https://i.postimg.cc/bJsFkMNq/reconocimiento2.jpg)](https://postimg.cc/2bNwf0Zt)


Buscamos sus versiones


[![reconocimiento3.jpg](https://i.postimg.cc/vT7jZnZn/reconocimiento3.jpg)](https://postimg.cc/wRvFW7Jx)


Convertimos el archivo que se nos genero para poder visualizarlo mejor con el siguiente comando **--> xsltproc pc.xml -o pc.html <--** y luego lo abrimos con el navegador


[![reconocimiento4.jpg](https://i.postimg.cc/rwckL8g3/reconocimiento4.jpg)](https://postimg.cc/vcPjrwJt)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                   | 
|:------------: |:-----------------:| :----------------------------------:| 
| 192.168.1.23  | Debian            | 22 ssh 7.9p1 Debian 10+deb10u2      |
|               |                   | 80 http nginx 1.14.2                |
|               |                   | 53 domain 9.11.5-P4-5.1+deb10u5     |



## 2. Análisis de Vulnerabilidades / Debilidades

Ya que al consultar la dirección ip de la maquina no nos resuelve ningún dns, vamos a proceder a utilizar la herramienta dnsrecon para comprobar 

[![analisis.jpg](https://i.postimg.cc/PJQkC952/analisis.jpg)](https://postimg.cc/RJhY8PfH)


Ya que conseguimos que si hay un dns vamos a editar 📝 el siguiente archivo para que este dns se resuelva internamente en nuestra máquina 🖥️ con el siguiente comando --> **sudo pluma /etc/hosts** <--

Ahora en el archivo vamos agregar 192.168.0.14 (un espacio el cual nos vamos a copiar exactamente del que esta arriba ) y navigator.hm, aquí lo que queremos decir es si preguntan por navigator.hm me la vas a resolver a la dirección 192.168.0.14

[![analisis2.jpg](https://i.postimg.cc/1X9jjtyr/analisis2.jpg)](https://postimg.cc/sQNmB3QM)

