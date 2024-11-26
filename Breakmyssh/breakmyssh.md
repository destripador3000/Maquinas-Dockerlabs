Esta es una máquina catalogada como muy fácil en dockerlabs, lo primero que hacemos una vez descargada la máquina es descomprimir archivo y luego pasarle el argumento:
```
sudo su bash auto_deploy.sh breakmyssh.tar
```
Con este comando se activara la máquina en nuestro entorno de docker.
Luego hacemos un escaneo del objetivo con el comando:
```
nmap -p- -sV -sC -n -Pn -sS --min-rate 5000 172.17.0.2
```
Este comando utiliza los argumentos min rate: para que el escaneo vaya un poco más rápido El argumento -sS para que sea rápido y sutil. -sC es un argumento usado nmap para potenciar el escaneo. -p- para escanear todos los abiertos posibles en la ip. -sV para que saque la versión de los servicios corriendo en el objetivo. -n para que no haga la resolución DNS. -Pn para no hacerle un ping a la máquina objetivo.
![[escaneoBreakmyssh.png]]
Como se puede observar una vez hecho el escaneo se puede encontrar el puerto 22 abierto, en el que esta corriendo el servicio ssh.El cual tiene una versión antigua como openSSH 7.7 la cual tiene una vulneración de enumeración de usuarios así que utilizaremos un exploit para aprovecharnos de esta vulnerabilidad. Este exploit es el de la vulnerabilidad CVE-2018-15473. 
Para poder explotar esta vulnerabilidad podemos utilizar la herramienta searchsploit:

```
searchsploit OpenSSH 7.7
```
Ejecutando este comando nos daremos cuenta que esta versión del servicio ssh tiene una vulnerabilidad de enumeración de usuarios. utilizando el exploit de mestploit nos dara el usuario. El cual es "lovely" y utilizando hydra con el diccionario rockyou.txt nos daremos cuenta que la contraseña es "rockyou" con esto ya podremos ingresar a la máquina por el servicio ssh.
Una vez adentro con en el comando:
```
find / -perm -4000 -ls 2>/dev/null
```
podemos ver que archivos podemos ejecutar en la máquina, uno de ellos es el comando su. El cual al momento de ser ejecutado nos pide una contraseña. navegando por los distintos directorios, llegamos al directorio /opt el cual tiene un archivo oculto llamado .hash. Al pasar este archivo por el programa hash.com encontramos una contraseña "estrella", contraseña que podremos usar con el comando su y esta vez si nos encontraremos loggeados como root. Este sería el fin de la máquina breakmyssh.
