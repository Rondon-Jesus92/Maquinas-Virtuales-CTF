# Máquina ColddBox

Dificultad --> Facil

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Kiba**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'


![image](https://github.com/user-attachments/assets/8e8aeb9a-ac09-480f-b75b-56615c41323d)

## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos

    sudo nmap -p1-10000 -sC -sV -T4 10.10.25.0 -vv

![image](https://github.com/user-attachments/assets/eee7e445-138b-44bc-aa95-ad3672ae3b54)


![image](https://github.com/user-attachments/assets/c1c5954a-a42e-41cc-bd7f-683aecb18f36)


### Resumen de la Información Obtenida

|IP           | Sistema OPerativo | Puertos/Servicios           | 
|:----------: |:-----------------:| :--------------------------:| 
|10.10.25.0   | Ubuntu / Linux    | 22 ssh OpenSSH 7.2p2        |
|             |                   | 80 http Apache httpd 2.4.18 |
|             |                   | 5601 esmagent kibana 6.5.4  |



## 2. Análisis de Vulnerabilidades / Debilidades 🔍

Al tener abierto el puerto 80 revisamos la dirección ip


![image](https://github.com/user-attachments/assets/435501e3-168f-40c8-b4cf-fc027d496223)


De igual forma abrimos con el puerto 5601 el cual encontramos que esta corriendo un servicio


![image](https://github.com/user-attachments/assets/9146d585-5de3-4669-b443-162c1fa88230)


Revisando la pagina encontramos que se esta corriendo kibana 6.5.4 ahora buscaremos por internet si existe una vulnerabilidad . En github conseguimos como aprovecharnos de esta versión vamos a clonar el repositorio y seguir los pasos que nos indican


![image](https://github.com/user-attachments/assets/61224c6e-0c03-487a-9525-8b324c95da99)



## 3. Explotación 💥

Clonamos el repositorio

![image](https://github.com/user-attachments/assets/976bc9ca-d197-49dc-a970-fa68849758eb)


Ejecutamos python3 cve-2019-7609.py y nos indica la sintaxis correcta para ejecutarlo


![image](https://github.com/user-attachments/assets/1025e690-5cd0-40f7-90a8-540dd9aa1205)


le damos permisos de ejecución , colocamos nuestra maquina a la escucha y procedemos a ejecutarlo


![image](https://github.com/user-attachments/assets/740ed9c7-f547-48ec-ae11-82325de0212a)


Como pueden ver tenemos acceso

![image](https://github.com/user-attachments/assets/ca016197-ff1e-4200-b30e-61b1ea36c3b4)


Ahora procedemos a buscar la primera bandera 🚩

THM{1s_easy_pwn3d_k1bana_w1th_rce}

![image](https://github.com/user-attachments/assets/67e7bf9e-bf94-463e-8ed9-7e2888b62f23)



## 4. Escalación de privilegios 🧗

En tryhackme nos indica que podemos escalar privilegios a través de las capabilities, la cual la vamos a enumerar con el siguiente comando

    getcap -r / 2>/dev/null


![image](https://github.com/user-attachments/assets/97dde912-e883-44c9-ad3b-6f7a01026a78)



Nos vamos aprovechar del binario python3, buscamos en la pagina gtobins


![image](https://github.com/user-attachments/assets/7c89d9ec-df36-4977-8def-a6c3a9a7b598)



Copiamos la ruta de python mas el comando que nos indican en la pagina logrando tener acceso como root

    /home/kiba/.hackmeplease/python3 -c 'import os; os.setuid(0); os.system("/bin/sh")'


![image](https://github.com/user-attachments/assets/1fcac518-3186-441c-978e-d868a98c4f94)


Nos vamos al directorio root donde vamos a conseguir la ultima bandera

THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es}


![image](https://github.com/user-attachments/assets/d307fa6c-bd10-42e1-b62d-f5308fd34323)



## 5.Banderas 🏁

|user.txt | THM{1s_easy_pwn3d_k1bana_w1th_rce}           |
|:-------:|:--------------------------------------------:|
|root.txt | THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es} |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 


- What is the vulnerability that is specific to programming languages with prototype-based inheritance?

Prototype pollution

- What is the version of visualization dashboard installed in the server?

6.5.4

- What is the CVE number for this vulnerability? This will be in the format: CVE-0000-0000

CVE-2019-7609

- Compromise the machine and locate user.txt

THM{1s_easy_pwn3d_k1bana_w1th_rce}

- How would you recursively list all of these capabilities?

getcap -r /

- Escalate privileges and obtain root.txt

THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es}



















































































































