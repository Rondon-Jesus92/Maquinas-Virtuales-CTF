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


[![reconocimiento4.jpg](https://i.postimg.cc/rwckL8g3/reconocimiento4.jpg)](https://postimg.cc/vcPjrwJt)


### Resumen de la Información Obtenida

|IP             | Sistema OPerativo | Puertos/Servicios                   | 
|:------------: |:-----------------:| :----------------------------------:| 
| 192.168.1.23  | Debian            | 22 ssh 7.9p1 Debian 10+deb10u2      |
|               |                   | 80 http nginx 1.14.2                |
|               |                   | 53 domain 9.11.5-P4-5.1+deb10u5     |



## 2. Análisis de Vulnerabilidades / Debilidades

Ya que al consultar la dirección ip de la maquina no nos resuelve ningún dns, vamos a proceder a utilizar la herramienta dnsrecon para comprobar 

[![analisis.jpg](https://i.postimg.cc/PJQkC952/analisis.jpg)](https://postimg.cc/RJhY8PfH)


Ya que conseguimos que si hay un dns vamos a editar 📝 el siguiente archivo para que este dns se resuelva internamente en nuestra máquina 🖥️ con el siguiente comando --> **sudo pluma /etc/hosts** <--

Ahora en el archivo vamos agregar 192.168.0.14 (un espacio el cual nos vamos a copiar exactamente del que esta arriba ) y navigator.hm, aquí lo que queremos decir es si preguntan por navigator.hm me la vas a resolver a la dirección 192.168.0.14

[![analisis2.jpg](https://i.postimg.cc/N0YqWG3p/analisis2.jpg)](https://postimg.cc/mPd5942H)

Procedemos a realizar un fuzzing a la dirección navigator.hm

[![analisis3.jpg](https://i.postimg.cc/pTXjchmZ/analisis3.jpg)](https://postimg.cc/v1kDcmLg)


[![analisis4.jpg](https://i.postimg.cc/d0v1V6Km/analisis4.jpg)](https://postimg.cc/4YLZ8zym)


## 3. Explotación

Abrimos Metasploit con el comando -->**msfconsole**<-- y buscamos un exploit para la versión Navigate CMS v2.8

[![explotacion.jpg](https://i.postimg.cc/BnfYtL7B/explotacion.jpg)](https://postimg.cc/8jwB05S7)


En este caso vamos a utilizar la opción n°3 y le damos show options para ver que parámetro nos faltan modificar


[![explotacion2.jpg](https://i.postimg.cc/L8r1mLym/explotacion2.jpg)](https://postimg.cc/qNxRLhVF)


Modificamos el RHOTS, en este caso no se coloca la dirección IP sino el dns que seria navigator.hm ya que lo estamos resolviendo de forma interna y procedemos a darle run para ejecutarlo.


[![explotacion3.jpg](https://i.postimg.cc/2y8q0w57/explotacion3.jpg)](https://postimg.cc/TLFYwngK)


Ahora solo por comodidad de nosotros y para tener un Shell más dinámica haremos lo siguiente para mejorarla:

•	Procedemos a buscar en la página de reverse Shell el código que vamos a ejecutar para obtener nuestra shell reverse con la opción Bash -i.

•	Colocamos nuestra máquina en forma de escucha con el comando -->**nc -lvnp 9001**<--.

•	En la sesión de meterpreter colocaremos los siguientes comandos -->**shel**<-- y luego -->**bash -i**<--.

•	Por ultimo copiamos el código que nos indica en la página de reverse shell


[![explotacion4.jpg](https://i.postimg.cc/P53BJb9v/explotacion4.jpg)](https://postimg.cc/PCD6FDtd)

En nuestro equipo que levantamos el netcat y estamos en modo escucha se nos conectara, procedemos a ejecutar --> **Bash -i**<-- y luego -->**Ctrl+Z**<-- para suspender y que pase a segundo plano

En otra ventana ejecutaremos el siguiente comando ** stty raw -echo; fg
El comando stty raw -echo; fg en Unix/Linux se utiliza para cambiar la configuración del terminal y luego devolver el control a un proceso en primer plano. Aquí está el desglose de lo que hace cada parte:

1.	stty raw -echo:

Es un comando utilizado para cambiar y configurar las características del terminal.
o	raw pone el terminal en modo raw (bruto), lo que significa que los caracteres se envían al proceso de inmediato sin ser procesados o modificados (sin buffering ni conversión de línea). Esto puede ser útil para aplicaciones que necesitan un control completo sobre la entrada del usuario.
o	-echo desactiva el eco de la entrada del teclado, lo que significa que lo que escribas no se mostrará en la pantalla.

2.	fg:

Es un comando que se utiliza para traer al primer plano un proceso que está en segundo plano. Si tienes un proceso suspendido (por ejemplo, con Ctrl+Z) o ejecutándose en segundo plano, fg lo reanuda en el primer plano para que puedas interactuar con él.
En conjunto, el comando stty raw -echo; fg se usa para configurar el terminal para recibir la entrada del usuario en modo bruto y sin mostrar lo que se escribe, y luego reanudar un proceso que estaba en segundo plano, permitiendo que interactúes con él. Esto podría ser útil en situaciones como la implementación de interfaces de usuario basadas en texto o en la depuración de programas que requieren una entrada precisa.

Ya una vez explicado de que trata este comando, ejecutamos los siguientes comando que se puede apreciar en la imagen para terminar de configurar nuestra Shell reverse mas interactiva 


[![explotacion5.jpg](https://i.postimg.cc/nzJzSt2J/explotacion5.jpg)](https://postimg.cc/d75wDpgH)


buscamos información en la carpeta cfg

[![explotacion6.jpg](https://i.postimg.cc/7PX69dTF/explotacion6.jpg)](https://postimg.cc/Z0B4KfHH)


Ejecutamos un cat al archivo globals.php, conseguimos un usuario y contraseña 

[![explotacion7.jpg](https://i.postimg.cc/QxtMymQH/explotacion7.jpg)](https://postimg.cc/5jh1HwkV)


Nos conectamos mediante ssh con el usuario y contraseña obtenidos


[![explotacion8.jpg](https://i.postimg.cc/v8SycsnZ/explotacion8.jpg)](https://postimg.cc/Sj83vHVB)


Obtenemos el contenido de la bandera 1 🚩

[![bandera1.jpg](https://i.postimg.cc/gkCPHHN9/bandera1.jpg)](https://postimg.cc/Ln37Hfcv)


## 4. Escalación de privilegios


Ejecutamos el siguiente comando y colocamos nuestro equipo con server para descargar en la maquina victima el archivo de linpeas


[![escalacion.jpg](https://i.postimg.cc/xqrFMtYt/escalacion.jpg)](https://postimg.cc/1VHHZ08q)


Ejecutamos Linpeas y vemos que tenemos un binario por el cual podemos escalar privilegios 


[![escalacion3.jpg](https://i.postimg.cc/fWvKKzCM/escalacion3.jpg)](https://postimg.cc/V5dnsc2h)


Realizamos la descarga ⬇️ y le damos permisos de ejecución

[![escalacion2.jpg](https://i.postimg.cc/bYGy0rpt/escalacion2.jpg)](https://postimg.cc/c6SG0sKx)


Buscamos en la pagina GTFObins como escalar privilegios por php 


[![escalacion4.jpg](https://i.postimg.cc/d1wCkCzr/escalacion4.jpg)](https://postimg.cc/f3qy1VWR)


Escalamos como root ejecutando los comandos que se nos indica y vemos el contenido de la bandera 2 🚩

[![bandera2.jpg](https://i.postimg.cc/LsR5NpNM/bandera2.jpg)](https://postimg.cc/WdWs1xB9)


## 4.Banderas 🏁

|Bandera 1 | 19019f428f02d94f958b9f709732a51e |
|:--------:|:--------------------------------:|
|Bandera 2 | e3b9c48f529685a5fca3e8a5d7d27e0a |

