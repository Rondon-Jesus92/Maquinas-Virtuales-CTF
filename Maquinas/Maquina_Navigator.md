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
