Está máquina es de dockerlabs, es una máquina catalogada como muy sencilla.
Primero se despliega el laboratorio en nuestra terminal kali.
Luego de eso pasamos a escanear nuestro objetivo:
```
nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn 172.17.0.2 
```
Este comando utiliza los argumentos min rate: para que el escaneo vaya un poco más rápido El argumento -sS para que sea rápido y sutil. -sC es un argumento usado nmap para potenciar el escaneo. -p- para escanear todos los abiertos posibles en la ip. -sV para que saque la versión de los servicios corriendo en el objetivo. -n para que no haga la resolución DNS. -vvv para poder observar cada que se encuentre un puerto abierto y no esperar a que acabe el escaneo. -Pn para no hacerle un ping a la máquina objetivo.
![[escaneoTRUST.png]]

Como se puede observar se encontrar dos puertos abiertos el 22 en el que corre el servicio ssh y el 80 en el que corre el servicio http. Podemos observar que el servicio ssh está un poco actualizado y no tenemos ninguna de las 2 credenciales necesarias para entrar en él y utilizar un poco de fuerza bruta para poder hallar la otra (Las credenciales necesarias son usuario y contraseña).
Teniendo en cuenta esta dificulta en el servicio ssh podemos, utilizaremos [[Fuzzing]] web para poder hallar archivo o directorios escondidos. El comando a usar es:
```
gobuster -u http:/172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,sh,py
```

Este comando utilizara la herramienta gobuster para recorrer una url con una lista de posibles archivos o directorios en ella, también se usa el comando -x con las extensiones de algunos archivos, de esta manera encontrara dichos archivos con estas extensiones.
![[gobusterTrust.png]]
como se puede observar una vez arrojado el ataque se encuentran 2 archivos con extensiones html y php respectivamente.

![[secretTrust.png]]
Este es el contenido de la extensión secret.php.
Bueno como podemos recordar anteriormente habíamos visto el puerto ssh abierto pero no teníamos ninguna credencial pues bueno aquí podemos ver en texto claro que el usuario Mario tiene acceso a esta máquina, es decir ya tenemos el usuario para proceder con un ataque de fuerza bruta con hydra.
![[ataquehydra.png]]
Como se puede observar al pasarle hydra con el argumento -l indicando que conocemos el usuario y el argumento -P indicando que no conocemos la contraseña y pasando el diccionario rockyou.txt nos damos cuenta que la contraseña para el usuario "mario" es chocolate.
![[sshEntrada.png]]
Como se puede observar al entrar por el servicio ssh con el usuario mario y posteriormente ingresando la contraseña logramos acceder a la máquina objetivo.
![[revisión.png]]
Con el comando Find y el argumento -perm señalando la raíz para ver que archivos podemos ejecutar como root podremos escalar privilegios pero se puede ver que no hay algún binario para ejecutar.
![[escaladaDepivilegios.png]]
con el comando sudo -l podemos ver que el usuario mario puede usar la consola de vim. Esto es una gran señal ya que agregando posteriormente el comando:
```
!/bin/bash
```
Podremos escalar privilegios y quedar como usuario root.
Con la escalada de privilegios ya somos super Usuarios y ya hemos acabado.


