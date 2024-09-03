# Máquina Robot

Dificultad --> Medio

Enlace de la maquina --> https://drive.google.com/file/d/17aUpHkn3ZuuOGcksA_U7_-Xp4ImOCojx/view

Instrucciones --> Se necesita instalar VMawre Workstation para poder virtualizar la maquina

'

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Buscando la IP de la máquina con sudo arp-acan -l

![image](https://github.com/user-attachments/assets/a5427343-b0f5-465a-882c-fc01d2de39f8)


Buscando los puertos abierto utilizando Nmap:

    sudo nmap -p- -T4 -sS -vvv 192.168.1.13 | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//

![image](https://github.com/user-attachments/assets/c0d70f3e-ed56-4201-ae83-9b7c841883a4)


Buscando vulnerabilidades a los puertos abiertos con Nmap


![image](https://github.com/user-attachments/assets/1b8afef8-a49c-49ed-8d18-2d50814b06e1)


Convirtiendo el archivo pc.xml a html para poder visualizarlo mejor




![image](https://github.com/user-attachments/assets/9eac354b-eb6d-437f-929c-f5f9babc37f7)


![image](https://github.com/user-attachments/assets/703e56e8-1f08-44dc-993f-998475d3e9a4)


![image](https://github.com/user-attachments/assets/3ec563b6-b595-497a-b658-a147c82083b5)


Convirtiendo el archivo pv.xml a html para poder visualizarlo mejor


![image](https://github.com/user-attachments/assets/c97df58e-702f-4afa-bfb5-d324e6ca9b90)


![image](https://github.com/user-attachments/assets/e83574c7-a1ed-4995-acc6-2d918d64a019)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios | 
|:------------: |:-----------------:| :----------------:| 
| 192.168.1.13  | Ubuntu 14.04.2 LTS| 80 http Apache    |
|               |                   | 443 http Apache   |










































