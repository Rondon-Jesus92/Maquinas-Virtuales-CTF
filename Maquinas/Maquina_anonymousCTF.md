# Máquina Anonymous CTF

Dificultad --> Media

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Anonymous CTF**

'

![image](https://github.com/user-attachments/assets/be3b38be-721b-48e2-9c93-85482c5d762e)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos

    sudo nmap 10.10.73.210 -T4 


![image](https://github.com/user-attachments/assets/3b7fcb58-c23b-4b6f-b66f-22b8c2c9740c)


    sudo nmap -sV -Pn -T4 10.10.73.210


![image](https://github.com/user-attachments/assets/20997626-be8b-485b-9519-dbb06d71ad1f)



### Resumen de la Información Obtenida

|IP            | Sistema OPerativo | Puertos/Servicios              | 
|:-----------: |:-----------------:| :-----------------------------:| 
| 10.10.73.210 |  Linux            | 21 ftp vsftpd 2.0.8            |
|              |                   | 22 ssh OpenSSh 7.6p1           |
|              |                   | 139 netbios-ssn samba smbd 3.x |
|              |                   | 445 netbios-ssn samba smbd 3.x |


## 2. Análisis de Vulnerabilidades / Debilidades 🔍

Como tiene disponible conectar por ftp en modo anonymous , nos conectamos para ver que encontramos 


![image](https://github.com/user-attachments/assets/cc6b935b-86d0-47f6-9da4-1343ed8b5994)


Accedemos al directorio scripts en el cual vamos a conseguir 3 archivos


![image](https://github.com/user-attachments/assets/db0187f6-8b7a-4cbf-b932-7eefb38d54f1)


Nos descargamos los 3 archivos 


![image](https://github.com/user-attachments/assets/26b32ad7-fc2f-4a98-a154-aec70f59a58d)


## 3. Explotación 💥


Abrimos con nano el archivo clean.sh borramos su contenido y copiamos nuestra reverse shell


![image](https://github.com/user-attachments/assets/89176c82-f5e7-4211-9ec6-de54acf82510)


![image](https://github.com/user-attachments/assets/1b7655d5-e9e8-4f5b-bb24-f0e0f1646103)



Ejecutamos el siguiente comando para reemplazar el archivo [clean.sh](http://clean.sh) con el que modificamos

    put clean.sh

![image](https://github.com/user-attachments/assets/c0a8977e-5561-4d1d-92a1-ffcef4b06fa2)


Nos colocamos a la escucha en nuestra máquina esperamos un momento a que se conecte


![image](https://github.com/user-attachments/assets/cace5543-dccf-47c0-9150-e924e1de0f75)



Hacemos un ls y logramos encontrar la primera bandera 

90d6f992585815ff991e68748c414740


![image](https://github.com/user-attachments/assets/ed05e5b8-0e5e-4d93-a8f5-d2f18d25151b)


Mejoramos nuestra shell para hacerla mas dinámica


![image](https://github.com/user-attachments/assets/b84aade0-0b1b-4d3c-91a5-9c55a3f66931)


## 4. Escalación de privilegios 🧗


Descargamos el linpeas en nuestra máquina para luego descargarlo en la máquina victima y le damos permisos de ejecución 


![image](https://github.com/user-attachments/assets/923e1f89-77d3-47e0-a7b7-c6331d07f50f)


Procedemos a ejecutar el linpeas


![image](https://github.com/user-attachments/assets/38d70a53-35d1-4ef8-887a-a390db7a187e)


Encontramos que podemos escalar privilegios a través de un binario


![image](https://github.com/user-attachments/assets/874075a2-7ee9-4f33-b697-03ae3362ac8d)


Buscamos como escalar privilegios 

![image](https://github.com/user-attachments/assets/cde9d1ca-8aba-4e8e-abae-bb44573196e7)


Y ejecutamos el siguiente comando logrando escalar privilegios

    /usr/bin/env /bin/sh -p


![image](https://github.com/user-attachments/assets/5ef1eeda-8637-44da-9aad-28824e756c0a)


Procedemos a buscar la ultima bandera en el directorio root

4d930091c31a622a7ed10f27999af363


![image](https://github.com/user-attachments/assets/5cab5ee9-9ee5-4c14-80d8-4cf3983f0992)


## 5.Banderas 🏁

|user.txt | 90d6f992585815ff991e68748c414740  |
|:-------:|:---------------------------------:|
|root.txt | 4d930091c31a622a7ed10f27999af363  |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 


- Enumerate the machine.  How many ports are open?

4

- What service is running on port 21?

ftp

- What service is running on ports 139 and 445?

smb

- There's a share on the user's computer.  What's it called?

pics

- user.txt

90d6f992585815ff991e68748c414740

- root.txt

4d930091c31a622a7ed10f27999af363









































