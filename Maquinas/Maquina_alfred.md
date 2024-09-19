# Máquina Game Zone

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Alfred**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos
abiertos

    sudo nmap -Pn --min-rate=6000 -vvvvvv 10.10.252.112


![image](https://github.com/user-attachments/assets/a2f7f09b-4053-4ecf-ba61-12a1bf419c57)


Ya obtenidos los puertos abiertos buscamos sus versiones y vulnerabilidades utilizando
la herramienta Nmap con diferentes parámetros.


![image](https://github.com/user-attachments/assets/9d644a1b-b3f2-45d1-aae0-e045a617e809)


Conertimos el archivo PC.xml a HTML para poder visualizarlo mejor.

    xsltproc PC.xml -o pc.html

![image](https://github.com/user-attachments/assets/47b8d602-e009-4637-8497-d4cd403741ca)

'

![image](https://github.com/user-attachments/assets/315e37aa-0493-436a-97af-ce606096c0a1)


Convertimos el archivo pv.xml a html para poder visualizarlo mejor

    xsltproc pv.xml -o pv.html


![image](https://github.com/user-attachments/assets/ef895e61-d8d1-420b-9787-ce53942bae21)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo    | Puertos/Servicios         | 
|:------------: |:--------------------:| :------------------------:| 
| 10.10.252.112 | Windows 7 Ultimate   | 80 http Microsoft IIS 7.5 |
|               |                      | 3389 ms-wbt-server        |
|               |                      | 8080 http Jetty 9.4.z     |


## 2. Análisis de Vulnerabilidades / Debilidades


Como tenemos servicio http revisamos la dirreción IP con los puertos que conseguimos abiertos

### 10.10.252.112

![image](https://github.com/user-attachments/assets/aa5972f1-780a-4754-af42-1657062b13c0)

### 10.10.252.112/8080


![image](https://github.com/user-attachments/assets/4e34d884-dcd2-4c09-9511-bca4d5d47741)


Utilizando las contraseñas admin admin logramos acceder al panel de control

![image](https://github.com/user-attachments/assets/e17943b3-ad07-4ef5-9680-1cba6acbe1a1)


## 3. Explotación 💥

### Manual

Descargamos el script de power shell Invoke-PowerShellTcp.ps1 de la pagina github 

![image](https://github.com/user-attachments/assets/6e3fddc7-adb0-465d-aa17-dd4b60fe99d2)


Ahora vamos a panel de control de Jenkins y creamos un nuevo proyecto que lo llamaremos acceso


![image](https://github.com/user-attachments/assets/9cc1ee40-9702-473a-a0c3-b24e1805dab8)


En configuraciones en la sección de ejecutar elegimos " ejecutar un comando de Windows " y colocamos el siguiente script donde especificamos nuestra dirección ip donde va descargar la power shell que tenemos montada en nuestro servidor y el netcat que vamos activar con el puerto 9001


![image](https://github.com/user-attachments/assets/e28f13cb-3597-40d7-b60d-a8cf8ec03ad8)


Una vez guardada la configuración activamos nuestro netcat y procedemos a ejecutar nuestra tarea acceso que ejecutara por atrás nuestro script


![image](https://github.com/user-attachments/assets/24977134-c10c-4543-abb0-8deab1ae1dbe)


Logramos acceder


![image](https://github.com/user-attachments/assets/dd747d2b-1fb7-45eb-8509-db391a175a8b)


Buscando en el usuario bruce conseguimos el contenido del user.txt:
79007a09481963edf2e1321abd9ae2a0

![image](https://github.com/user-attachments/assets/9005c5c6-0739-40c1-bc9a-ca2213c37eeb)


### Automatizada:

Ahora creamos con msfvenom nuestro archivo para realizar la shell reverse


![image](https://github.com/user-attachments/assets/95f2497b-9cac-4e73-8ea9-7883bfacb5e6)


Accedemos a metasploit seleccionamos el exploit y el payload que vamos a utilizar


![image](https://github.com/user-attachments/assets/04e8da84-5f59-42f9-a2f5-2176541187cb)


Modificamos el parámetro de LHOST por nuestra IP y el LPORT por el puerto que elegimos cuando creamos el archivo para la shell reverse que seria el 10000


![image](https://github.com/user-attachments/assets/7be762ba-5714-470e-9bbf-f6ba63c6b37c)


Logramos Acceder


![image](https://github.com/user-attachments/assets/fafb51d7-686b-41fc-a2a7-0558e4a3f991)


## 4. Escalación de privilegios


Con el siguiente comando podemos visualizar todos privilegios que tenemos sobre la maquina con el usuario actual

    SeImpersonatePrivilege


![image](https://github.com/user-attachments/assets/d6397fb9-fd72-488a-85cc-dabd80dabd2d)


Cargamos el modo incognito con load incognito y luego listamos los tokens con list_tokens -g


![image](https://github.com/user-attachments/assets/9c66ec21-1a2e-40fb-a003-33512454a1c2)


Ahora nos hacemos pasar por el token BUILTIN\Administrators con el siguiente comando


![image](https://github.com/user-attachments/assets/8e5b573b-addc-4433-bc80-566d75f5a498)


Confirmamos con el comando getuid que tenemos privilegios de administrador


![image](https://github.com/user-attachments/assets/ec8a66c7-31be-40a0-b8bf-988f479705b4)


Vamos a emigrar a un proceso mas seguro para tener una mejor estabilidad en nuestra escalada de privilegio, ejecutamos el comando ps para enlistar los procesos

![image](https://github.com/user-attachments/assets/922ab0b2-4e8c-4311-b80f-f7936b177dae)


Con el comando migrate 668 emigramos al proceso services.exe y procedemos a buscar el archivo root.txt


![image](https://github.com/user-attachments/assets/a31561a7-bb81-4dff-9854-8938d2d961aa)


Obtenemos el contenido del archivo root.txt: dff0f748678f280250f25a45b8046b4a


![image](https://github.com/user-attachments/assets/eee3366f-300d-47be-9d2a-39e3088898cc)


## 5.Banderas 🏁

|user.txt | 79007a09481963edf2e1321abd9ae2a0 |
|:-------:|:--------------------------------:|
|root.txt | dff0f748678f280250f25a45b8046b4a |


## 6.Extra 🚨


• ¿Cuántos puertos están abiertos? (TCP solamente)

3

• ¿Cuál es el nombre de usuario y la contraseña para el panel de inicio de sesión? (en el formato nombre de usuario:contraseña)

admin:admin

•¿Qué es la bandera user.txt?

79007a09481963edf2e1321abd9ae2a0

•¿Cuál es el tamaño final de la carga útil exe que generó?

73802

•Cuál es la salida cuando ejecutas el getúido comando?

NT AUTHORITY\SYSTEM

•
Lea el archivo root.txt ubicado en C:\Windows\System32\config

dff0f748678f280250f25a45b8046b4a






