# Máquina Lazyadmin

Dificultad --> Facil

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Lazyadmin**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

![image](https://github.com/user-attachments/assets/9772f16c-8d54-4751-a4a6-b594ac12a008)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos


![image](https://github.com/user-attachments/assets/d7904d0a-75e5-4eec-afbe-8653c924cd47)


sudo nmap -sV -sC -T4 10.10.84.39 -vv


![image](https://github.com/user-attachments/assets/2dacba53-2249-42f5-896d-bab625520f2c)



### Resumen de la Información Obtenida

|IP           | Sistema OPerativo | Puertos/Servicios           | 
|:----------: |:-----------------:| :--------------------------:| 
| 10.10.84.39 |  Linux            | 22 OpenSSH 7.2p2            |
|             |                   | 80 http Apache httpd 2.4.18 |


## 2. Análisis de Vulnerabilidades / Debilidades 🔍


Realizamos un fuzzing


![image](https://github.com/user-attachments/assets/454e977a-a456-461c-bd37-03a147d6acbc)


![image](https://github.com/user-attachments/assets/49ef86b0-e938-49e3-917b-cae6508efe20)


![image](https://github.com/user-attachments/assets/c17b2cce-2511-42ae-9b0b-c8d14d318da3)


Revisando los directorios nos encontramos con el siguiente archivo el cual lo vamos a descargar en nuestra máquina


![image](https://github.com/user-attachments/assets/17579fc1-a5df-40b2-bb13-ba2027309d12)


Leemos el archivo con el comando cat y conseguimos un usuario y un hash


![image](https://github.com/user-attachments/assets/488d52c3-f1c2-49f2-aa2a-2a957e32bf5f)


Lo codificamos

![image](https://github.com/user-attachments/assets/0df25b4a-b2be-452d-99df-95ae1594751e)


Ahora vamos a utilizar el usuario manager y la contraseña que codificamos para iniciar en el portal de sweetRice


![image](https://github.com/user-attachments/assets/a82f9ff4-ed30-4076-8a11-cea65b70cb63)


## 3. Explotación 💥


Nos situamos en el apartado Ads en el cual vamos a subir un archivo llamado shell.php en el que copiaremos  una reverse shell colocando nuestra IP y el puerto que vamos a levantar por el netcat


![image](https://github.com/user-attachments/assets/7a285c79-006e-42cb-afca-5155cbb5faab)


Una vez subida nuestra shell activamos el netcat y procedemos a ejecutar


![image](https://github.com/user-attachments/assets/06a6d798-1d6c-41d9-b9e8-66f8d6169773)


Una vez conectados buscamos la primera bandera 🚩 que se encuentra en el directorio itguy

THM{63e5bce9271952aad1113b6f1ac28a07}

![image](https://github.com/user-attachments/assets/758da479-d9f6-4538-8f71-7808a903df6c)



## 4. Escalación de privilegios 🧗

Ahora procedemos a buscar escalar privilegios, encontramos un archivo que se llama backup.pl que nos podría ayudar a escalar privilegios

![image](https://github.com/user-attachments/assets/597ad037-5306-4e79-9378-0814d1b5e55a)

Mejoramos nuestra shell con los siguientes comandos


![image](https://github.com/user-attachments/assets/830f14af-ba14-4bfc-8862-84658648268f)


Ahora vamos a proceder darle permisos SUID a la bash para cuando se ejecute el archivo backup.pl nos de acceso root.


![image](https://github.com/user-attachments/assets/a3a85dcd-922c-4c0c-ac6a-7837e0cbb6cb)


Ahora procedemos a buscar el contenido de la bandera root.txt 🚩

THM{6637f41d0177b6f37cb20d775124699f}


![image](https://github.com/user-attachments/assets/30b6210b-3169-40f4-af9a-a845f7e38670)



## 5.Banderas 🏁

|user.txt | THM{63e5bce9271952aad1113b6f1ac28a07}  |
|:-------:|:--------------------------------------:|
|root.txt | THM{6637f41d0177b6f37cb20d775124699f}  |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 

- What is the user flag?

THM{63e5bce9271952aad1113b6f1ac28a07}

- What is the root flag?

THM{6637f41d0177b6f37cb20d775124699f}













































































































