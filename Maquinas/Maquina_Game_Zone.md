# Máquina Steel Mountain

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Steel Mountain**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos


![image](https://github.com/user-attachments/assets/33ca4154-cca0-427d-9d77-4a52a79e26ed)


Ya obtenidos los puertos abiertos buscamos sus versiones y vulnerabilidades utilizando la herramienta Nmap con diferentes parámetros.


![image](https://github.com/user-attachments/assets/328f8dde-0a61-47a6-83d8-43cc161f7935)


Conertimos el archivo PC.xml a HTML para poder visualizarlo mejor.


![image](https://github.com/user-attachments/assets/74791144-36b1-4f1c-b372-5498d992b352)



![image](https://github.com/user-attachments/assets/d9b3bfc2-4b7f-4851-93da-486bfa457ec1)



Lo convertimos a formato HTML para poder visualizarlo mejor


![image](https://github.com/user-attachments/assets/18bd259c-0c83-4df7-beeb-87d81519e6e7)



### Resumen de la Información Obtenida

|IP          | Sistema OPerativo    | Puertos/Servicios              | 
|:---------: |:--------------------:| :-----------------------------:| 
| 10.10.1.29 | Ubuntu Linux 16.04.6 | 22 ssh 7.2p2 Ubuntu 4ubuntu2.7 |
|            |                      | 80 http Apache httpd 2.4.18    |


## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos un Fuzzing


![image](https://github.com/user-attachments/assets/99acaa09-8ba0-4f9f-9078-308f77013e0f)



                                hacemos mysql injection en login ' o 1=1 -- -

![image](https://github.com/user-attachments/assets/81597124-439e-4777-837e-222c80629bc6)



Logramos entrar


![image](https://github.com/user-attachments/assets/03ee549c-c53b-43f0-baff-e93f129387a0)


Aplicamos la siguiente consulta mysql para obtener el Hostname y su versión: **' UNION SELECT NULL, @@HOSTNAME, @@VERSION#**


![image](https://github.com/user-attachments/assets/f2d6ebf4-420e-478c-820a-5c58ffb8b25f)
